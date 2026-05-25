# Design System — 2test.html

The visual contract that `2test.html` implements. Update this file in the same commit any time `<style>` tokens, type scale, layout breakpoints, or signature details change in the HTML. The philosophy section is durable; the tokens evolve.

---

## Philosophy: Examining Light

The aesthetic is the warmth of a pediatric exam room with good natural light — not the fluorescent sterility of a corporate clinical dashboard, and not the soft-toy primary colors of "kid-friendly software." This tool is used by a doctor doing intellectual work between interruptions; the visual register honors both the seriousness of the synthesis and the humanity of the context. The feeling on page load should be the same as picking up a well-worn fountain pen on a paper-strewn worktable: ready, capable, deferential to the work. Nothing decorative competes with the clinical content. Nothing childish undercuts the gravity of an autism diagnosis. Painstakingly calibrated to feel like a personal instrument rather than enterprise software.

**Color and material.** A warm off-white background (`#FBF8F3`, paper under a desk lamp), with pure white reserved for the active editing surface — the lit page. Ink is a deep warm brown-black (`#1F1B16`), never pure black; warmth in the type carries respect for the reader. One saturated accent — a deep amber (`#B8730E`) — used exclusively for interactive affordances (focus rings, active section markers, the live preview "live" indicator). Critical states are brick (`#8B2E1F`), not screaming red; success is sage (`#5A6E3A`), calm, never bright. Total palette: 7 semantic values + 4 soft variants. Surface depth comes from a single 1px warm-gray rule, not shadows.

**Typography.** A three-tier system mirroring the document's content layers, all sourced from the system font stack because the no-CDN constraint is non-negotiable and because system fonts on modern OSes are genuinely beautiful when used with discipline. Section headers and the live-preview prose are set in a serif stack — serifs are vanishingly rare in clinical software and carry document gravitas. Form labels, inputs, and UI chrome use the system sans. Numeric data — ages, scores, IDs, timestamps — uses monospace for digit alignment and data-display authority. Letter-spacing on small-caps labels, optical sizing in the serif at the largest scales, a body line-height of 1.55 — the kind of polish that only comes from obsessive iteration.

**Motion and state.** Every interaction is acknowledged with a swift, satisfying micro-response — 150ms for chevron rotations and checkbox transitions, 200ms for panel state changes, immediate for keyboard input. No animation theater. The focus ring is a 3px amber outline at 2px offset, applied with `:focus-visible` so mouse users don't see it but keyboard users always do. All motion guarded by `prefers-reduced-motion`. Hover states change more than opacity — they shift the border weight or add an underline, never just dim.

**Spatial logic.** A strict 4px base grid governs every spacing decision. Density is medium-high: this clinician is an expert who lives in the tool. But every interactive element is still 44×44 — density never compromises touch target. The desktop layout uses a 60/40 split (input/preview); the gutter between them is exactly 32px (`--sp-8`) and that value appears nowhere else in the layout, which means it reads visually as a structural boundary, not a generic gap.

**Signature details.** Two, both reinforcing the *examining light* metaphor:

1. **Lit-section bar.** When a section in the form has focus or contains the most recently edited field, a 3px-wide amber bar appears on its left edge, fading from full saturation at the section header down through the section body — light falling from above. On mobile this is the only "where you're working" cue and is instantly glanceable from arm's length.

2. **Cooling save dot.** A 6px circle next to the "Saved Xs ago" timestamp interpolates from `--accent` (just saved) to `--ink-faint` (>30s ago) via a CSS custom property `--save-age-pct` that JS updates every second. Save freshness visualized as a literal cooling lamp.

---

## Tokens (canonical reference)

All defined as CSS custom properties at `:root` in `2test.html`. Inline-only — no separate stylesheet.

### Color

