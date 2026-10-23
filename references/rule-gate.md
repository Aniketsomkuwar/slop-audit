# Rule Gate - Purpose, Liveliness, and Delivery

Derived from antislop (MIT, `miqdadbadjuber/anti-slop`), merged and adapted for this catalog. The 245 patterns say what a defect looks like. These rules govern the decision that produced it, so the same defect does not return under a new shape.

## What this is

A filter, not a style guide. It imposes no palette, typeface, or layout. It rejects technique used without a purpose, and it requires the result to be alive rather than merely clean.

The purpose test, applied before any visual technique:

> What does this serve? If the only answer is "it looks safe" or "it looks AI", the technique goes or is reworked. If the answer names hierarchy, identity, or readability, it stays and the reason is written down.

The replacement test, applied before delivery:

> If the logo and product name were swapped, would this still feel like this product?

If no, the design is generic. Fix the direction, not the decoration.

## The three tiers

| Tier | Meaning | Failure |
|---|---|---|
| Hard Gate | Absolute. No purpose excuses it. | FAIL |
| Purpose-Gate | Technique allowed, written reason required. | FAIL when used as a default or the reason is missing |
| Quality Lock | Consistency requirement. | FAIL when the system is inconsistent |

When a rule and a pattern disagree, the stricter one wins. Report rule IDs (`R-XX`) next to pattern IDs, never instead of them.

## Hard Gate

| ID | Rule |
|---|---|
| R-02 | No em dash (`—`) in output text. Use comma, period, colon, or parentheses. |
| R-03 | Mobile is a designed state. No horizontal overflow, no clipped text or cards, navbar usable, targets at least 44x44 px, spacing consistent across breakpoints. |
| R-17 | No statistic without a real source. No real data means no number, not an invented one. |
| R-18 | No fictional testimonials, avatars, names, or job titles. No real proof means no testimonials section. |
| R-23 | Do not create a logo, avatar, statistic, testimonial, or navigation structure without instructions or an honest placeholder (`[LOGO]`, `[REAL DATA]`, "Coming soon"). |
| R-24 | Every navbar item points to a real destination. Unbuilt features are left out or labeled. |
| R-25 | WCAG AA contrast: 4.5:1 normal text, 3:1 large text (18px+). Check the worst point of the area the text crosses, not one sample. |
| R-26 | Every interactive element has real behavior or is removed. Acceptable behavior: anchor to an existing section, working modal (Escape closes), state toggle, real external action, form with feedback. |
| R-27 | Every data view has empty, loading, and error states. Ideal-condition-only is unfinished. |
| R-28 | No template FAQ. If the real questions are unknown, no FAQ section. |
| R-32 | Full keyboard operation: logical tab order, Enter/Space activation, Escape closes dialogs, visible focus indicator. Never `outline: none` without a replacement. |
| R-33 | No feature implemented by a script that string-replaces source or CSS. Write it in source. |
| R-34 | Every shipped theme works: contrast, colors, fonts, layout in both modes. |
| R-35 | Run or build before delivery. Click every interactive element and record the result. If the artifact cannot run, say so and verify by code inspection instead of claiming a click-through. |
| R-36 | No invented security, compliance, or performance claims. No fabricated testimonials, statistics, or names. |
| R-37 | Load written direction (`DESIGN.md` or a brand brief) before building. Without it, label the output "draft without direction", set ENERGY 1 / RHYTHM 1 / MOTION 1, and do not present it as shippable. |
| R-38 | Every claim, feature, testimonial, statistic, nav item, and visual is real or an explicit placeholder. An empty section beats a fabricated one. |

## Purpose-Gate

Each rule allows the technique when it serves hierarchy or identity and the reason is written down. The dose caps are hard.

