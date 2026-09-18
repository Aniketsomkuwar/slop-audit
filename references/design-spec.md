# Design Spec — Construction Workflow, Tokens, and Pre-Emit Gate

The pattern catalog (`references/slop-patterns.md`) says what not to build. This file says
how to build: the staged workflow that stops a model from reaching for the statistical
default, the token tables that make motion, colour, and type decisions before any component
exists, and the gate that refuses delivery when a check fails.

Both skills carry this file byte-identical. `slop-design` reads it *before*
generating; `slop-audit` reads it when remediating.

---

## Why interfaces come out generic

Ask an agent to "make it beautiful" or "make it premium" and it reaches for the most
common pattern in its training data: centred hero, purple-blue gradient, BETA badge, three
identical feature cards, Inter everywhere, soft shadow on every container,
`transition: all`. Clean enough to pass a glance, and not designed.

The failure is not visual talent. It is that a vague prompt grants the model permission to
default. Constraints are what remove the default. The workflow below is the constraint
apparatus — run the stages in order and most catalog patterns never get emitted, which is
cheaper than detecting and repairing them afterwards.

---

## Stage 1 — Brief (no code yet)

Before writing any markup, fix these seven decisions. Record them in
`.slop-tracker/design-brief.md` so later scans can diff the output against the intent.

| Decision | Values | Drives |
|---|---|---|
| Mode | brand-first / product-first | Everything below. A landing page may be expressive; a dashboard is efficient |
| Audience | who, in what context, on what device | Density, type size, contrast floor, touch targets |
| Mood | 3 adjectives, specific ("archive, precise, quiet" — not "modern, clean, premium") | Type choice, colour chroma, motion intensity |
| Visual density | airy / balanced / dense | Spacing scale, information per viewport |
| Visual variance | restrained / varied / expressive | How far sections may diverge from each other |
| Motion intensity | none / functional / expressive | Duration and easing budgets |
| Banned list | project-specific forbidden patterns | Reported against on every scan |

**Design Read.** Before the table, state the read in one line, out loud:

> Reading this as: \<page kind> for \<audience>, with a \<vibe> language, leaning toward
> \<design system or aesthetic family>.

Ask one clarifying question only if the read genuinely diverges from what the brief implies.
The read exists so a wrong assumption is corrected in one sentence rather than after a build.

**The three dials.** Density, variance, and motion intensity are not adjectives — put each on
a 1-10 scale and hold it for the whole build. The baseline is **variance 8 / motion 6 /
density 4**. A dashboard pulls variance down and density up; a portfolio pushes variance up.
The value matters less than committing to one and letting every later decision follow from it.

**Anti-default discipline.** A dial set high is not permission to reach for the loudest
default. Variance 8 means the sections genuinely differ from each other, not that every
section is decorated.

Then answer the one question that catches most generic output: **what is the single visual
idea this interface is remembered by?** If the answer is "a card grid", the brief is not
finished.

**Scope gate.** If the target is luxury hospitality, fine dining, or a destination venue,
the H1-H18 contract applies (see `references/luxury-hospitality-protocol.md`) and its step
order supersedes: truth → canvas → page archetype → media → structure → type → indicators
→ content presence. Verify every spatial claim against the brief *first*; a beautiful site
that lies about the building is the one defect no redesign recovers from.

### Generic layout reflexes to ban by default

Blocking these at brief time is worth more than detecting them later:

- Centred hero + purple-blue gradient + badge + three-card row
- Fake dashboard mockups or floating icon pills as hero art
- Decorative badges ("BETA", "AI POWERED") carrying no information
- `transition: all`, uniform 300ms `ease-in-out`, default `ease`
- Card-per-thing dashboards; three identical pricing/feature cards
- Symmetrical 50/50 split repeated down the page
- Inter/Roboto/Open Sans chosen as an unexamined identity default

---

## Stage 2 — Constraints (tokens before components)

Define the token layer first. Components built before tokens inherit ad-hoc values, and
that is D1-D6 drift by construction.

### Motion tokens

Motion is a budget. Frequency decides the spend — that single rule prevents most M-class
findings.

| Interaction class | Frequency | Duration | Curve |
|---|---|---|---|
| Keyboard shortcut, search, filter, tab switch, row hover | very high | 0–100ms, or none | — |
| Button press / active feedback | high | 80–120ms | `cubic-bezier(0.23, 1, 0.32, 1)` |
| Hover, focus, small state change | medium | 120–180ms | `cubic-bezier(0.23, 1, 0.32, 1)` |
| Enter / exit of a small surface (tooltip, menu, popover) | medium | 150–250ms | ease-out on enter, ease-in on exit |
| Larger surface or layout transition (sheet, dialog, page region) | low | 250–400ms | `cubic-bezier(0.77, 0, 0.175, 1)` |
| Rare, deliberate moment (success, reveal of the anchor visual) | rare | up to ~600ms | custom, authored for the moment |