| Token            | Value     | Used for                                                |
|------------------|-----------|----------------------------------------------------------|
| `--bg`           | `#FBF8F3` | App background (warm off-white, paper)                  |
| `--surface`      | `#FFFFFF` | Active editing surface, output region                   |
| `--surface-sunk` | `#F2EEE6` | Subtle inset surfaces (instrument cells, phase chips)   |
| `--ink`          | `#1F1B16` | Primary text                                            |
| `--ink-muted`    | `#6B6359` | Secondary text, labels                                  |
| `--ink-faint`    | `#A39B8E` | Disabled state, placeholder text, cool save dot         |
| `--rule`         | `#E5DFD3` | 1px separators                                          |
| `--accent`       | `#B8730E` | Focus, active section bar, "live" indicator, primary CTA|
| `--accent-soft`  | `#F4E4C8` | Active-section tint, placeholder highlight              |
| `--critical`     | `#8B2E1F` | Errors, PHI warnings                                    |
| `--critical-soft`| `#F3DCD6` | Error backgrounds                                       |
| `--success`      | `#5A6E3A` | "Saved", "complete", successful copy fill               |
| `--success-soft` | `#E2E8D3` | Success backgrounds                                     |

Adding a 14th color is a sign of drift. Reach for an existing variable first.

### Typography

| Token           | Stack                                                                                |
|-----------------|---------------------------------------------------------------------------------------|
| `--font-display`| `'Iowan Old Style', 'Hoefler Text', Cambria, Georgia, serif`                          |
| `--font-body`   | `-apple-system, BlinkMacSystemFont, 'Segoe UI', 'Helvetica Neue', Arial, sans-serif`  |
| `--font-mono`   | `'SF Mono', SFMono-Regular, Menlo, Consolas, 'Liberation Mono', monospace`            |

### Type scale (fixed sizes — clinical use demands predictability)

| Token          | px  | Used for                                                |
|----------------|-----|----------------------------------------------------------|
| `--ts-micro`   | 11  | Phase chips, status pills, save timestamp (uppercase, `0.04em` letter-spacing) |
| `--ts-label`   | 13  | Form labels, helper text, hints                          |
| `--ts-body`    | 16  | Inputs, body prose, live preview body text               |
| `--ts-body-lg` | 18  | Section intro paragraphs, key clinical statements        |
| `--ts-subhead` | 22  | Subsection headers (serif)                               |
| `--ts-section` | 28  | Section headers (serif)                                  |
| `--ts-page`    | 34  | Application title only                                   |

Line-heights: 1.4 for headers, 1.55 for body, 1.3 for monospace. Letter-spacing: `0.04em` uppercase on micro labels, `-0.01em` on largest serif sizes for optical correctness.

### Spacing (4px base)

`--sp-1` 4 · `--sp-2` 8 · `--sp-3` 12 · `--sp-4` 16 · `--sp-5` 20 · `--sp-6` 24 · `--sp-8` 32 · `--sp-12` 48 · `--sp-16` 64

`--sp-8` is reserved for the desktop input/preview gutter and appears nowhere else. That exclusivity is what makes it read structurally.

### Radii

`--r-sm` 3px (inputs, buttons) · `--r-md` 6px (sections, modals) · `--r-full` 999px (pills)

### Motion

`--dur-micro` 100ms · `--dur-fast` 150ms · `--dur-state` 200ms · `--ease-out` `cubic-bezier(0.2, 0.8, 0.2, 1)`

All transitions wrapped in `@media (prefers-reduced-motion: no-preference)`.

### Focus ring

```css
:focus-visible {
  outline: 3px solid var(--accent);
  outline-offset: 2px;
  border-radius: var(--r-sm);
}
```

Single global rule. No per-component overrides. `:focus-visible` (not `:focus`) so mouse users don't see it.

---

## Layout breakpoints

| Breakpoint  | Width          | Behavior                                                                                  |
|-------------|----------------|--------------------------------------------------------------------------------------------|
| Phone       | < 768 px       | Single column scroll. Sticky top bar: native `<select>` jump nav + "View Output" overlay.  |
| Tablet      | 768 – 1199 px  | Single column with sticky left nav rail; output as right-side drawer.                       |
| Desktop     | ≥ 1200 px      | CSS Grid: `60fr var(--sp-8) 40fr`. Input column 1, gutter column 2 (empty), preview col 3. |

