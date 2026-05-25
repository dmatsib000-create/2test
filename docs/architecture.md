# Architecture — 2test.html

This file specifies the shape of the application before any HTML is written. Treat it as the contract that `2test.html` implements. When the implementation diverges, update this file *in the same commit* and explain why in the commit message.

---

## Delivery shape

One file: `2test.html`. Inline `<style>` and `<script>`. No build step, no bundler, no transpile, no npm install. The clinician downloads the raw file from GitHub (`dmatsib000-create/2test`), saves it locally (OneDrive, USB, anywhere), and double-clicks it to open in their default browser. It works offline forever. No network requests on any code path.

System font stack only: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif`. No `@font-face` declarations, no external font references.

---

## Visual design system

See [`docs/design-system.md`](design-system.md) for the full visual contract: the *Examining Light* philosophy, the color/type/spacing/motion tokens, the layout breakpoints, the component rules, and the two signature details (lit-section bar and cooling save dot). That file is the authority for everything inside `<style>` in `2test.html`. Update it in the same commit as any token change.

## File layout in the repo

```
2test.html              ← the entire application
docs/
  references.md         ← bibliography + verification ledger
  branching-logic.md    ← council decisions, reversals, residuals
  architecture.md       ← this file
  design-system.md      ← visual tokens, philosophy, component rules
