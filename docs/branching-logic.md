# Branching Logic — Council Decisions & Design Reversals

Living record of non-trivial design forks. Each entry: what was decided, who deliberated, what threshold was met, what residual remains honest. When a decision is later reversed, the original entry is *not* deleted — a **REVERSED** marker is appended pointing to the superseding entry.

Decisions are listed in chronological order. The most recent is at the bottom.

Convention for reversals: append a block immediately under the original entry of the form:

```
> **REVERSED 2026-XX-XX** — see entry "[New title]" below. Reason: [one sentence].
```

---

## 2026-05-25 · Toolkit & deployment shape

**Decision:** Single-file `2test.html` with inline `<style>` and `<script>`. Zero runtime dependencies. No CDN, no fonts loaded over network, no build step, no backend, no telemetry. `/clinical-html-ui` and `/gui-design` skills will be invoked at the layout and visual-design phases respectively. `/web-artifacts-builder` was explicitly rejected — it assumes a build step or CDN-loaded runtime that violates the operational constraints.

**Council:** none required — operational constraints were specified, not deliberated.

**Residual:** if a future feature genuinely requires a third-party library, the constraint forces re-evaluation rather than silent CDN load. Acceptable.

---

## 2026-05-25 · Instrument interpretation strategy

**Options considered:**
- Raw entry only (clinician writes all prose)
- Auto-interpret with cited cutoffs (clinician verifies)
- Hybrid

**Decision:** **Hybrid.** Deterministic-cutoff instruments (SCQ, M-CHAT-R/F, SCARED, PHQ-9A, GAD-7, Vanderbilt, SNAP-IV, AQ-10, ASSQ, MDQ, CRAFFT, etc.) get auto-interpretation against published cutoffs cited in `references.md`. Judgment-heavy instruments (DP-4, ADOS-2, CARS-2-ST, KBIT-2R, RITA-T, DAYC-2, SRS-2, GARS-3) get raw entry only. Auto-interpretations are always editable; an `edited` flag persists in state and renders an "edited" badge in the preview.

**Council:** none — strongest time-saving-to-clinical-risk ratio was uncontested.

**Residual:** the editable-with-flag pattern preserves clinician authority but adds state-shape complexity (every auto-interp slot needs `{raw, generated, override, edited}`). Worth the complexity.

---

## 2026-05-25 · Persistence model

**Decision:** **Both.** localStorage auto-save (debounced ~400ms) for in-session recovery, plus an exportable session string (`2test:v1:<base64(gzip(JSON))>`) the clinician copies and stores wherever they want. PHI never auto-syncs anywhere; the clinician is the only persistence agent.

**Council:** none.

**Residual:** localStorage is per-browser-profile-per-origin. If the clinician uses the same browser at home, session-level state could persist across contexts. Mitigated by the de-identified-by-design decision (next entry) — there's no PHI in localStorage to leak.

---

## 2026-05-25 · Smallest target screen — mobile-first

**Decision:** Phone is the smallest supported screen (~360-400px wide). CSS-driven responsive layout with three breakpoints: phone (<768px) single-column with output overlay, tablet (768-1199) single-column with side drawer, desktop (≥1200) two-column split with persistent live preview.

**Council:** none.