CSS-driven only. No JS-controlled responsiveness.

---

## Component-level rules

### Form group

- `flex column, gap var(--sp-2), margin-bottom var(--sp-5)`
- Label: serif `--font-display`, `--ts-label`, weight 600, color `--ink-muted`, **small-caps via uppercase + `0.04em` letter-spacing** — the typographic signature that says "field label" everywhere consistently
- Input: `min-height 44px`, padding `10px 12px`, border `1px solid --rule`, radius `--r-sm`, font `--font-body` at `--ts-body`
- Helper text below input: `--ts-label`, `--ink-muted`
- PHI-hint text: `--critical`, italic, with bracketed examples in `--font-mono`

### Section (native `<details>`)

- Border `1px solid --rule`, radius `--r-md`, background `--surface`
- Section header: serif `--ts-subhead`, weight 600, custom chevron via `::before` rotating 90° on open
- Phase chip right-aligned: `--ts-micro`, uppercase, `--surface-sunk` background, `--ink-muted` text, `--r-full` radius
- **Lit-section bar** (signature detail): `.is-lit` class triggers a `::after` pseudo with `linear-gradient(to bottom, var(--accent), transparent)` 3px wide on the left edge

### Output region (Epic-paste-critical)

- `width: 100%; max-width: none` — must reflow into any container Epic offers
- No transforms, no `user-select` restrictions, no fixed pixel width
- Only Epic-safe HTML inside: `<p>`, `<strong>`, `<em>`, `<ul>`, `<li>`, `<br>`, `<mark>`
- Background `--surface`, padding `--sp-5`, serif typography (live-preview prose)

### Placeholder highlight

- `.ph` on `<mark>` elements wrapping bracketed placeholders like `[PATIENT NAME]`
- `--accent-soft` background, italic, monospace, padding `0 4px`, radius `--r-sm`
- Brackets + italic + mono give colorblind-safe second/third differentiators
- Print: 1px border for background-stripping printers

### Copy bar

- Sticky bottom of preview region
- Primary button: `--accent` background, white text, `min-height 44px`, weight 600
- After successful copy: 600ms `--success` fill from bottom-up (guarded by reduced-motion)
- `aria-live="polite"` status region announces "Copied to clipboard"
- `@media print { display: none }`

### Save status

- Monospace `--ts-micro`, uppercase, `--ink-muted`
- 6px **cooling dot** (signature detail) — `background: color-mix(in srgb, var(--accent) calc((100 - var(--save-age-pct)) * 1%), var(--ink-faint))`
- JS writes `--save-age-pct` 0→100 over 30 seconds since last save
- Reduced-motion: discrete amber/gray steps, no smooth interpolation

### Modal

- Per `/clinical-html-ui` Pattern #6 — CSS-toggled wrapper, focus trap, Escape closes, focus returns to trigger
- Not native `<dialog>` (institutional Chromium variance)
- Backdrop via `::before` pseudo, no extra DOM node

---

## What's intentionally NOT in this system

- No icon library (CDN trap; CSS/unicode does the job)
- No dark mode in v1 (would require re-verifying every contrast pair under a council)
- No drop shadows on cards (the 1px rule does separation work)
- No corporate-clinical blue (deliberate departure from the medical-software default)
- No animation beyond the four specified above (chevron rotate, copy fill, save cool, lit-section appearance)
- No purple, no teal, no gradient against white — the AI-default aesthetic is explicitly out

---

## Adding a new token or component

1. Check whether an existing token covers the case. Almost always: yes.
2. If genuinely new, propose the addition in a council if it crosses a semantic boundary (e.g., a fourth status color). Otherwise just add.
3. Update this file in the same commit as the HTML change. Drift here is the worst kind.
