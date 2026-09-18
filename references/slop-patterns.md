# AI Slop Pattern Reference — Full Catalog

Authoritative pattern catalog. Integrated from anti-ui-slop (ritmex-skills),
anti-slop (miqdadbadjer/anti-slop), the motion principles popularised by Emil
Kowalski (transform/opacity-only, named transitions, duration scaled to distance and
frequency), the render-side and source-side catalogs from Anti-AI-UI (b4r7x, MIT), the
mechanical composition rules from design-taste-frontend, the quality floor from
impeccable (Apache 2.0), and the evidence discipline from leeskills (MIT) — extended with
accessibility, design-token drift, state coverage, real-content stress, responsive,
interaction-feedback, dark-mode, hospitality/luxury, and engineering categories.

**Core objective:** avoid low-effort "AI-generated template" tropes, boilerplate
Bootstrap/SaaS defaults, and unstyled third-party widgets. Every layout must feel
deliberate, bespoke, and tailored to the actual product domain.

Total: 245 patterns across 18 categories.

The count includes the composition checks X1-X10. Earlier revisions listed X1-X4 as
"beyond catalog" and excluded them from the total — that exclusion is retired, since X5-X10
are reported on the same footing as every other pattern. A category total that does not add
up to the headline number is the defect this note exists to prevent.

Categories cover two surfaces. V, T, C, L, M, P, I, Q, X describe the **rendered artifact**.
E describes the **source** — React state, data fetching, lifecycle, component architecture,
TypeScript, markup, security, performance, testing, and form handling. A deliverable can pass
every render-side check and still fail E. Scan both.

Patterns are one half of the system. `references/design-spec.md` carries the other half —
the construction workflow, motion tokens, OKLCH colour rules, type roles, and the
pre-emit gate that blocks delivery. Read both before generating UI.

---

## Visual Details (V1-V21)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **V1** | Border accent on rounded element | Colored border >=2px and radius >=12px on same generic card | Source | Choose one: neutral 1px border, lower radius, or no border |
| **V2** | Glassmorphism everywhere | Repeated translucent cards + backdrop-filter: blur on static content | Judgment | Use opaque surfaces; express hierarchy through contrast, spacing, elevation |
| **V3** | Side-tab accent border | One side of card has colored border >=3px | Source | Use status dot, icon, label, or restrained tint when meaning exists; remove empty decoration |
| **V4** | Hairline border with wide shadow | 1px border + shadow blur >=20px on one element | Source + opt-in | Keep crisp edge OR soft elevation (not both) |
| **V5** | Repeating-gradient stripes | repeating-linear-gradient or repeating-conic-gradient on surface | Source + opt-in | Use solid surface or existing brand texture |
| **V6** | Extreme border radius | Radius >=24px turns small card/section/input into blob | Judgment | Use role-based radius scale; common cards: 8-16px |
| **V7** | Amateurish hand-drawn SVG | Complex inline SVG scene/mascot with rough geometry | Judgment | Use real asset, simplify to purposeful diagram, or remove illustration |
| **V8** | Generic AI icons | Sparkle, star, magic, lightning, diamond, robot, AI orb | Source | Replace with product-relevant icons or remove |
| **V9** | Lucide icons | Every icon from same thin-stroke rounded library | Source | Use product-specific icons or vary styles |
| **V10** | Colored left stripe | Thin colored vertical bar on left edge of cards/rows/sections | Source | Remove or use as status indicator with meaning |
| **V11** | Small arrows (→/↗) | Placed on almost every button as pure decoration | Source | Remove or add functional meaning (sort, expand, etc.) |
| **V12** | AI capsule badges | Pill shape, thin border, glow, small dot, uppercase "AI Powered" etc. | Source | Remove or use as functional status badge with reason |
| **V13** | Generic AI typography | Large monospace headings, "HOW IT WORKS" uppercase with wide tracking | Source | Use brand typeface; reserve monospace for code/context |
| **V14** | Typeface chosen without reason | Font picked because it's AI default, not because it fits brand | Source | Choose font based on brand character; Inter valid if justified |
| **V15** | Generic illustrations | Undraw, Storyset, or 3D blob characters with no real product connection | Source | Use product-relevant illustrations or remove |
| **V16** | Mixed icon styles in one view | Outline + filled + duotone glyphs in the same view | Source | Pick one stroke/weight system |
| **V17** | Leftover placeholder images | lorem-picsum, unsplash-random, placeholder API paths in a production build | Source | Replace with real or authorized assets |
| **V18** | Border-as-hierarchy crutch | A dividing border or outline drawn around every text block, card, and label instead of establishing hierarchy typographically | Source | Establish hierarchy with font size, weight, letter-spacing, and muted opacity (`0.6`–`0.8`). Reserve borders for structural separation, not for containing text |
| **V19** | Ungraded third-party embed | Raw default-styled map, calendar, calculator, or booking/chat widget dropped into a custom palette with no filter stack, overlay, or vendor theme | Source | Grade embeds to the surrounding palette with a CSS filter stack, custom color overlay, or the vendor's theme hook |
| **V20** | Unscrimmed ambient video | Video used as a section underlay with no gradient overlay, so foreground text contrast varies frame to frame | Source + Render | Calibrated linear **and** radial gradient scrims, verified so text holds AAA contrast across every frame |
| **V21** | Geometric mask faking an organic cutout | A circle, polygon, or radial-gradient cutout approximating a photographic subject's edge, so the subject appears cropped by a shape it does not match | Source + Render | Derive an alpha matte from the actual image, or produce a real cut-out asset. The geometric approximation reads worse than omitting the effect |

## Typography (T1-T18)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **T1** | Flat type hierarchy | Adjacent roles differ <1.25x and lack contrast in weight/family/spacing/color | Source | Define fewer roles with clearer combined contrast; use ratio as screening heuristic |
| **T2** | Icon tile stacked above heading | Repeated feature cards use rounded-square icon tile directly above heading | Source | Align icon and heading; place icon in flow; remove container |
| **T3** | Italic serif display headline | Serif italic >=32px leads generic startup hero | Source | Use roman styling or display treatment from brand register |
| **T4** | Hero eyebrow/pill chip | Tiny tracked uppercase text/pill sits immediately above hero heading | Source | Fold information into heading, navigation context, or body copy |
| **T5** | Repeated section kickers | 3+ sections repeat same uppercase tracked mini-label | Source | Let heading hierarchy, artifacts, section-specific structure create navigation |
| **T6** | Oversized hero headline | Heading with 8+ words uses >=48px or text-5xl styling | Source | Shorten claim or reduce responsive size so supporting content visible |
| **T7** | Crushed letter spacing | Display tracking < -0.05em; body text carries negative tracking | Source | Restore character shapes; use modest optical tightening around 0 to -0.02em |
| **T8** | Overused font | Inter, Geist, Space Grotesk, or Instrument Serif appears as default identity | Source, weak | Report only with other convergence signals; strengthen roles with existing fonts first |
| **T9** | Single font for everything | One family uses nearly identical weight/width/spacing/proportions across every role | Source, weak | Build contrast inside family or add second family with defined display/body/data role |
| **T10** | All-caps body text | Paragraphs/long blocks >=20 words in uppercase | Quality | Use sentence case; reserve uppercase for short labels |
| **T11** | "It's not just X, it's Y" | Construction repeated across sections | Source | State the thing directly |
| **T12** | Buzzword stacking | seamless, leverage, unlock, elevate, robust, cutting-edge in one block | Source | Name exact action, object, user, result |
| **T13** | Em-dash overuse | 3+ em-dashes as sentence-structure crutch in one body block | Source | Rewrite with sentence boundaries, commas, colons, parentheses |
| **T14** | Inflated fake urgency/stats | "only 3 spots left", "10,000+ happy customers" with no source | Source | Remove, or cite the real source |
| **T15** | Rule-of-three headline tic | "Fast. Reliable. Secure." | Source | Say the one thing that matters |
| **T16** | Proportional numerals in tabular context | Metrics, prices, dates, or table columns render with default proportional figures, so digit widths vary and the column jitters as values update | Source | Set `font-variant-numeric: tabular-nums` on any figure read in a column or refreshed in place. Proportional figures stay correct for prose and display type |
| **T17** | Uppercase label without compensating tracking | Uppercase micro-labels — eyebrows, table headers, badges — set at default or negative tracking, so letters crowd each other | Source | Uppercase needs air: roughly 0.05em–0.12em. Q8 is the inverse defect — wide tracking applied to running body copy |
| **T18** | No modular type scale | Sizes chosen ad hoc per component; no ratio relates adjacent roles, so hierarchy is accidental rather than decided | Source + Judgment | Define roles (display/heading/body/label/data) on one ratio, commonly 1.2–1.333. D4 is the *drift* case — a defined scale that components ignore. Report T18 only when no scale exists to drift from |