CHANGELOG.md            ← human-readable why-not-what entries (added when first feature ships)
README.md               ← clinician-facing usage instructions
```

Nothing under `docs/` is loaded at runtime. It exists for the audit trail and for future-maintainer onboarding.

---

## State model (`S`)

Single source of truth, one object, mutated only through binding from form inputs and rendered only through pure functions. No DOM-as-state.

```
S = {
  meta: {
    schemaVersion: 1,
    sessionId:     <uuid-v4 generated on first save>,
    savedAt:       <ISO timestamp>
  },

  demographics: {
    ageMonths:        <integer 12-264, covers 1y-22y>,
    sex:              "male" | "female" | "intersex" | "unspecified",
    insurerCategory:  "commercial" | "medicaid" | "tricare-champva"
  },

  school: {
    status:          "under3" | "age3to5_none" | "age3to5_program"
                   | "schoolage_public" | "schoolage_charter"
                   | "schoolage_private" | "schoolage_homeschool"
                   | "schoolage_notenrolled",
    currentSupport:  "none" | "504_adequate" | "504_upgrade"
                   | "iep_adequate" | "iep_inadequate"
                   | "early_steps" | "vpk_ese_prek"
  },

  concerns: {
    chief:           <text>,
    onset:           <text>,
    progression:     <text>
  },

  history: {
    developmental:   { milestones: {...}, regression: bool, regressionDetail: <text> },
    medical:         { perinatal: bool, seizures: bool, gi: bool, sleep: bool,
                       feeding: bool, otherRelevant: <text> },
    family:          { asd: bool, idGdd: bool, adhd: bool, psych: bool,
                       geneticSyndrome: bool, otherRelevant: <text> },
    social:          { livesWith: <enum>, primaryLanguage: <text>,
                       caregiverSetting: <enum> },
    medications:     { onStimulant: bool, onSsri: bool, onAntipsychotic: bool,
                       onAlpha2: bool, otherRelevant: <text> }
  },

  observations: {
    appearance:      <text>,
    interaction:     <text>,
    communication:   <text>,
    play:            <text>,
    behavior:        <text>,
    additional:      <text>
  },

  instruments: {
    // Each slot follows the same shape:
    // {
    //   used: bool,
    //   raw: <number | structured object>,
    //   generated: <string | null>,       // auto-interp output (null if judgment-heavy)
    //   override: <string | null>,         // clinician's edited text
    //   edited: bool                       // true if override differs from generated
    // }
    scq:         {...},
    mchat_rf:    {...},
    swyc:        {...},
    promis_ec:   {...},
    promis_pp:   {...},
    promis_ped:  {...},
    asq3:        {...},
    asq_se2:     {...},
    aq10:        {...},
    srs2:        {...},
    assq:        {...},
    gars3:       {...},
    phq9a:       {...},
    nih_asq:     {...},
    mfq:         {...},
    pas:         {...},
    scas:        {...},
    scared:      {...},
    gad7:        {...},
    mdq:         {...},
    pq16:        {...},
    crafft_n:    {...},
    vanderbilt:  {...},
    snap_iv:     {...},
    adhd_rs_iv_preschool: {...},
    dp4:         {...},
    kbit2r:      {...},
    rita_t:      {...},
    cars2_st:    {...},
    ados2:       {...},
    dayc2:       {...}
  },

  dsm5tr: {
    a: { a1: bool, a2: bool, a3: bool, evidence: <text> },
    b: { b1: bool, b2: bool, b3: bool, b4: bool, evidence: <text> },
    c: bool,
    d: bool,
    e: bool,
    additionalReasoning: <text>
  },

  specifiers: {
    withId:           "yes" | "suspected" | "no" | "unknown",
    withGdd:          "yes" | "suspected" | "no" | "n/a",
    withLanguage:     "yes" | "no" | "unknown",
    withMedical:      "yes" | "no",
    severitySC:       1 | 2 | 3 | null,
    severityRRB:      1 | 2 | 3 | null
  },

  differentials: [
    // { dx: <text>, considered: bool, rationale: <text> }
  ],

  letters: {
    aba: {
      audience:         "florida_medicaid" | "tricare_champva" | "generic_commercial"
                        // defaults from demographics.insurerCategory; override-able
    },
    iep: {
      // template selection is derived from school.status × school.currentSupport
      // no fields here unless future per-letter overrides are needed
    }
  }
}
```

`schemaVersion` is incremented when the state shape changes in a way the session-string codec can't transparently rehydrate. Migration logic lives next to the codec.

---

## Render pipeline

Three pure render functions, each `(S) → string` returning Epic-safe HTML:

- `renderAP(S)` — Assessment and Plan paragraph for the SOAP note body
- `renderABA(S)` — ABA medical-necessity letter, branching on `S.letters.aba.audience`
- `renderIEP(S)` — IEP / school document, dispatched through a router (next section)

Each render function:
- Reads `S`, mutates nothing.
- Returns a string of HTML using only Epic-safe tags: `<p>`, `<strong>`, `<em>`, `<ul>`, `<li>`, `<br>`.
- Emits bracketed placeholders for identifiers: `[PATIENT NAME]`, `[DOB]`, `[INSURER ID #]`, `[SCHOOL NAME]`, `[EVALUATION DATE]`.
- Carries a header comment naming the `references.md` keys it cites.

The preview panel re-renders on every state change (debounced ~150ms). The copy target is a hidden `contenteditable` mirroring the preview; the Copy button writes both `text/html` and `text/plain` flavors via `navigator.clipboard.write` for Epic-paste fidelity.

---

## IEP router & templates

```
function renderIEP(S) {
  const key = `${S.school.status}__${S.school.currentSupport}`;
  const template = IEP_TEMPLATES[key] ?? IEP_TEMPLATES.fallback;
  return template(S);
}
```

Initial template inventory (each is a pure function `(S) → string`):

| Key                                          | Purpose                                                   | Cites                                  |
|----------------------------------------------|-----------------------------------------------------------|----------------------------------------|
| `under3__none`                               | Early Steps referral                                      | idea-part-c, fla-early-steps           |
| `under3__early_steps`                        | Additional services request within Early Steps             | idea-part-c, fla-early-steps           |
| `age3to5_none__none`                         | VPK / FDLRS / ESE Pre-K referral                          | idea-part-b, fla-beess                 |
| `age3to5_program__vpk_ese_prek`              | Amendment of existing 3-5 services                        | idea-part-b, fla-beess                 |
| `schoolage_public__none` (and charter)       | Child Find evaluation request                              | idea-part-b §300.111                   |
| `schoolage_public__504_adequate`             | Sentinel: "no letter needed — current 504 adequate"        | —                                      |
| `schoolage_public__504_upgrade`              | Request reevaluation for IEP eligibility                  | idea-part-b, section-504               |
| `schoolage_public__iep_adequate`             | Sentinel: "no letter needed — current IEP adequate"        | —                                      |
| `schoolage_public__iep_inadequate`           | IEP amendment request                                      | idea-part-b §300.324                   |
| `schoolage_private__*`                       | Accommodations request under Section 504 / ADA Title III  | section-504, ada-title-iii             |
| `schoolage_homeschool__*`                    | District consultation under Florida homeschool ESE        | fla-beess                              |
| `schoolage_notenrolled__*`                   | School re-entry / district consultation                   | idea-part-b, fla-beess                 |
| `fallback`                                   | "No template matched — please report this combination."   | —                                      |

Shared paragraph helpers (called from multiple templates):
- `functionalImpairmentParagraph(S)` — synthesizes adaptive deficits & functional impacts from `S.instruments`, `S.observations`, `S.dsm5tr`.
- `clinicalJustificationParagraph(S)` — connects diagnostic findings to educational implications.
- `signOffParagraph(S)` — common closing language (no signature line; just the prose closer).

When the helper count exceeds 5, revisit whether the abstraction is paying for itself (per `branching-logic.md` IEP-modeling residual).

---

## Session-string codec

```
encode:  S → JSON.stringify → CompressionStream('gzip') → base64 → "2test:v1:<payload>"
decode:  "2test:v1:<payload>" → strip prefix → base64-decode → DecompressionStream('gzip') → JSON.parse → S
```

If `CompressionStream` is unavailable (older institutional Chromium), fall back to `"2test:v1u:<base64(JSON)>"` (the `u` denoting uncompressed). The decoder handles both.

Schema-version mismatch behavior: if the incoming version is older than current, run sequential migration functions (`migrate_1_to_2`, etc.). If newer, refuse with a clinician-readable error ("This session string was created in a newer version of the tool. Update your copy of 2test.html.").

---

## Persistence

- **localStorage key:** `2test:session:current`. Writes are debounced to ~400ms after the last input. A small "Saved Xs ago" stamp in the header reflects the last successful write.
- **"New patient" button:** double-confirms, then clears localStorage and resets `S` to defaults. The current state is offered as an Export-string download before the clear, so accidental clears are recoverable.
- **PHI posture:** by design, no PHI is ever in `S`, so localStorage and session strings carry no PHI. This is a load-bearing assumption — see `branching-logic.md` "PHI minimization — de-identified by design."

---

## Layout breakpoints

| Breakpoint  | Width          | Layout                                                                                  |
|-------------|----------------|------------------------------------------------------------------------------------------|
| Phone       | < 768 px       | Single column scroll. Sticky top bar: section dropdown + "View Output" overlay button.   |
| Tablet      | 768 - 1199 px  | Single column. Sticky left section nav. Output is a slide-out drawer from the right.     |
| Desktop     | ≥ 1200 px      | Two columns: input ~60% left (with sticky section nav), output ~40% right always visible.|

All transitions are CSS-driven (`@media` queries). No JS-controlled responsiveness.

Touch targets ≥44×44 px on phone and tablet. Keyboard navigation works without exception. Focus order matches visual order. Color is never the sole signal — every status indicator carries a text or icon companion. Targets WCAG 2.2 AA.

---

## Auto-interpretation engine

```
SCORING = {
  scq:       { interpret: (raw) => ({ text, cite, band }), cite: 'scq-rutter-2003' },
  scared:    { interpret: (raw) => ({ text, cite, band }), cite: 'scared-birmaher-1999' },
  // ... per instrument with a deterministic cutoff
}
```

For instruments listed in the hybrid-set (see `branching-logic.md` "Instrument interpretation strategy"), `interpret(raw)` returns `{ text, cite, band }`. For judgment-heavy instruments, the slot has no entry in `SCORING` — the UI shows raw entry + a free-text prose field.

Every generated `text` is editable. When the clinician edits, the override is stored in `S.instruments.<key>.override` and `S.instruments.<key>.edited` flips to true. The preview shows an "edited" badge so the audit trail is honest.

---

## Comment conventions

- Do not restate what the code does. Explain *why* when the why is non-obvious.
- `// WARY:` marks fragile or under-trusted code (distinct from `TODO` and `FIXME`).
- Intentional non-features are commented inline (e.g., "this is add-only by design — do not introduce removal logic").
- Non-obvious CSS values get a comment explaining why the obvious alternative doesn't work (e.g., `display: contents` is intentional and `display: ''` will break the focus order).
- Every renderer function header carries: a one-line "why this exists" and the list of `references.md` keys it cites.

---

## Out of scope for v1

The following are explicitly *not* in v1. If they arrive, they get a council and an entry in `branching-logic.md` before implementation.

- Multi-patient session list.
- Any PHI fields (name, DOB, MRN, address, school name, parent names, evaluator name, physician name).
- Network calls of any kind, including font loads or telemetry.
- Pre-population from Epic (no integration; would require backend).
- PDF export. Letters are prose-paste-only.
- Auto-detection of identifying info in free-text inputs (the soft hint is the only guardrail).
- Pediatric instruments not on the clinician's list. Adding instruments is additive; doing so requires a new entry in `references.md`.
- Languages other than English in the rendered output. The intake can accept the primary-language field for documentation, but the letters render in English.