Rules that hold across the table:

- Animate **transform and opacity**. Not width, height, padding, margin, top, left (M2).
- Name the properties. Never `transition: all` (M9).
- Entrances decelerate. `ease-in` on an entrance is a defect (M11).
- A page can fail on duration (M5) or on curve (M13) independently — set both tokens.
- Amplitude scales with distance and mass. A 4px shift and a full-panel slide do not share
  a duration.
- `prefers-reduced-motion: reduce` removes transform-based motion and preserves opacity and
  colour changes. Motion is never the only carrier of a state change (M7, M12).
- **UI motion stays under 300ms**, except a deliberate one-off moment. A 180ms dropdown reads
  as more responsive than a 400ms one at the same actual speed.
- **Never `scale(0)`.** Start at `scale(0.9–0.97)` with `opacity: 0` (M14).
- **Entrances ease-out, exits faster.** Slow where the user is deciding, fast where the system
  responds — asymmetric timing is what makes an interface feel authored.
- **Prefer transitions over keyframes** for anything retriggerable — transitions interrupt and
  retarget, keyframes restart from zero (M15).
- **Popovers scale from their trigger**, modals from center (M16).
- **Stagger 30-80ms between items**, and never block interaction while it plays (M8).
- **Gate hover motion** behind `@media (hover: hover) and (pointer: fine)` (M18).

Springs, where physics beats a fixed duration — drag with momentum, interruptible gestures,
decorative pointer tracking:

```js
{ type: "spring", duration: 0.5, bounce: 0.2 }        // Apple-style, easier to reason about
{ type: "spring", mass: 1, stiffness: 100, damping: 10 } // traditional, more control
```

Keep bounce subtle (0.1-0.3) and reserve it for drag-to-dismiss. A spring carries velocity
through an interruption, which is exactly what a gesture the user may reverse needs.

```css
:root {
  --ease-out-ui: cubic-bezier(0.23, 1, 0.32, 1);
  --ease-in-out-panel: cubic-bezier(0.77, 0, 0.175, 1);
  --dur-instant: 0ms;
  --dur-fast: 120ms;
  --dur-base: 180ms;
  --dur-panel: 320ms;
}
@media (prefers-reduced-motion: reduce) {
  :root { --dur-fast: 0ms; --dur-base: 0ms; --dur-panel: 0ms; }
}
```

### Colour tokens

Work in OKLCH. It makes "the same hue, lighter" and "less chroma for dark mode" one-number
operations, which is exactly what the C-class rules need.

| Role | Rule |
|---|---|
| Surface floor | Never `#000`. Lift the dark floor and tint it toward the brand hue — `oklch(0.18 0.012 250)`, not `#000` (C7) |
| Light surface | Never `#fff` as a large plane. Tint and pull it just off the top (C7) |
| Neutrals | Small chroma at the brand hue — enough to belong, well under half the brand's chroma (C8) |
| Brand | The one chromatic anchor. Reserved for identity, not decoration |
| Action | Distinct from brand unless the brand *is* the action. Status colours never double as action colours (C10) |
| Status | success / warning / error / info. Re-tuned per theme, never reused |
| Emphasis | Contrast and weight before colour; colour is the last tool |
| Dark-mode fills | Lower chroma, higher lightness. Full-chroma fills vibrate on dark surfaces (C9) |
| The reflex to refuse | The warm-neutral-plus-terracotta "tasteful" scheme is a default, not a decision. If the palette arrived without a reason, it is C11 |

Floors: 4.5:1 for body text, 3:1 for large text and UI boundaries (C6/A2). Re-verify in
dark mode against the actual dark surface — a ratio tuned on light says nothing about dark
(DM1).

```css
:root {
  --surface-0: oklch(0.99 0.004 250);
  --surface-1: oklch(0.97 0.006 250);
  --ink-1: oklch(0.24 0.012 250);
  --ink-2: oklch(0.48 0.010 250);
  --brand: oklch(0.55 0.14 250);
  --action: oklch(0.52 0.17 250);
  --radius-card: 10px;
}
@media (prefers-color-scheme: dark) {
  :root {
    --surface-0: oklch(0.18 0.012 250);   /* lifted, tinted floor — not #000 */
    --surface-1: oklch(0.22 0.014 250);
    --ink-1: oklch(0.93 0.008 250);
    --ink-2: oklch(0.72 0.010 250);
    --action: oklch(0.68 0.13 250);        /* lifted, chroma reduced for dark */
  }
}
```