### Font pairing — match the typeface to the identity

Mixing a geometric/bubbly sans with a classic or formal face is the defect (T8/V14,
and H6 in the hospitality domain). Pick by identity instead:

- **Clean / technical:** neutral, balanced sans-serifs — Plus Jakarta Sans, Manrope, Inter
- **Editorial / refined:** a high-contrast serif paired with a restrained humanist sans — Cormorant Garamond with Tenor Sans or Plus Jakarta Sans

**Inter is not banned.** T8 flags it as an *identity default*, and H6 flags it as a
*pairing* against a display serif. Inter is a correct choice for a clean/technical product
identity with brand justification. Report it only where it is an unjustified default or a
mismatched pairing.

## Color and Contrast (C1-C11)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **C1** | AI color palette | Purple/violet gradients or cyan-on-dark dominate without brand evidence | Source, weak | Report only as cluster signal; derive action/status/surface/accent from existing product context |
| **C2** | Dark mode with glowing accents | Dark surfaces repeat colored box shadows or neon text shadows | Source | Use neutral elevation and bounded state indicators; focus rings exempt |
| **C3** | Gradient text | background-clip: text + transparent fill on headings/metrics/labels | Source | Use solid text color; express emphasis through hierarchy |
| **C4** | Gray text on colored background | Neutral gray on chromatic surface loses contrast | Source/Render | Use light/dark tint related to surface hue; verify contrast |
| **C5** | Cream/beige reflex | Warm off-white becomes whole page's generic tasteful surface without palette | Source, weak | Establish deliberate surface, ink, accent, and status colors |
| **C6** | Contrast failing WCAG AA | <4.5:1 normal text, <3:1 large text | Source/Render | Adjust foreground, background, weight, or size; verify computed result |
| **C7** | Pure black/white surfaces | `#000` background or `#fff` surface used as-is, with no tint and no lifted floor | Source | Tint neutrals toward the brand hue and lift the dark floor — e.g. `oklch(0.18 0.012 250)`, not `#000`. Pure `#fff` on pure `#000` produces halation and destroys perceived depth |
| **C8** | Zero-chroma neutrals against a chromatic brand | The neutral ramp sits at 0 chroma while the brand hue is saturated, so greys read as a second, unrelated system | Source | Give neutrals a small chroma at the brand hue — enough to belong, generally well under half the brand's chroma |
| **C9** | Full-chroma accent fill on a dark surface | The brand accent is used at maximum saturation as a large fill on a dark background, so it vibrates against the surface | Source + Render | Reduce chroma and lift lightness for dark-mode fills; keep full chroma for small indicators only. C2 is *glow* (shadow, neon text); C9 is *fill saturation* |
| **C10** | Palette with no role assignment | Colours exist as a flat hex list; nothing distinguishes brand, action, status, emphasis, and surface | Source (absence) | Give every colour a job and name the token after the job rather than the hue. Report when one chromatic colour serves two different jobs, or when status and action share a single colour |
| **C11** | Premium-consumer palette reflex | The page lands on the same "tasteful" warm-neutral-plus-terracotta scheme with no brand evidence: backgrounds in the `#f5f1ea` / `#f7f5f1` / `#fbf8f1` / `#efeae0` / `#ece6db` / `#e8dfcb` family, accents in the `#b08947` / `#b6553a` / `#9a2436` / `#9c6e2a` / `#bc7c3a` / `#7d5621` family, ink in the `#1a1714` family | Source + Judgment | Pick a named, committed world instead. C5 is the *surface-only* cream/beige reflex; C11 is the full three-role scheme appearing together, which is what makes it a tell rather than a choice. Report one or the other, never both |

## Layout and Space (L1-L13)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **L1** | Hero metric layout | Hero centers large number, tiny label, supporting stats with generic accent | Judgment | Lead with product/artifact/verified evidence; preserve only meaningful metrics |
| **L2** | Identical card grids | 4+ siblings repeat icon, heading, paragraph with identical sizing | Judgment | Choose structure from content: ranked layout, table, list, accordion, or genuine uniform controls |
| **L3** | Monotonous spacing | Same gap/padding token serves item/group/component/section boundaries | Source | Map spacing tokens to semantic relationships; create clear grouping contrast |
| **L4** | Nested cards | 3+ nested levels repeat borders/fills/radii/shadows | Source | Remove redundant surfaces; group with spacing, headings, dividers, or one shared container |
| **L5** | Numbered section markers | Display 01/02/03 labels decorate independent sections | Source | Keep numbers for real sequences, timelines, ranks, or references |
| **L6** | Line length too long | Running text exceeds ~80 chars per line | Quality | Constrain prose to roughly 60ch-75ch; code/tables/deliberate data exempt |
| **L7** | Content overflowing container | Text/media spills, clips, creates accidental horizontal scroll | Quality | Wrap, constrain, truncate with accessible affordance; provide deliberate scrolling |
| **L8** | Positioned child clipped by overflow | Tooltip/menu/popover cut by ancestor using overflow:hidden/clip | Quality | Move layer outside clipping context; use portal; change overflow where safe |
| **L9** | Non-semantic markup | div-soup instead of button/nav/main/header | Source | Use semantic elements |
| **L10** | Default 50/50 split reflex | Section after section resolves to a predictable 50% text / 50% media split | Source | Vary the rhythm: full-width banner, asymmetric columns (40/60), tabular data rows, or an interactive multi-panel viewport |
| **L11** | Orphan or decorative grid cell | Grid cell count does not match item count: an empty cell remains, or a filler cell was inserted to square the layout | Source | Let content choose the grid. Redistribute the item span, or move to a structure whose cell count follows the content. A bento is composed *from* content, never filled *to fit* |
| **L12** | Above-the-fold hero overflow | At 1440×900 the hero headline or primary CTA falls below the fold, or the header exceeds ~80px or wraps to a second row | Render | Hold the header to one row under ~80px, the headline to two lines, and get the CTA above the fold without scrolling |
| **L13** | CTA competition | Two or more calls to action carry equal visual weight, or two express the same intent ("Get started" beside "Start free trial") | Judgment | One primary action per view; secondaries take subordinate treatment and a genuinely different intent |

