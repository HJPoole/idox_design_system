# Idox Design System — Rationale

This document captures the reasoning behind design decisions in this system: what was decided, why, and what constraints each decision places on the interface. It is not a spec — token values and component rules live in [design.md](design.md). This document exists to prevent those rules from being eroded when decisions are revisited without context.

---

## Audience & intended response

The primary audience is public sector and local government professionals — case workers, planning officers, licensing administrators, and council staff. These users are task-focused: they are not browsing; they are completing work. They interact with these interfaces repeatedly, often daily, on fixed-position monitors in office environments. A subset of users will have accessibility requirements that must be met by law (Equality Act 2010, PSBAR 2018).

The intended response is **competence, trust, and ease** — the interface should feel reliable, fast to learn, and transparent in its behaviour. Delight and novelty are not goals. When a design choice feels too plain, that is often correct. When a design choice feels expressive or unexpected, it should be questioned.

---

## Decision log

### 1. Single typeface — DM Sans only

**What was decided:** The entire interface uses DM Sans exclusively. No secondary typeface is introduced at any scale or context. Monospace text (the `code` stack) is the only exception, restricted to `code` and `pre` elements and structured reference fields.

**Why:** DM Sans was selected for its legibility at small sizes — particularly in the 12–14px range used for labels, table cells, and helper text. It is a low-contrast humanist sans-serif with open apertures that render cleanly on standard office monitors and in Windows ClearType environments, which are the dominant display environment for the product's users. A dual-typeface approach would introduce visual complexity without improving legibility or hierarchy — the type scale and weight system (`400`, `500`, `600`, `700`) is sufficient to establish all necessary levels of information hierarchy.

**Constraint:** No second typeface may be introduced, including for headings, marketing copy, or special contexts. If hierarchy feels insufficient, the solution is a size or weight adjustment — not a font change.

**Validation:** To be confirmed — render testing on Windows ClearType at 100% zoom recommended before shipping.

---

### 2. Deep navy brand colour — blue.800 as the primary interactive signal