| ID | Rule | Dose cap |
|---|---|---|
| R-01 | No default blue-to-purple, blue-to-cyan, purple-to-pink gradients, rainbow or neon palettes, radial orbs, or full-page glow. A gradient that separates hierarchy levels is craft; the same gradient on every section is a default. | One gradient job per page |
| R-04 | Icons must be relevant to the content. No sparkle, star, magic, lightning, diamond, orb, robot, and no icon set chosen for its library look (Lucide-style thin rounded strokes). | No generic glyph without a written relevance |
| R-06 | No monospace as a terminal costume, no wide-tracked uppercase labels by default. Typeface follows brand character, not the model's default roster (Inter, Geist, Space Grotesk, Geist Mono, JetBrains Mono, Fira Code). | One type reason per family |
| R-07 | No grid, blueprint, graph paper, or dot background by default. Texture must support the product's identity. | One texture job |
| R-08 | Arrows (`→`, `↗`) are not the default on every button. | Only where direction is the message |
| R-09 | No decorative capsule badges ("AI Powered", "Beta", "New", "Secure", "Fast"). A badge needs a real status. Never capsule + thin border + glow + dot + uppercase together. | Functional status only |
| R-10 | Glass is an accent, not the character of the UI. | 1-2 elements |
| R-12 | Shadow marks elevation, not every component. The page should not float. | One elevation system |
| R-13 | Glow is a focus accent. | 1-2 elements |
| R-14 | Feature cards may not be identical in size, icon, padding, and layout when the content differs in weight. | Variation follows hierarchy |
| R-19 | Motion needs a UX purpose and must match the declared MOTION dial. No Fade Up + Float + Scale + Bounce on everything. | One choreography per view |
| R-22 | No Undraw, Storyset, or 3D blob illustrations without a product connection. Prefer real screenshots or none. | One illustration reason |

## Quality Locks

| ID | Rule |
|---|---|
| R-05 | No template layout: hero + cards, "How It Works" always three steps, bento mosaic by default, fake terminal hero, three pricing columns by default, "Trusted By" bar under the hero, four-column template footer, or every section built from the same centered title + card grid. Structure follows content and the RHYTHM dial. |
| R-11 | Radius is consistent with the design system. Not every element is a pill. Variation is hierarchy. |
| R-15 | No generic CTAs (Get Started, Learn More, Try Now, Explore, Discover). Name the action. |
| R-16 | No buzzwords: AI Powered, Next Generation, Revolutionary, Seamless, Cutting Edge, Intelligent, Ultimate, Powerful, Effortless. State the benefit. |
| R-20 | The design has an identity: a specific palette, a typeface chosen for a reason, a composition that belongs to this product. |
| R-21 | Theme follows brand, product, and audience. Dev and creative tools may justify dark. If there is no strong reason, build a working toggle, and do not defer it. |
| R-29 | At most 2-3 core colors plus one accent. Neutrals do not count. Five or more colors need an explicit system. |
| R-30 | Do not clone Linear, Vercel, Stripe, Notion, or Apple unless asked. Reference is not a template. |
| R-31 | Write a one-line reason for every major decision: color, layout, type, spacing, cards, illustration. If it cannot be written in one line, revisit the decision. |

## Craftsmanship standard

| ID | Criterion | Question |
|---|---|---|
| C-1 | Intentionality | Does every choice have a reason beyond "it is the AI default"? |
| C-2 | Functional completeness | Does every interactive element actually work, or is it absent? |
| C-3 | Content-driven composition | Does every section exist because the content needs it? |
| C-4 | Resilience | Does it hold in every state, theme, breakpoint, and with a keyboard only? |
| C-5 | Evidence over claims | Is every testimonial, statistic, and claim real and verifiable, or absent? |

## Liveliness

Removing slop leaves a void. Liveliness is added deliberately, not assumed. Set three dials before generating and hold them across the page.