## Motion (M1-M18)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **M1** | Bounce/elastic easing | Dialogs/cards/routine controls use overshoot curves/repeated spring effects | Source | Use restrained ease-out curve and shorter travel |
| **M2** | Layout-property animation | Large/repeated transitions animate width/height/padding/margin/top/left causing reflow | Quality | Prefer transform and opacity; use bounded disclosure for necessary height changes |
| **M3** | Image hover transform | Generic cards repeatedly scale/rotate imagery on hover | Source + opt-in | Keep imagery still or use restrained overlay, caption, or border response |
| **M4** | Blanket scroll-fade-in | Applied to every section indiscriminately | Source | Reserve reveal motion for content that genuinely arrives in sequence |
| **M5** | Uniform 300ms ease-in-out | Same duration regardless of element size or travel distance | Source | Scale duration to distance and mass |
| **M6** | Reflexive hover:scale-105 / lift-shadow | Applied to non-interactive cards | Source | Motion only where interaction is real |
| **M7** | No prefers-reduced-motion | Motion runs regardless of the user's OS setting | Source | Honour the media query; provide a still equivalent |
| **M8** | Staggered-list-reveal cliché | Applied to non-list content | Source | Reserve staggering for genuine lists |
| **M9** | Blanket `transition: all` | Transition declared on every property rather than on the properties meant to animate | Source | Name the properties: `transition: transform 160ms, opacity 160ms`. Blanket transitions animate unintended properties and make the timing unpredictable |
| **M10** | Motion on a high-frequency action | Keyboard shortcut, search, filter, tab switch, row hover, or menu open animates long enough to be felt as lag | Source | High-frequency interactions resolve in roughly 100–150ms, or immediately. Reserve deliberate motion for occasional and rare interactions |
| **M11** | Entrance on ease-in or over 300ms | Entering elements use an ease-in curve, or a duration above 300ms | Source | Entrances decelerate: ease-out or a custom curve, kept short. Ease-in belongs on exits |
| **M12** | Motion-only state indication | A state change is carried solely by animation, with no persistent static difference once the motion ends | Source + Judgment | Pair the motion with a static affordance that survives `prefers-reduced-motion`. A7 is the colour-only case; M12 is the motion-only case |
| **M13** | Untuned default easing curve | Every state transition uses the browser default `ease` or `linear`, with no curve chosen for the interaction | Source | Choose a curve per interaction class and keep it in a token. M5 covers the duration axis, M13 covers the curve axis — a page can fail either one alone |
| **M14** | Entrance from `scale(0)` | Element appears from nothing — `scale(0)`, or a pure opacity pop with no scale offset | Source | Start from `scale(0.9–0.97)` plus `opacity: 0`. The small non-zero start is what makes the entrance read as physical rather than as a toggle |
| **M15** | Keyframes where a transition belongs | A rapidly-retriggered element (toast, toggle, dynamic list) animates via `@keyframes`, so an interrupted run restarts from zero instead of retargeting mid-flight | Source | Use a `transition` for anything the user can retrigger before it settles — transitions interrupt and retarget, keyframes restart. Reserve keyframes for predetermined, one-shot motion |
| **M16** | Popover scaling from its own center | Dropdown, menu, or popover scales from the element's center rather than from the trigger that opened it | Source | Set `transform-origin` to the trigger. Modals are the one exemption — they are centered in the viewport and keep `transform-origin: center` |
| **M17** | Compositor-breaking animation technique | Motion that forces main-thread work: child transforms driven by a CSS variable on the parent, `will-change` left on permanently, or a rAF loop writing into React state | Source | Set `transform` directly on the animating element, scope `will-change` to the animating window, and keep rAF out of React state. Animate `transform` and `opacity` only (M2) |
| **M18** | Ungated hover motion | Hover-triggered transform that also fires on touch, where the tap produces a false hover and the element moves under the finger | Source | Gate hover motion behind `@media (hover: hover) and (pointer: fine)`. Applies to any hover transform, not just scale |

## Copy (P1-P9)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **P1** | Em-dash overuse | Body block contains 3+ em-dash characters or repeated HTML entities | Source | Rewrite with sentence boundaries, commas, colons, or parentheses |
| **P2** | Marketing buzzword | Generic claims: supercharge, streamline, empower, world-class, enterprise-grade, seamlessly, unlock | Source | Name exact action, object, user, and result |
| **P3** | Aphoristic cadence | Multiple sections repeat patterns: Not X. Y. or Less X. More Y. | Source | Replace with direct product facts and natural sentence rhythm |
| **P4** | Theater framing copy | Marketing dismisses category as theater without explaining practical failure | Source + opt-in | State the behavior, limitation, or consequence directly |
| **P5** | "It's not just X, it's Y" | Same construction as T11 | Source | State the thing directly |
| **P6** | Buzzword stacking | Same list as T12 | Source | Name exact action, object, user, result |
| **P7** | Em-dash overuse as crutch | Same tell as T13 | Source | Rewrite with sentence boundaries |
| **P8** | Inflated fake urgency/stats | Same tell as T14 | Source | Remove or cite the source |
| **P9** | Rule-of-three headline tic | Same tell as T15 | Source | Say the one thing that matters |

## Imagery (I1-I6)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **I1** | Broken/placeholder image | Rendered image markup has missing/empty src, #, known placeholder services, or placeholder API paths | Quality | Use real asset, generated asset with authorization, fallback, or remove element |
| **I2** | Generic stock-photo tells | diverse-team-laughing-at-laptop, handshake close-up, lightbulb-idea shot | Judgment | Use imagery specific to the actual product or venue |
| **I3** | AI-image generation artifacts | Garbled embedded text, malformed hands, uncanny symmetry, inconsistent shadows | Render | Regenerate, or use real photography |
| **I4** | Mixed icon styles in one view | Outline + filled + duotone | Source | Pick one system (see V16) |
| **I5** | Inconsistent aspect ratios/crops | Random crops across a grid or gallery; images not using `object-fit: cover` | Source + Render | Standardize ratios by role — 1:1 catalog items, 16:9 landscape showcases, 4:5 vertical portraits — and set `object-fit: cover` |
| **I6** | Leftover placeholder images | lorem-picsum, unsplash-random in production | Source | Replace (see V17) |

## General Quality (Q1-Q10)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **Q1** | Cramped padding | Text/controls within ~8px of bordered/colored edge | Render | Provide enough internal space for component's role; 12-16px suits common controls/cards |
| **Q2** | Body text touching viewport edge | Running text reaches viewport edge, especially on mobile | Render | Add responsive content gutter, commonly at least 16px on narrow screens |
| **Q3** | Justified text | text-align: justify without effective hyphenation/language support | Source | Left-align body copy or enable tested hyphenation for editorial use case |
| **Q4** | Low contrast text | Misses WCAG AA: 4.5:1 normal, 3:1 large | Source/Render | Adjust foreground, background, weight, or size; verify computed result |
| **Q5** | Skipped heading level | Rendered outline jumps from h1 to h3 or similar | Source/Render | Restore sequential semantic levels; keep visual styling in classes |
| **Q6** | Tight line height | Multi-line body text uses line height below ~1.3 | Source | Use roughly 1.45-1.7 according to typeface and measure |
| **Q7** | Tiny body text | Running body text below 12px; 12-13px readability risk | Source | Use at least 14px for body copy; 16px is strong default |
| **Q8** | Wide letter spacing on body | Paragraphs use tracking above 0.05em | Source | Restore normal tracking; reserve wide tracking for short labels |
| **Q9** | Contract-breaking refactor | Existing HTML classes, IDs, or data-attributes renamed, removed, or restructured while JS, data fetching, or event listeners depend on them | Source + Judgment | **Blocker.** Preserve every technical hook. Diff selectors the JS references against the markup before changing styles; if a rename is genuinely required, update both sides in the same change |
| **Q10** | Unthemed browser surfaces | Text selection, caret, scrollbar, focus ring, underline offset, and tabular numerals all still carry browser defaults, so the parts the designer did not draw belong to no design system | Source | Theme them from the palette. This is the cheapest available signal that a page was built rather than assembled, and the one most reliably skipped |