### Type tokens

| Role | Rule |
|---|---|
| Roles | display / heading / body / label / data — five, named, used everywhere |
| Scale | one ratio, commonly 1.2–1.333 (T18). Ad-hoc sizes are the defect |
| Hierarchy | adjacent roles must differ visibly in combined size + weight + spacing + colour (T1) |
| Measure | 60–75ch for running prose (L6) |
| Line height | ~1.5 body; tighter only for display type at large sizes (Q6) |
| Numerals | `font-variant-numeric: tabular-nums` wherever digits align in a column or update in place (T16) |
| Uppercase | 0.05–0.12em tracking on uppercase micro-labels (T17); never wide tracking on body (Q8) |
| Display cap | Display type stops around 6rem. Past that the hero is shouting rather than leading (T6) |
| Tracking floor | Tight display tracking stops around -0.04em. Beyond it the letterforms collide (T7) |
| Family | one family carrying hierarchy through size/weight/width/spacing is stronger than two unrelated ones. A second family earns its place by role (T9) |

Inter is a legitimate face for a clean/technical identity. It is a defect as an unexamined
default (T8) or paired against a display serif (H6).

---

## Stage 3 — Build

Build against the brief and the token layer. Per section, before moving on:

- **Structure follows content.** Comparable items → table or aligned list. A sequence →
  steps. Narrative → editorial flow. Choices → bounded controls.
- **One anchor per section** (X6). One element carries the section. If everything is
  equally weighted, nothing is the point.
- **Vary the rhythm.** A page of identical splits is X5. Let each section take the shape its
  content implies: full-bleed, asymmetric, tabular, multi-panel.
- **Compose the grid from the content** (L11). Cell count follows item count. Never add a
  filler cell to square a layout.
- **Group with spacing and type before reaching for a container** (V18, L4). A border is
  structural separation, not a hierarchy tool.
- **State coverage is part of the component, not a follow-up** (S1-S8): empty, loading,
  error, disabled, partial, permission-gated.
- **Feedback is part of the action** (F1-F6): pending state, success confirmation,
  double-submit guard, field-mapped validation.
- Hover may add. It must never gate (F7).

Build the source with the same discipline as the render. The E-category defects are cheapest
to avoid here and most expensive to find later:

- **Derive, do not synchronise.** If a value can be computed from props or existing state,
  compute it. State kept in sync by an effect is two sources of truth that will disagree (E1, E2).
- **Every effect declares its cleanup** — timer, listener, subscription, request (E14-E17).
- **Every fetch ships all four branches**: loading, error, empty, success (E9).
- **One owner per fact.** One state value, one source, one boundary (E2, E25).
- **Type the boundaries.** `any`, `as`, and `!` at a trust boundary are the same finding:
  the compiler was told to stop checking where checking mattered most (E26, E29, E30).
- **Validate server-side.** The client is never the check (E41).
- **Never put a secret where the client can read it.** Anything shipped is public (E39).
- **Keep the client boundary at the leaf** that needs interactivity (E11, E44).
- **Extract on the second use.** Class soup, duplicated clusters, and arbitrary values are all
  a component that has not been written yet (E32, E33, E36).

---

## Stage 4 — Polish passes

Run in this order. Each pass has one job.

1. **Type pass** — roles on the scale, measure bounded, numerals tabular where they align,
   uppercase labels tracked, hierarchy visible without colour (T16-T18, T1, L6, Q6).
2. **Colour pass** — roles assigned, neutrals tinted, floors lifted, dark mode independently
   verified (C7-C10, DM1-DM5).
3. **Motion pass** — every transition named, duration from the frequency table, curve
   authored, reduced-motion honoured (M9-M13, M2, M5, M7).
4. **Copy pass** — buzzwords replaced with exact action/object/user/result, em-dashes
   reduced, rule-of-three headlines collapsed (P1-P9, T11-T15).

---

## Stage 5 — Gate (blocks delivery)

Run this before handing back code. A failure is not a note — it blocks.

**Contract**
- [ ] No class, ID, or `data-*` the runtime references was renamed, removed, or restructured (Q9)
- [ ] React keys stable and unique; semantic elements used for their role (A4, L9)