**What was decided:** The brand and primary interactive colour is `blue.800` (#0A1F8F) — a deep navy. All primary buttons, active states, selected states, and brand-coloured text reference this value or the adjacent interactive scale (blue.700 for hover, blue.900 for pressed). The secondary accent is `blue.600` (#195FD2).

**Why:** Deep navy blue is widely established as a signal of authority, institutional reliability, and public sector identity — the intended register for a system serving local government. It provides strong contrast on white surfaces (the system's primary component background), comfortably exceeding WCAG AA requirements at all applicable text sizes. The blue.800 / blue.700 / blue.900 interactive scale gives a clear three-step response (rest / hover / pressed) without departing from the brand hue, maintaining a visually coherent interactive language. Using a single hue family for interaction means users can reliably associate the colour with affordance — blue means "you can do something here."

**Constraint:** The brand blue must not be used decoratively — for section fills, illustration, or stylistic accents. Its sole function is to signal interactivity and brand identity. Any non-interactive use of blue risks training users to ignore its affordance signal.

**Validation:** To be confirmed — contrast ratios for all blue / white combinations to be checked in the design token audit before the first component library release.

---

### 3. Three-level content hierarchy — default, secondary, tertiary

**What was decided:** Text content uses three colour levels: `content-default` (slate.900), `content-secondary` (slate.600), and `content-tertiary` (slate.400). These are used strictly in hierarchy order — primary information, supporting information, and supplementary information respectively.

**Why:** A three-level hierarchy covers every content context in the product — headings and body copy at the primary level, metadata, subheadings, and field labels at the secondary level, placeholder text and timestamps at the tertiary level. A four- or five-level hierarchy would introduce ambiguity about which level to apply, leading to inconsistency across teams. Two levels would force either the conflation of supporting and supplementary content, reducing hierarchy clarity.

`content-tertiary` (slate.400 / #94A3B8) deliberately sits below the WCAG AA threshold for normal-weight body text. This is intentional: tertiary content is by definition non-critical, and its lower contrast signals that it is supplementary. It must never be used for text that a user needs to read to complete a task — helper text, error messages, and field labels are all secondary or primary.

**Constraint:** No additional content colour values should be introduced. If a new content tier is needed, map it to the nearest existing level. `content-tertiary` must never be used for interactive affordances, field labels, error messages, or any content that a user is required to read.

---

### 4. Rounded components — moderate radius as the default

**What was decided:** The system defines a nine-step border radius scale from `none` (0px) to `full` (9999px). Interactive components — inputs, buttons, and icon buttons — default to `rounded.lg` (8px). Structural containers — panels, cards, tables — use `rounded.xl` (12px). Badges and chips use `rounded.full`. The `none` and `sm` values are available but restricted to specialist layout and code contexts.

**Why:** Moderate rounding (8–12px) was chosen to position the system as modern and professional — distinct from legacy enterprise software aesthetics (which often use zero rounding) without trending toward consumer product aesthetics (which often use aggressive rounding). The choice aligns with the B2G (business-to-government) product category, where the audience expects professional software that is clearly distinct from consumer apps, but not archaic.

`rounded.lg` (8px) for interactive components and `rounded.xl` (12px) for containers creates a consistent size relationship between controls and the surfaces they sit within. The full range of values is defined in the token set to avoid future ad-hoc values being introduced when edge cases arise.

**Constraint:** Only values from the defined scale (`none`, `sm`, `default`, `md`, `lg`, `xl`, `2xl`, `3xl`, `full`) may be used. No value outside this set should be applied to any component. If a new component does not clearly fit an existing radius value, default to `rounded.lg` for interactive elements and `rounded.xl` for containers, and record the edge case here.

---

### 5. Structured shadow scale — elevation communicates layer, not decoration

**What was decided:** The system provides a six-level shadow scale (`sm` through `xl`, plus `inner` and `none`). Components are assigned shadow values based on their elevation context: inline components use `shadow-sm`, floating surfaces use `shadow-default` or `shadow-md`, overlays use `shadow-lg` or `shadow-xl`. Shadow is always paired with a visible border — not used as a replacement for it.

**Why:** In data-dense interfaces, elevation cues are functional — they tell users whether a surface is inline, floating, or modal. A flat (no-shadow) system would make dropdowns, date pickers, and modals harder to distinguish from the page surface, particularly on desktop monitors where the separation between content and interactive surface matters most. Conversely, heavy use of shadow creates visual clutter that competes with content. The structured scale ensures that elevation is applied with intention rather than style.

The decision to pair shadow with border (rather than use shadow alone) reflects a practical accessibility consideration: users who adjust system contrast settings may suppress or reduce shadow visibility. A visible border ensures the component boundary remains defined in all display environments.

**Constraint:** Shadow values must not be applied to decorative purposes — for visual interest, hover animations, or branding. Shadow signals elevation, not emphasis. If a component needs to stand out, the solution is colour, border weight, or typography — not a larger shadow.

---

### 6. Four-state status colour system — success, error, warning, neutral

**What was decided:** Three semantic status hues are defined — green (success), red (error), amber (warning) — each with a four-value pattern: `default` (filled action), `subtle` (alert background), `border` (alert outline), `content` (inline text). A neutral pattern using the slate scale covers non-status contextual information.

**Why:** Case management, planning, and licensing applications are fundamentally status-driven — records are approved, rejected, pending, expired, or flagged. A consistent, unambiguous status language across all products reduces the cognitive load of context-switching between applications in the Idox suite. A user moving from a planning application to a licensing application encounters the same semantic colour for the same meaning.

Warning uses `amber.500` as its default but `amber.800` as its content text colour — not white. This is a deliberate exception to the pattern used for success and error. At amber.500 fill, white text does not meet WCAG AA contrast requirements. Using amber.950 or amber.800 for text on amber.50 surfaces achieves the required contrast ratio while maintaining visual coherence within the amber family.

**Constraint:** Status colours must not be repurposed outside their semantic meaning. Red must not be used for branding, emphasis, or non-error states. Amber must not be used decoratively. If a product requires additional status states beyond success / error / warning / neutral, they must be mapped to an existing semantic group and the mapping documented here before implementation.

---

### 7. Two focus ring patterns — ring for inputs, outline for buttons

**What was decided:** The system uses two distinct focus ring implementations:
- Inputs, selects, and textareas: `focus:ring-1 focus:ring-focus focus:border-focus` — a 1px ring drawn inside the element boundary.
- Buttons, links, toolbar controls, and other interactive elements: `focus-visible:outline-solid focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-brand-accent` — a 2px outline drawn outside the element with 2px offset.

**Why:** Input fields already have a visible border that defines their shape. A ring that draws inside the border boundary integrates naturally with the existing border treatment — replacing it on focus rather than layering over it. A 2px offset outline, by contrast, would float outside the input and clash with adjacent form elements in dense grid layouts.

Buttons and interactive controls benefit from the offset outline because it creates a clear, unambiguous focus indicator that does not obscure the button's own fill, border, or label. The 2px offset creates breathing room that makes the focus state unmistakable without requiring the element to change its size or layout footprint.

Both patterns use WCAG 2.2 SC 2.4.11-compliant dimensions and contrast. The `focus-visible` selector (versus `focus`) ensures outline focus styles only appear on keyboard navigation — not on mouse click — reducing visual noise for pointer users.

**Constraint:** Do not unify the two patterns into a single implementation unless both input and button contexts are re-evaluated. The split exists for good visual reasons. Never suppress focus styles with `outline-none` on an interactive element unless a custom focus style is applied.

---

### 8. Semantic token layer — components reference semantics, never primitives

**What was decided:** The token system is two-tiered. Primitive tokens define the raw colour scale (blue.50 through blue.950, slate.50 through slate.950, etc.). Semantic tokens assign meaningful names to those values (`interactive-default`, `text-secondary`, `border-focus`, etc.). Components and styles always reference semantic tokens — never primitive values.

**Why:** Primitive values are not stable over time. As the product evolves, the specific hex value used for the interactive default may shift — perhaps to improve contrast, differentiate between product lines, or accommodate a brand refresh. If components reference `blue-800` directly, every such reference must be found and updated. If they reference `interactive-default`, the change is made in one place. This architecture also makes the intent of a colour application explicit: `interactive-default` tells a reader why the colour is there; `blue-800` does not.

The semantic layer also enables theming and product-line differentiation in the future. A second product could use a different primitive scale while preserving the same semantic structure — all component logic remains unchanged.

**Constraint:** No component implementation may reference a primitive token directly (e.g. `blue-800`, `slate-200`). All colour, opacity, and shadow values must pass through a semantic token. If a use case is not covered by an existing semantic token, the semantic layer should be extended — not bypassed.

---

### 9. Opacity for disabled states — preserve layout, communicate unavailability

**What was decided:** Disabled components use `opacity-50` applied over the base component styles, rather than replacing background and text colours with grey variants. The component retains its shape, size, and position in the layout — only its visual weight is reduced.

**Why:** Collapsing or replacing disabled components creates layout instability. When a field becomes enabled, the layout shifts — this is disorienting in form-heavy workflows where users are filling in records. Preserving the layout with a reduced-opacity treatment communicates unavailability without disrupting the spatial context. 50% opacity was chosen as a practical value that provides sufficient contrast reduction to signal non-interactivity while remaining distinguishable from removed content across a range of display environments.

**Constraint:** Disabled elements must never be removed from the DOM to hide them unless they are genuinely not applicable. If a field becomes unavailable depending on a prior selection, it should be visually disabled and remain in position — not hidden. Hiding creates a more confusing experience when the enabling condition is later met.

---

## What this means in practice

These decisions collectively produce a system with a small number of firm constraints:

- One typeface. DM Sans only. No exceptions except the code monospace stack.
- Blue is interactive. Any blue element signals that a user can act. Never use blue decoratively.
- Three text colour levels. `text-primary`, `text-secondary`, `text-tertiary` — in that order of importance. Tertiary is supplementary only.
- Status colours are semantic contracts. Green means success, red means error, amber means warning — across every product in the suite.
- Shadow and border work together. Shadow signals elevation; border signals boundary. Neither replaces the other.
- Components reference semantic tokens. Primitives are for the token definition file only.
- Disabled means visible and inert — not hidden, not collapsed.
- Secondary buttons use `border-strong`. Only primary buttons carry `border-brand-accent`.
- Inputs use `ring-focus` on focus. Buttons use `outline-brand-accent` with offset.

[design.md](design.md) contains the token values and component rules that implement these decisions. Changes to tokens should be traced back to a decision recorded here.

---

## Open questions & validation backlog

| Question | Priority | Method |
|---|---|---|
| Does `interactive-default` (#0A1F8F) meet WCAG AA on `surface-default` for all button label sizes, including `button-small` (12px semibold)? | High | Contrast checker — 4.5:1 required for text, 3:1 for large text |
| Does amber warning text (`warning-content` on `warning-subtle`) meet WCAG AA? | High | Contrast checker — confirm before shipping warning callout components |
| Does `text-tertiary` (slate.400) meet WCAG AA on `surface-default` for any text use case? | High | Confirm it does not — document the cases where it is and is not permissible |
| Does DM Sans render acceptably at 12px on Windows ClearType at 96 DPI? | High | Render test in Windows Chrome / Edge at 100% zoom before type scale sign-off |
| Do the shadow elevations provide sufficient layer distinction for floating components (dropdowns, date pickers) on `surface-base` backgrounds? | Medium | Visual QA pass on a form view using a panel-in-canvas layout |
| Does the three-level text hierarchy cover all content types in the planning and licensing products, or are edge cases (e.g. inline code, reference numbers) unaddressed? | Medium | Content audit across two existing product screens |
| Is `opacity-50` for disabled components distinguishable enough from enabled components on `surface-base` backgrounds without a separate disabled border treatment? | Low | Accessibility review with AT users |
| Should the left navigation support a collapsed/icon-only state, and if so, what token changes does that require? | Low | Information architecture review — out of scope for initial token release |
| Are the exact values for `surface-muted`, `border-subtle`, `list-hover-bg`, `list-active-bg`, and navbar tokens confirmed in design-tokens.css? | High | Cross-check token values in design-tokens.css against the values in design.md |