## Composition Checks Beyond Catalog (X1-X10)

| ID | Check | Description |
|----|-------|-------------|
| **X1** | Motion saturation | Many elements enter, float, pulse, wiggle, or bounce; keep motion tied to single state change/spatial relationship |
| **X2** | Decorative priority inversion | Icon containers, badges, ornaments occupy more visual weight than message they introduce |
| **X3** | Redundant UX writing | Label, sublabel, helper, placeholder, hint repeat same fact |
| **X4** | Modal abuse | Scrolling multi-column multi-section workflow lives in modal |
| **X5** | Page-level archetype uniformity | The same section skeleton repeats down the page and across routes — identical 50/50 splits, identical card rows, identical section heights. H18 is the hospitality-scoped form of this; report X5 elsewhere, never both. Fix is per-page architecture: each route owns the structure its content implies |
| **X6** | No visual anchor | Every element in a section carries equal weight; nothing is memorable and no element reads as the point of the section. Give one element dominance, chosen by content priority rather than by size alone |
| **X7** | Unbroken zigzag alternation | Every media/text section flips side in strict alternation, so the third consecutive zigzag makes the page read as a template rather than a sequence | Cap consecutive zigzags at two; the third section in a row must take a different shape (full-bleed, tabular, stacked, multi-panel) |
| **X8** | Split-header reflex | Heading left, supporting paragraph right, repeated as the opening of section after section | Let the header take the shape its content implies. A split header on every section is a layout the page never decided on |
| **X9** | Unearned production chrome | Interface furniture that signals "shipped product" without carrying information: version labels in the hero, scroll cues, decorative status dots, locale/time/weather strips, decoration text strips, floating top-right sub-text, photo-credit captions, version footers, micro-meta-sentences, and generic step labels | Delete anything on this list unless it carries real, decision-relevant information. Measure twice: this chrome is what makes a page read as a demo rather than a product |
| **X10** | Numbered-pagination chrome | `01 / 4` counters, scoring bars with filled tracks, and step indicators attached to content that is not a real sequence | Keep numbering only where order itself is information the reader needs (see L5) |

## Accessibility (A1-A16)

| ID | Pattern | Tell | Correction |
|----|---------|------|------------|
| **A1** | Missing/generic alt text | "image", "photo1.png" | Describe the image's content or purpose |
| **A2** | Contrast failing WCAG AA | Duplicate of C6/Q4 — report once | Adjust and verify |
| **A3** | No visible focus state | outline removed with no replacement | Provide a visible focus indicator |
| **A4** | Non-semantic markup | div-soup (see L9) | Use semantic elements |
| **A5** | Broken keyboard nav | Unreachable interactive elements, no skip-to-content | Restore focus order; add a skip link |
| **A6** | Form inputs with no label | Placeholder used as the only label | Add an associated `<label>` |
| **A7** | Color-only status indication | No icon or text fallback | Add a non-color cue |
| **A8** | Missing ARIA roles/live regions | Dynamic content announces nothing | Add appropriate roles and live regions |
| **A9** | Clickable non-interactive element | `<div onClick>` or `<span onClick>` where a `<button>` belongs: not focusable, not keyboard-operable, no role announced | Use the element whose semantics match the behavior. Styling is not a reason to avoid `<button>` |
| **A10** | Positive `tabindex` | `tabindex="1"` or higher reorders focus away from document order | Use `0` to add an element to the natural order, `-1` for programmatic focus only. Positive values are virtually always a defect |
| **A11** | Modal without a focus trap | Focus escapes to the page behind an open dialog, or is never moved into the dialog on open and never returned to the trigger on close | Move focus in on open, trap it while open, restore it to the trigger on close. Use `<dialog>` or the platform's focus management rather than hand-rolling it |
| **A12** | Icon-only control with no accessible name | A close, menu, or search button renders an SVG with no label | Give it an accessible name — visible text, `aria-label`, or a visually-hidden span. The icon alone names nothing |
| **A13** | Decorative SVG not hidden from assistive tech | Purely decorative icons and ornaments are announced, adding noise to every read | Mark decorative graphics `aria-hidden="true"` and `focusable="false"`. Meaningful ones get a `<title>` |
| **A14** | Autofocus on page load | A field steals focus on load, scrolling past the content and disorienting screen-reader and keyboard users | Autofocus only inside a surface the user just opened, such as a dialog or a search panel they triggered |
| **A15** | `title` attribute as the only label | Tooltip-as-label on a control or an image, unreachable by keyboard and touch | Use a real `<label>`, a visible caption, or `aria-label`. `title` is never the sole carrier of a name |
| **A16** | Error state with no programmatic association | A validation message renders visually but is not tied to its field, so assistive tech never announces it | Wire `aria-describedby` to the message and `aria-invalid` to the field. F5 is the visual mapping; A16 is the programmatic one |

## Design-Token / Drift Detection (D1-D6)

| ID | Pattern | Tell | Correction |
|----|---------|------|------------|
| **D1** | Multiple radii for one component type | Across files | Collapse to one token |
| **D2** | Hardcoded hex/px | Instead of referencing defined tokens | Reference the token |
| **D3** | Spacing off the defined scale | Random 13px/22px instead of 8/16/24 | Snap to the scale |
| **D4** | Font-size/weight combos outside the type scale | Ad-hoc sizes | Snap to the scale |
| **D5** | Divergent shadow/elevation values | For equivalent components | Collapse to one token |
| **D6** | Near-duplicate hex values | Should collapse into one token | Reduce to one |

## State Coverage (S1-S8)

| ID | Pattern | Correction |
|----|---------|------------|
| **S1** | Empty state (zero items) | Design it intentionally, not a bare blank container |
| **S2** | Loading state missing | Add skeleton or spinner |
| **S3** | Skeleton/loading layout mismatch | Match the skeleton to the real content's final layout to avoid shift |
| **S4** | Error state | Visible, human-readable error — not a silent failure or raw stack trace |
| **S5** | Disabled state | Visibly disabled, not merely non-functional |
| **S6** | Partial-data state | Null fields must not break layout or render "undefined"/"null" |
| **S7** | Permission-gated state | Show an appropriate message, not a broken/empty view |
| **S8** | Offline/network-loss state | Where relevant |

## Real-Content Stress Test (R1-R6)