| Dial | 1 | 2 | 3 | Answers |
|---|---|---|---|---|
| ENERGY | quiet, linear | balanced | bold, expressive | How hard does the design say hello? |
| RHYTHM | uniform grid | mostly consistent, a few breaks | asymmetric, mixed | How much do sections differ? |
| MOTION | hover states only | scroll reveal, transitions | parallax, pin, choreography | How much motion, and why? |

The anchors (Linear, GOV.UK, Stripe, Vercel, Awwwards) are taste references for judging a value, not templates.

Declare the Design Read before generating:

> Reading this as: `<page kind>` for `<audience>`, in a `<visual language>` style, dial ENERGY `<1-3>` / RHYTHM `<1-3>` / MOTION `<1-3>`.

Levers:

- One focal point per screen. Exactly one element dominates; everything else defers.
- Hierarchical contrast by size, weight, and color, not by decoration.
- Whitespace as structure, not leftover space.
- One deliberate accent. Zero is sterile; everywhere is slop.
- One identity motif, repeated with intent, that makes the page belong to the product.

## Delivery Gate

Run before delivery. Output a PASS/FAIL line per block with evidence. Any FAIL blocks the deliverable.

### Block 1: Hard Gate (all answers must be no)

- Em dash anywhere in UI text?
- Horizontal overflow, clipped text, or broken mobile layout?
- Statistics without a source?
- Fictional testimonials or identities?
- Assets created without instructions or honest placeholders?
- Navbar links to anything that does not exist?
- Any text below 4.5:1, or large text below 3:1?
- Dead buttons, dropdowns, or forms with no behavior and no visible "Coming soon"?
- Missing empty, loading, or error state?
- Template FAQ questions?
- Not keyboard operable, or no visible focus?
- Any feature added by a patch script instead of source?
- A shipped theme that breaks?
- Delivered without running it, or without a recorded click-through?
- Fabricated security, compliance, performance, or customer claims?
- Built without direction and not labeled "draft without direction" with dials 1/1/1?

### Block 2: Purpose-Gate (FAIL if default or unwritten)

- Gradient, glow, or color used as a default with no stated hierarchy or brand reason?
- Generic or irrelevant icons, or an icon set picked for its library look?
- Monospace costume, wide-tracked uppercase, or a typeface with no brand reason?
- Background grid, blueprint, or dot pattern with no identity reason?
- Decorative arrows on most buttons?
- Decorative capsule badges, or the full capsule + border + glow + dot + uppercase stack?
- Glass on more than 1-2 elements?
- Large shadow on every component?
- Glow on cards, buttons, badges, icons, background, and border at once?
- Identical feature cards despite unequal content weights?
- Template animations on everything, or motion that contradicts the MOTION dial?
- Generic illustrations with no product connection?

### Block 3: Liveliness (all answers must be yes)

- Are ENERGY / RHYTHM / MOTION declared?
- Is the output consistent with the declared dials?
- Is there one clear focal point per screen?
- Is whitespace structural?
- Is there exactly one deliberate accent?
- Is there an identity motif?
- Was a Design Read declared before generation?

### Block 4: Craftsmanship and Quality Locks (all answers must be no)

- Any decision justified only by "it is the AI default"?
- Any interactive element that does nothing?
- Any section that exists only to fill a template?
- Any break in a state, theme, breakpoint, or keyboard use?
- Any fabricated claim?
- Any template layout from R-05, or a section rhythm that contradicts RHYTHM?
- Every element pill-shaped with no radius variation?
- Generic CTAs?
- Buzzwords?
- Still generic with the logo swapped?
- Dark mode forced with no reason, or a required toggle deferred?
- More than 2-3 core colors plus one accent?
- A clone of a popular product?
- Any major decision whose reason cannot be written in one line?

## Report format

For each finding: rule ID, pattern ID if one applies, evidence (Observed, Measured, Inferred, Unknown), confidence (Confirmed, Probable, Candidate, Exempted), severity (blocker, major, nit), and location. The gate report is the final block, not a substitute for the findings list.