**Residual:** side-by-side criteria-and-A&P (the DBP voice's "synthesis surface" preference) is impossible on phone. Mitigated by a sticky "Reference" affordance that pops criteria as an overlay on small screens.

---

## 2026-05-25 · DSM-5-TR criteria surface

**Decision:** Editable A/B/C/D/E checklist with a free-text "Additional reasoning" field appended. Checklist state feeds the A&P and the ABA letter renderers; free-text catches what the checkboxes can't structure.

**Council:** none — the hybrid was strictly dominant over either pure checklist (too rigid) or pure free-text (loses structure for letter generation).

**Residual:** if criteria evolve in future DSM revisions, the checkbox structure has to migrate. Schema version in state (`meta.schemaVersion`) signals this; migration logic added when needed.

---

## 2026-05-25 · PHI minimization — de-identified by design

**Decision:** The tool accepts no identifying patient data. Demographics are reduced to: age in months (single integer; displayed as months <36, years ≥36), sex, insurer category (Commercial / Medicaid / TRICARE-ChampVA), school status (categorical), current educational support (categorical). No name, DOB, MRN, address, parent names, school name, evaluator name, physician name. Letters render with bracketed placeholders (`[PATIENT NAME]`, `[DOB]`, `[INSURER ID #]`, `[SCHOOL NAME]`, `[EVALUATION DATE]`) the clinician fills in Epic post-paste.

**Council:** none — the PHI-posture and session-string-portability gains were uncontested once articulated.

**Residual:**
- Clinician can still type identifying info into free-text fields. Soft guardrail only (a one-line hint near each free-text area: *"PHI-free — use 'the child' or `[NAME]`."*). No regex scan — too false-positive-prone.
- The renderers cannot use the patient's name to personalize prose. For ABA letters, this is plausibly *more* defensible — clinical-content focus, not warmth. Documented as known tradeoff.

---

## 2026-05-25 · Intake scope discipline

**Decision:** **Synthesis-relevant only.** If a field doesn't appear in any of the three renderers (A&P, ABA, IEP), it does not belong on the form. Epic holds the chart-completeness data. Medical/family/social/medication sections collapse to focused checkboxes plus minimal free-text for ASD-relevant items. Screeners take raw totals (one number per scale), not item-by-item entry, except where the instrument's structure requires items (e.g., SCQ lifetime-vs-current distinction).

**Council:** none.

**Residual:** if a clinician decides they want a field for their own record-keeping that doesn't drive output, the answer is "use Epic for that." This is intentional and documented.

---

## 2026-05-25 · ABA letter audience selector

**Decision:** Three options — **Florida Medicaid · TRICARE/ChampVA · Generic commercial.** Defaults from the insurer category demographic field; clinician can override per letter.

**Council:** none.

**Residual:** if a payer-specific letter is later needed (e.g., a particular commercial plan with an idiosyncratic medical-necessity vocabulary), a fourth template is added. Additive change.

---

## 2026-05-25 · Letter signature & header

**Decision:** Prose body only. No signature line, no NPI, no clinic header, no salutation closer. The clinician handles all letterhead and signing in Epic.

**Council:** none.

**Residual:** none.

---

## 2026-05-25 · IEP letter modeling — N templates vs single conditional template

**Options:**
- (A) N discrete templates with a router function keyed on `(school.status, school.currentSupport)`, plus a small set of shared paragraph helpers for content that's situation-invariant.
- (B) Single template with conditional sections.

**Council members & round-1 ratings:**

| Voice                       | A    | B    | Falsifier for A                                                                 |
|-----------------------------|------|------|----------------------------------------------------------------------------------|
| Clinical content owner      | 92   | 65   | Templates share >70% verbatim text → B wins on DRY                              |
| Maintenance engineer        | 88   | 55   | >3-4 templates need a shared snippet with no extracted helper → drift           |
| Workflow analyst            | 90   | 60   | Clinicians want cross-template paragraph mixing → discrete model becomes friction|
| Audit / documentation lead  | 96   | 50   | Templates not named clearly → audit value collapses                              |

Round-1 mean for A: **91.5.** Below the ≥95% threshold. The drag was the shared-prose concern (three voices flagged it).

**Mitigation introduced round 2:** extract a small set of pure paragraph helpers — `functionalImpairmentParagraph(S)`, `clinicalJustificationParagraph(S)`, `signOffParagraph(S)` — that templates call.

**Round-2 ratings for A with mitigation:** Clinical 96 · Engineering 95 · Workflow 92 · Audit 97. **Mean 95.0.** Threshold met.

**Decision: Option A — N discrete templates with router and paragraph helpers.**

**Honest residuals:**
1. If helper count grows past ~5, the abstraction may cost more than it saves. Revisit at that point.
2. Cross-template recombination (clinician wants the Early-Steps framing for a school-age scenario) is genuinely unaddressed. Workaround: clinician edits the rendered output before pasting into Epic. Document as a known limitation in `README.md`.

**Template inventory (initial set):**
- `iep_under3_none` — Early Steps referral
- `iep_under3_earlysteps` — additional services request within Early Steps
- `iep_3to5_none` — VPK/FDLRS/ESE Pre-K referral
- `iep_3to5_vpkese` — amendment of existing 3-5 services
- `iep_schoolage_none` — Child Find evaluation request (IDEA Part B)
- `iep_schoolage_504adequate` — sentinel "no letter needed"
- `iep_schoolage_504upgrade` — request reevaluation for IEP eligibility
- `iep_schoolage_iepadequate` — sentinel "no letter needed"
- `iep_schoolage_iepinadequate` — IEP amendment request, IDEA §300.324

(Private school collapsed into the school-age categories; IDEA-binding language replaced with Section 504 / ADA Title III language at the renderer level where applicable.)

---

## How to add a new entry

When a non-trivial decision is made, append a section with:
- ISO date heading
- Options considered (if more than one)
- Decision
- Council (members, ratings with falsifiers, threshold met, or "none" with reason)
- Honest residual(s)

When a decision is reversed, append the REVERSED marker under the original entry and add the new decision as a new entry below.