| ID | Pattern | Correction |
|----|---------|------------|
| **R1** | Long text overflow | Truncation/wrap behaviour must exist |
| **R2** | List scale | 0, 1, and 100+ items must not break layout; paginate or virtualize |
| **R3** | Mismatched string lengths | In sets that must align (table columns, card titles) |
| **R4** | Number edge cases | Zero, negative, very large, decimals where integers were assumed |
| **R5** | Missing/broken image fallback | Alt text plus a placeholder graphic, not a broken-image icon |
| **R6** | Special characters / RTL / emoji | Must not break layout |

## Responsive / Breakpoint Consistency (B1-B5)

| ID | Pattern | Correction |
|----|---------|------------|
| **B1** | Token drift at certain breakpoints | Same component's spacing/type-scale usage checked at mobile/tablet/desktop |
| **B2** | Touch targets below 44x44px (iOS) / 48x48px (Android) | Enlarge on mobile breakpoints |
| **B3** | Unintentional horizontal scroll | Eliminate at every breakpoint |
| **B4** | Content silently disappearing on smaller breakpoints | Adapt instead of hiding |
| **B5** | Fixed pixel widths that don't scale | Use fluid widths; avoid overflow or dead whitespace |

## Interaction Feedback Audit (F1-F7)

| ID | Pattern | Correction |
|----|---------|------------|
| **F1** | Async actions with no loading indicator | Show progress during the wait |
| **F2** | Destructive actions with no confirmation | Add a confirmation step |
| **F3** | Successful actions with no success feedback | Toast, inline message, or visual state change |
| **F4** | Double-submit not prevented | Button must not stay clickable during a pending request |
| **F5** | Validation errors not mapped to their field | Attach the error to the specific field |
| **F6** | Hover/focus/active states missing on a clickable element | Add all three |
| **F7** | Hover-locked essential data | Names, prices, or specs reachable only via a hover drawer or hover-only tooltip, so touch devices cannot reach them at all | Make core data immediately visible and operable. Hover may *add*; it must never *gate* |

## Dark Mode / Theme Parity (DM1-DM5)

| ID | Pattern | Correction |
|----|---------|------------|
| **DM1** | Contrast not re-checked independently in dark mode | Good light-mode ratio guarantees nothing |
| **DM2** | Icons/illustrations merely inverted | Purpose-build for dark backgrounds; watch for halos and wrong shadows |
| **DM3** | Semantic colors (error red, success green) not re-tuned | Fix contrast on dark |
| **DM4** | Elevation/shadow system invisible or wrong on dark | Redefine for dark surfaces |
| **DM5** | Spacing/layout drift between light and dark | Resolve to one layout |

## Engineering — Source-Code Slop (E1-E58)

Design-surface patterns are only half the artifact. A page can pass every V/T/C/L/M check and
still be source-code slop: state that fights React, effects that race, types that assert rather
than check, secrets in the client bundle. This category scans the **source**, not the render.

Scope: React / Next.js / TypeScript / Tailwind codebases. Patterns are detected by reading the
source; several also produce a runtime symptom (a warning, a race, a leak) that confirms them.

### State and effects (E1-E6)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **E1** | Derived state in `useState` + `useEffect` | A value is stored in state, then kept in sync with a prop or another state by an effect. The value was always computable | Source | Compute it during render. If it is expensive, `useMemo` — not state plus an effect. The effect adds a render, a stale frame, and a second source of truth |
| **E2** | Duplicated state | The same fact lives in two places and is updated in both, so they can disagree | Source | Keep one owner; derive the rest. Two state variables describing one thing is the defect that produces the "sometimes out of sync" bug report |
| **E3** | `useCallback`/`useMemo` cargo cult | Memoization wrapped around trivial values, or around handlers passed to a component that is not memoized | Source | Memoize against a measured re-render, not by reflex. An unmemoized child makes the wrapped callback pure cost |
| **E4** | `useEffect` as an event handler | An effect watches a state flag and performs an action the user already triggered | Source | Do the work in the handler that fired the event. The effect loses the causal link and re-runs on unrelated renders |
| **E5** | Continuous input value in React state | Drag, scroll, mousemove, or pointer position written into `useState`, so every frame re-renders the tree | Source | Keep continuous values out of React state — a ref, a motion value, or a directly-set transform. React state is for discrete state |
| **E6** | Undivided context | One context holds unrelated values, so every consumer re-renders when any field changes | Source | Split by change frequency, or move to a store with selector subscriptions |

### Data fetching (E7-E12)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **E7** | Fetch in `useEffect` with no guard | An async request resolves after the component unmounted or the dependency changed, writing state into a dead tree | Source | Guard the write (`ignore` flag or `AbortController`), or use the framework's data layer instead of a hand-rolled effect |
| **E8** | No `AbortController` | In-flight requests continue after navigation or unmount, wasting the response and racing the next one | Source | Abort on cleanup. Pass the signal through to `fetch` |
| **E9** | Happy-path-only fetch | The request handles success and nothing else: no loading branch, no error branch, no empty branch | Source | All three are part of the component (S1, S2, S4). A fetch that can only succeed is a fetch that will show a blank screen |
| **E10** | No cache or dedup | Two components mount and request the same endpoint; a remount refetches everything | Source | Use the framework's cache or a query library with a key, rather than bare `fetch` in an effect |
| **E11** | `'use client'` on everything | The directive sits at the top of the route, the layout, or most components, so the whole tree ships as client JavaScript | Source | Keep the boundary at the leaf that needs interactivity. A client boundary at the root discards the framework's main advantage |
| **E12** | `async` function passed directly to `useEffect` | `useEffect(async () => {...})` — the effect returns a Promise, so the cleanup is not a function and React warns | Source | Declare an inner async function and call it, returning the cleanup from the outer synchronous function |
| **E13** | Sequential awaits where parallel is available | Independent requests awaited one after another inside a loop or a body, so latency adds up | Source | `Promise.all` for independent work. Await in sequence only where a later call genuinely depends on an earlier result |

### Lifecycle and leaks (E14-E18)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **E14** | Uncleaned timer or interval | `setInterval`/`setTimeout` with no `clearInterval`/`clearTimeout` in the effect cleanup | Source | Return the clear call. An interval that outlives its component keeps firing and keeps a closure alive |
| **E15** | Uncleaned event listener or subscription | `addEventListener`, `subscribe`, `observe`, or a socket opened with no corresponding removal | Source | Remove exactly the listener that was added, in the same effect's cleanup |
| **E16** | Unrevoked object URL or blob | `URL.createObjectURL` with no `URL.revokeObjectURL`, so the blob stays in memory for the document's life | Source | Revoke in the cleanup. Matters most for uploads and media previews, where the blobs are large |
| **E17** | Module-level mutable state holding request or user data | A top-level `let`/`const` object that caches responses, holds a token, or accumulates a list across renders and routes | Source | Keep request and user data in a request-scoped or component-scoped owner. Module state is shared across every user of the module and survives navigation |
| **E18** | Unawaited promise with no error path | A fire-and-forget async call whose rejection becomes an unhandled rejection and is never surfaced | Source | Handle it, or explicitly mark it as intentionally ignored with a comment saying why |