**Access**
- [ ] 4.5:1 body / 3:1 large and UI boundaries, verified in **both** themes (C6, A2, DM1)
- [ ] Visible focus state on every interactive element; keyboard reachable (A3, A5)
- [ ] Every input has a label; every image has meaningful alt text (A1, A6)
- [ ] Status never communicated by colour alone (A7, M12)

**Layout**
- [ ] Header one row, under ~80px; headline ≤2 lines; primary CTA above the fold at 1440×900 (L12)
- [ ] One primary action per view; no duplicate-intent CTAs (L13)
- [ ] No horizontal overflow at any breakpoint; touch targets ≥44×44px (B2, B3, L7)
- [ ] No empty or filler grid cells (L11)
- [ ] Not every section the same shape or split (L10, X5)
- [ ] Each section has one anchor (X6)

**Motion**
- [ ] No `transition: all` (M9)
- [ ] No animation of width/height/padding/margin/top/left (M2)
- [ ] High-frequency interactions feel instant (M10)
- [ ] Entrances ease-out and stay short (M11)
- [ ] `prefers-reduced-motion` honoured, still state survives (M7, M12)

**Type and colour**
- [ ] Roles come from the defined scale; no ad-hoc sizes (T18)
- [ ] Tabular numerals wherever digits align (T16)
- [ ] No `#000` / `#fff` as large planes; neutrals tinted (C7, C8)
- [ ] Contrast re-verified in dark mode, not assumed (DM1)

**States and content**
- [ ] Empty, loading, error, disabled states present (S1-S8)
- [ ] Pending state, success feedback, double-submit guard (F1-F4)
- [ ] Long text, 0/1/100+ items, null fields, broken images do not break layout (R1-R6)
- [ ] No placeholder lorem-picsum/unsplash-random URLs in the build (I1, I6, V17)

**Source**
- [ ] Every effect that subscribes, times, or fetches returns a cleanup (E7, E14, E15, E17)
- [ ] Every fetch has loading, error, and empty branches (E9)
- [ ] No secret, key, or token reachable from client code (E39)
- [ ] No `dangerouslySetInnerHTML` on user-controlled input (E38)
- [ ] Every trust boundary validates server-side (E41)
- [ ] No `any`, suppression comment, or `!` standing in for a real type (E26, E27, E30)
- [ ] Stable list keys; no array index on a reorderable list (E21, E22)
- [ ] Client boundary at the interactive leaf, not the route root (E11, E44)
- [ ] Browser surfaces themed from the palette — selection, caret, scrollbar, focus ring (Q10)
- [ ] One correctness test on any money, auth, parse, or permission path (E51)

**Domain**
- [ ] If hospitality: truth step run first, H15 satisfied, no fabricated spatial claims
- [ ] If hospitality: H-IDs reported instead of their superseded base patterns

Report failures in the standard format (DIRECTION → FINDINGS → NEEDS RENDERED
VERIFICATION → EXEMPTIONS → VALIDATION) and fix before delivering. `slop-audit`
runs this gate after remediation; `slop-design` runs it as the pre-emit check.

### Label every finding's evidence

A finding is only as good as the evidence under it. Label each material statement with
exactly one of:

- **Observed** — directly present in the material supplied.
- **Measured** — produced by a deterministic check.
- **Inferred** — reasoned from evidence; say plainly that it is an inference.
- **Unknown** — not verifiable from what was supplied.

Never promote inferred or unknown material into a factual claim. An unknown item earns no
passing credit merely because it was not visible — silence is not compliance.

### The completion rule

Do not optimize for emptiness or uniformity. **Stop pruning when the next removal would
reduce** task completion, comprehension, evidence, identity, component or interaction
behavior, action discoverability, migration safety, or accessibility.

Removal is the default fix in this catalog, and it is the right one most of the time. It
stops being right the moment taking something away costs the user something. A page stripped
past that point is not cleaner; it is less usable, and it is the failure mode an anti-slop
pass is most likely to cause.

---

## Stage ordering against the fix order

This spec is the construction-time mirror of the remediation fix order. They are the same
sequence seen from two ends — the reason the fix order works is that it reverses the order
in which the defects were created.

| This spec | Fix order stage (`references/fix-order.md`) |
|---|---|
| 1 Brief | — (prevents Stage 1-3 defects) |
| 2 Constraints | 4 Type hierarchy, spacing rhythm, colour roles |
| 3 Build | 2 IA, 3 repeated templates |
| 4 Polish | 1 Correctness, 5 Decorative, 6 Copy |
| 5 Gate | 8 Motion, plus verification of all |