### Component architecture (E19-E25)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **E19** | God component | One component owns layout, data fetching, state, handlers, and rendering, usually well past 300 lines | Source | Split by responsibility: data, state, presentation. The component that does everything is the component nothing can be changed in |
| **E20** | Prop drilling through three or more layers | A value is threaded through components that do not use it, purely to reach a descendant | Source | Use composition (`children`), context, or colocate the state with the component that needs it |
| **E21** | Array index as key | `key={index}` on a list that can reorder, insert, or delete, so React reuses the wrong node's state | Source | Key on a stable identity from the data. Index keys are only safe for a list that is static and append-only |
| **E22** | Missing key | A list renders without a key at all, so React falls back to position and warns | Source | Add a stable key. Also applies to fragments in a list |
| **E23** | Boolean-prop configuration | `isLarge`, `isPrimary`, `isIcon`, `hasBorder` accumulating as props, so the component's surface is a matrix of flags | Source | Use variants, composition, or separate components. Flag matrices are how a component becomes unchangeable |
| **E24** | Inline handler recreated per row in a hot list | An arrow function defined inside the map callback for every item, in a list that re-renders often | Source | Hoist the handler and pass the identifier, or use a data attribute. Matters in lists that re-render on scroll or input |
| **E25** | No module boundaries | One directory, or one file, holds data access, business logic, and presentation with no separation | Source | Separate the layers the code actually has. PS-style "feature folders" are one answer, not the only one; the defect is having none |

### TypeScript (E26-E31)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **E26** | `any` | Explicit `any`, or an implicit one from an untyped source, flowing through the code | Source | Type it, or use `unknown` and narrow. `any` disables the checker exactly where the boundary is least trustworthy |
| **E27** | Suppression comment without a reason | `@ts-ignore` / `@ts-expect-error` with no explanation of what is being suppressed and why it is safe | Source | Fix the type, or write the reason. `@ts-expect-error` is preferable to `@ts-ignore` — it fails when the error stops occurring |
| **E28** | Absent or empty prop types | Component props typed `any`, `object`, or not at all, so every caller is unchecked | Source | Declare props explicitly. An untyped component is a component whose contract nobody can verify |
| **E29** | `as` cast to silence a mismatch | `as SomeType` applied to a value the compiler already said was something else | Source | Narrow or validate. A cast that resolves a real mismatch moves the failure to runtime |
| **E30** | Non-null assertion as a substitute for a check | `value!` on a value that can genuinely be null, especially data from a request or a DOM lookup | Source | Check it. If it truly cannot be null, encode that in the type instead of asserting it |
| **E31** | Permissive types | `string` where a union of known values belongs; every field optional; `object` for a shape | Source | Model the domain. A type that accepts anything catches nothing |

### Markup and styling (E32-E37)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **E32** | Utility class soup | Twenty or more utilities on one element, with no component or token extraction, repeated across the app | Source | Extract the element into a component or a named class. The long string is a component that has not been written yet |
| **E33** | Arbitrary values everywhere | `w-[437px]`, `mt-[13px]`, `text-[15px]` scattered through the markup instead of the scale | Source + Judgment | Snap to the scale. Arbitrary values are the Tailwind form of D3 spacing drift and D4 size drift |
| **E34** | Specificity escalation | `!important`, Tailwind's `!` prefix, or ever-deepening selectors used to win a fight | Source | Fix the source of the conflict. Escalation is a symptom; the next override will need a stronger one |
| **E35** | Inline `style` for values that belong in tokens | `style={{ color: '#2b6cb0', marginTop: 13 }}` on elements that should reference the token layer | Source | Move it into a token or a class. Inline style is D2 hardcoding wearing a different hat |
| **E36** | Duplicated utility clusters | The same eight-class string pasted into twelve places, so a change means twelve edits that will drift | Source | Extract once. This is the Tailwind form of copy-pasted logic |
| **E37** | No responsive strategy | Desktop-first classes with no `sm:`/`md:`/`lg:` breakpoints, or a single breakpoint used everywhere | Source | Define the collapse per section; test at the real breakpoints (B1-B5) |

### Security and privacy hygiene (E38-E43)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **E38** | `dangerouslySetInnerHTML` with unsanitized input | User or API content written straight into the DOM as HTML | Source | Render as text, or sanitize through a maintained library at the boundary. **Blocker** where the input is user-controlled |
| **E39** | Secret in the client bundle | An API key, token, or signing secret behind `NEXT_PUBLIC_*`, in client code, or in a `VITE_*` variable | Source | Move it server-side behind a route handler. Anything shipped to the client is public, whatever the variable is named |
| **E40** | Intermediate values and user data left in `console.log` | Logs in shipped code printing tokens, full response bodies, form values, or identifiers | Source | Remove before delivery. Logs ship to the browser console and to any error-reporting integration |
| **E41** | No validation at the trust boundary | Client-supplied values used server-side without checking shape, range, or authorization | Source | Validate at the boundary, server-side, regardless of what the client sent. The client is never the check |
| **E42** | Sensitive data in the URL, query string, or `localStorage` | Tokens, emails, or identifiers placed where they land in history, referrer headers, and logs | Source | Keep credentials out of URLs; prefer httpOnly cookies over `localStorage` for anything that authenticates |
| **E43** | Unvalidated redirect or fetch target | A redirect or request URL taken from a parameter without an allowlist | Source | Restrict to known origins or paths. This is the cheap version of an open-redirect and an SSRF |

### Performance and framework (E44-E50)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **E44** | Client boundary at the root | The whole page is a client component, so nothing streams and everything hydrates | Source | Push the boundary to the interactive leaves (E11 is the directive placement; E44 is the resulting payload) |
| **E45** | Raw `<img>` for large assets | No framework image component, no width/height, no modern format, no `loading` attribute | Source | Use the framework's image component, or at minimum explicit dimensions plus `loading="lazy"` |
| **E46** | Unsized media causing layout shift | Images, embeds, and ads without intrinsic dimensions, so content jumps as they load | Source + Render | Reserve the space. This is the largest single contributor to a failing CLS |
| **E47** | Heavy import with no dynamic boundary | A charting, editor, or 3D library imported at module top level into a route that needs it conditionally | Source | Import dynamically at the point of use so it stays out of the initial bundle |
| **E48** | Render blocked on non-critical data | The whole page waits on a request only one section needs | Source + Judgment | Stream or suspend the slow section instead of gating the page on it |
| **E49** | Google Fonts via `<link>` | A render-blocking stylesheet request to an external font CDN, with no `preconnect`, no `display=swap`, and a consequent shift | Source | Self-host via the framework font loader. Removes the third-party request and the layout shift |
| **E50** | Bundle growth with no accounting | Dependencies added per feature with no check on what they cost, and no analysis | Source | Check `package.json` against actual imports; verify the dependency is installed before importing it. Dead dependencies are pure payload |

### Testing and project structure (E51-E55)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **E51** | No test on a correctness-critical path | Money, auth, parsing, permissions, or date arithmetic with no test at all | Source (absence) | One test that fails if the logic breaks. Not a suite — the smallest check that would catch the regression |
| **E52** | Snapshot-only tests | Coverage exists but asserts only that rendered output is unchanged, so it breaks on every styling edit and catches no bug | Source | Assert on behavior: what the user sees, what the handler did. Snapshots are change detectors, not tests |
| **E53** | Tests asserting implementation detail | Assertions on internal state, private methods, or call counts that change on any refactor | Source | Assert on observable behavior, so the test survives a rewrite that preserves it |
| **E54** | Dead code and unused exports | Commented-out blocks, unreachable branches, exports nothing imports, forgotten feature flags | Source | Delete. Version control is the archive |
| **E55** | Duplicated logic | The same helper, formatter, or validation copy-pasted with small drift | Source | One implementation, one owner. The copies diverge the first time one is fixed |

### Form handling (E56-E58)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **E56** | Controlled/uncontrolled switch | An input starts uncontrolled and becomes controlled, or the reverse, so React warns and the value resets | Source | Choose one ownership model at first render and keep it. `undefined` is what triggers the switch |
| **E57** | File input value held in state | The `FileList` from a file input copied into React state, where it cannot be set back onto the input | Source | Read it on submit, or use a ref. File inputs are read-only for a reason |
| **E58** | Form state not reset after submit | Fields keep their values after a successful submit, so the user cannot tell whether it worked and may submit twice | Source | Reset on success, or navigate away. Pair with the success feedback in F3 |

**Meta-tells.** These do not fire individually — they are the gestalt read of a source tree.
Four or more together means the code was generated without a human review pass: a component
file that is also its own data layer, `any` at every boundary, fetch inside `useEffect`,
`console.log` still present, unused exports, index keys, `'use client'` on the root, and no
test on a path that matters. Report the individual E-IDs; use the meta-read only to set the
severity and to justify a whole-file rewrite over a patch.

Full code-pattern detail, per-framework notes, and the enforcement checklist:
`references/code-slop-guide.md`.

---

## Hospitality / Luxury (H1-H18)

Scope: ultra-luxury hospitality, Michelin-grade restaurants, architectural destination
venues only. SaaS, dashboards, dev tools, and aggregators do not trigger these.

**ID blocks:** forbidden patterns are H1-H7 and H13-H15 (detected by presence); mandatory
codes are H8-H12 and H16-H18 (detected by absence of the required treatment). IDs are
stable — new patterns append, they never renumber.

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **H1** | Card-grid aggregation | Rows of identical rounded cards: cropped image header + plain text body. Second tell: box-in-a-box thumbnail (photo with its own radius inside a card that already has border + radius) | Source | Full-bleed canvas, asymmetric split viewport, or editorial index. One frame, not two |
| **H2** | Generic status pills | Pill with pulsing green/red dot: "Open Today • 12:00 PM – 11:30 PM", "Live", "Active Now" | Source | Static typographic ledger line — `Open · 18:00–23:00`. Never animate the indicator |
| **H3** | Fake chrome around media | Mock window headers, tab strips with close buttons, `Sky Hi Dining • Live Map` bars around video/maps | Source | Media as a full-bleed architectural plane. No simulated OS or browser UI — unless the product genuinely is an OS simulator |
| **H4** | Raw embedded maps | Unstyled Google Maps — pastel roads, yellow highways, cyan water — in a dark palette | Source | Monochromatic CSS filter + dark overlay, or purpose-built vector/static cartography |
| **H5** | Circular icon lists | Icon-in-circle beside address/contact/hours rows (pin, telephone, clock, car) | Source | Tabular concierge ledger, architectural key-value rows, or editorial prose |
| **H6** | Fintech sans pairing | Outfit/Poppins/Inter/DM Sans/Montserrat against a luxury serif | Source | Plus Jakarta Sans, Tenor Sans, Syne, Manrope. The sans defers to the serif. This is a *pairing* rule — Inter is legitimate for a clean/technical identity |
| **H7** | Hover-gated core content | Dish photos, ingredients, tasting notes, pricing inside a hover reveal | Source | Confident static presence. Hover adds; it never gates. Also unreachable on touch |
| **H8** | Canvas over card (absence) | Boxed containers where full-bleed would serve; no hairline architectural borders | Source | Full-bleed planes, `1px solid rgba(223, 191, 108, 0.18)` hairlines, asymmetric split viewports |
| **H9** | Editorial typographic scale (absence) | Numerals/coordinates/labels set as incidental text | Source | Roman or serif numerals ("01"/"02"), tracking `0.2em`, small caps |
| **H10** | Discreet dietary indicators (absence) | Heavy VEG/NON-VEG badges, or the FSSAI bordered square with green/red dot | Source | 4-6px glowing jewel dot in a glass disc. Safety-mandated labelling exempt |
| **H11** | Cinematic media integration (absence) | Background video with hard edges, no scrim, or a scrim fighting the palette; fast-cut high-contrast footage | Source | Linear + vignette scrims bleeding into the palette. Footage slow, atmospheric, low-contrast |
| **H12** | Tabular concierge hierarchy (absence) | Contact/arrival data as prose or icon rows | Source | Concierge dossier: key left in tracked uppercase sans, Cormorant Garamond value right |
| **H13** | Commerce price and tag pills | A price (`₹200`) or a tag/category trapped in a rounded pill, chip, or grey button box | Source | Lightweight text labels, subtle dots, or an integrated typographic scale — not a control |
| **H14** | Retail menu chrome | Search toolbar with magnifier, pill filter chips, "Expand All" toggle on a dining menu | Source | Editorial category index, chapter curtain, or video chapter transition |
| **H15** | Spatial/factual hallucination | "Rooftop venue", "high-rise", "skyline balcony" claimed for a ground-level estate; invented floors, views, capacity | Judgment | Diff every claim against the brief. Silent brief = omit the claim. **Blocker, no exemption** |
| **H16** | Architectural signature footer (absence) | Generic 3-column template: tiny corner logo, plain link lists, floating circular SVG social icons | Source | Masthead, direct telephone booking line (`tel:`), tabular nav, low-opacity watermark brand stamp |
| **H17** | Curated third-party grading (absence) | Any embed left in its own palette, or placed raw beside a contact card in a 50/50 box split | Source | Grade to palette, frame asymmetrically. `filter: invert(92%) hue-rotate(185deg) saturate(35%) brightness(78%) contrast(125%);` |
| **H18** | Page-archetype uniformity (absence) | One generic template site-wide: 3 feature boxes on events, 3×3 grid on gallery, static paragraphs + icon bullets on experience, every section a 50/50 split | Source | Per-page venue-logic architecture (section ledger in `luxury-hospitality-protocol.md`) |

**Supersession — report one ID, never both:** H1>L2/V6/I5/L4, H2>V12/M4/M6, H4>C1/C4,
H5>A4/L9, H6>T8/V14, H8>L4, H9 inverse of T5/L5, H10>V12, H13>V12, H16>L2/V8/V9,
H18>L2/L3/L4/L10. H4 fires for a raw default-styled map, H17 for every other embed — never
both for one element. H3, H7, H11, H14, H15, H16, H17 have no base equivalent.

**Fix-order placement:** H15 → Stage 1. H4/H7/H11/H14/H17 → Stage 2. H1/H3/H5/H8/H12/H16/H18
→ Stage 3. H6/H9 → Stage 4. H2/H10/H13 → Stage 5. Never fix H2/H10/H13 before H1 or H18.

Full heuristics, section-by-section archetype ledger, exemption rules, and report rows:
`references/luxury-hospitality-protocol.md`.

---

## Generic ↔ Hospitality Mapping

The H-patterns are the hospitality-scoped case of generic defects. Report the generic ID
outside that domain, the H-ID inside it — never both.

| Generic | Hospitality | Relationship |
|---------|-------------|--------------|
| V19 Ungraded third-party embed | H17 (any embed), H4 (maps) | Same defect, house-palette severity |
| V20 Unscrimmed ambient video | H11 | H11 adds the slow/atmospheric footage rule |
| F7 Hover-locked essential data | H7 | H7 names dish photos, ingredients, tasting notes |
| V18 Border-as-hierarchy crutch | H8 (inverse) | H8 is the positive framing: canvas over card |
| L10 Default 50/50 split reflex | H18 | H18 is page-level; L10 is the section-level symptom |
| X5 Page-level archetype uniformity | H18 | Same defect; X5 is the generic scope, H18 the hospitality one |
| L13 CTA competition | H2, H13 | H2/H13 are the venue-status and price forms of one-action-per-view |
| V12/V13 capsule badges, generic type | H10, H13, H14 | H13 covers prices/tags; H10 covers dietary |
| L9 Non-semantic markup | H5 | H5 when the icon list encodes contact/venue data |

---

## Convergence Cross-Reference

Patterns that frequently co-occur — fix together to prevent cosmetic churn:

- **V6 + L4** (Extreme radius + Nested cards): Fix L4 first — removing the nested surface often resolves V6 by eliminating the card that needed extreme radius.
- **C1 + T8** (AI palette + Overused font): Fix T8 first by strengthening existing font roles — color choices become more deliberate as a result.
- **M1 + V2** (Bounce easing + Glassmorphism): Fix V2 first — the harder bounce reads as less jarring on solid surfaces.
- **V3 + L2** (Side-tab accent + Identical card grids): Fix L2 first — the accent border becomes redundant.
- **P2 + P4** (Buzzword + Theater framing): Fix P2 first with exact action/object/user/result — theater framing loses its audience when claims are concrete.
- **V18 + V4** (Border-as-hierarchy + Hairline with wide shadow): Both are borders doing a hierarchy job. Fix V18 first by moving hierarchy into type — the shadowed hairline usually disappears with it.
- **V19 + L10** (Ungraded embed + 50/50 split): The 50/50 split is why the embed is boxed at all. Fix L10 first — re-framing asymmetrically changes what the embed needs to match.
- **M9 + M2** (Blanket `transition: all` + Layout-property animation): The blanket transition usually hides the reflow offender. Fix M9 first by naming properties — the animated layout property surfaces immediately.
- **M13 + M5** (Untuned curve + Uniform duration): Curve and duration are separate axes. Setting one while the other stays default still reads as untuned; set both tokens together.
- **C7 + DM1** (Pure-black surface + Dark-mode contrast): A `#000` floor makes every contrast ratio fail against a different frame than the one it was tuned on. Lift the floor first, then re-check.
- **T18 + D4** (No scale + Drift from scale): Define the scale before chasing drift, or the drift has no reference to be measured against.
- **L11 + L2** (Orphan cell + Identical card grid): The orphan cell is usually the grid's last symptom. Fix L2 first — removing the grid removes the cell that had to be filled.
- **L12 + L13** (Hero overflow + CTA competition): Same crowding problem. Decide the single primary action (L13) first; the hero usually shortens enough to clear the fold (L12).
- **E7 + E9 + E14** (Unguarded fetch + happy path + uncleaned timer): One effect written without cleanup and without error branches produces all three. Rewrite the effect rather than patching each symptom.
- **E19 + E25** (God component + no boundaries): The god component is what a missing boundary looks like from the file. Fix E25 first — the split is what E19 was asking for.
- **E32 + E33 + E36** (Class soup + arbitrary values + duplicated clusters): All three are the same missing extraction. Extract the component once and the other two usually disappear with it.
- **E38 + E41** (Unsanitized HTML + no boundary validation): Compound. A trust boundary with no validation feeding unsanitized HTML is a live injection path, not two style findings. Fix both in one change.
- **E26 + E29 + E30** (`any` + `as` + `!`): Each is an escape hatch from the same unchecked boundary. Typing the boundary once closes all three.
- **C11 + C5 + T8** (Premium-consumer palette + cream reflex + overused font): The three roles of one unexamined "tasteful" default. Fix the palette decision first — the font choice usually follows the world it belongs to.

*After each system-level change, rescan. One primitive edit can clear many local symptoms.*

---

## Fix Order (apply sequentially)

1. **Broken behavior, accessibility, overflow, readability, and correctness** — Q1-Q10, C4, C6, C7, L6-L9, L12, M2, M9, A1-A16. Source-side: E7, E8, E12, E14-E18 (races, uncleaned lifecycle, silent failures), E38, E39, E41-E43 (security and privacy — all blocker-class where the input is user-controlled), E56-E58 (form ownership and reset). Includes Q9 (contract-breaking refactor) and, in the hospitality domain, H15 (fabricated venue claims).
2. **Information architecture and unsupported content** — X3, X6, P2, P4, I1, L10, L13, V19, V20, F7. Source-side: E9, E11, E44, E46 (missing states, client boundary at the root, payload and shift). In hospitality: H4, H7, H11, H14, H17.
3. **Repeated page templates, card anatomy, container depth** — L2, L4, L11, V2, V6, X5, X7-X10. Source-side: E19-E25 (component architecture — the render-side repetition usually has a component-side cause). In hospitality: H1, H3, H5, H8, H12, H16, H18.
4. **Type hierarchy, spacing rhythm, color roles** — T1-T18, C1-C5, C8-C11, L3, L5. Source-side: E1-E6, E26-E31 (state correctness and type quality — fix after the structure is settled, since a rewrite invalidates both). In hospitality: H6, H9.
5. **Decorative borders, gradients, glows, radii, icons, motion** — V1-V8, V18, V21, M1-M4, M14-M18. In hospitality: H2, H10, H13.
6. **Copy cadence and redundant labels** — P1-P9.
7. **Imagery and placeholder issues** — I1-I6.
8. **Motion and layout violations** — M5-M8, M10-M13. Source-side: E32-E37 (markup and styling — the cosmetic layer, last by design).

**Never apply a later stage before an earlier one.** Fixing a decorative border (Stage 5)
before fixing overflow (Stage 1) just produces a new overflow.

---

## Replacement Principles

- **Structure follows content**: comparable items suit tables/aligned lists; sequences suit steps; narrative content suits editorial flow; interactive choices suit controls with clear boundaries
- **Hierarchy uses a system**: establish explicit roles for display, heading, body, label, data; single font family supports hierarchy through size/weight/width/optical size/spacing; second family earns place through clear role
- **Hierarchy is typographic, not structural**: reach for size, weight, letter-spacing, and muted opacity (`0.6`–`0.8`) before reaching for a border, divider, or box
- **Color carries a job**: assign colors to brand, action, status, emphasis, surfaces; repeated decorative gradients/glows dilute those jobs
- **Spacing communicates relationship**: related items sit close; sections receive larger separation; repeated spacing tokens useful when semantic roles differ visibly
- **Motion communicates state, continuity, or causality**: default surfaces stay still; one meaningful transition carries more value than many ornamental effects
- **Decoration carries meaning**: domain artifacts, real product UI, data, photography, diagrams, brand motifs provide specificity; generic icons/blobs disappear when carrying no information
- **Asymmetry follows priority**: give more space to more important content; avoid arbitrary card-size variation created solely to appear designed
- **Motion is a budget, not a garnish**: high-frequency interactions resolve instantly, occasional interactions get polish, rare moments may carry delight. Spend accordingly — motion applied uniformly across all three reads as noise
- **Numbers are read in columns**: tabular figures wherever digits align or update in place
- **Neutrals belong to the brand**: tint them toward the hue and never let a dark surface start at pure black
- **One action per view**: a view with two equal primaries has none
- **Preserve the contract**: never rename, remove, or restructure classes, IDs, or data-attributes that the runtime depends on
