---
name: slop-audit
description: Detect and flag AI-generated low-quality or misleading content (slop) in UI and code. Runs a 4-pass scan — quality failures, 245 catalogued patterns (V/T/C/L/M/P/I/Q/X/A/D/S/R/B/F/DM/H/E), composition checks, source pass — tracks findings in slop-log.json with confidence, severity and auto-fix flags, applies an 8-stage fix order, and gates delivery on the pre-emit checklist in references/design-spec.md (motion tokens, OKLCH colour rules, type roles). Core objective across all domains: flag low-effort AI-template tropes, boilerplate Bootstrap/SaaS defaults, and unstyled third-party widgets. Use when auditing a codebase or frontend for generic AI-generated design, reviewing UI output for slop, checking motion quality (transition: all, ease-in entrances, animated high-frequency actions, scale(0) entrances), scanning source code for React/TypeScript slop (fetch-in-useEffect, uncleaned effects, any/as/! escapes, dangerouslySetInnerHTML, secrets in the client bundle, god components, array-index keys, class soup), checking a refactor for broken JS contracts (Q9), or when the target is luxury hospitality, fine dining, or a destination venue (activates the H1-H18 domain catalog: card-grid and price-pill aggregation, retail menu chrome, fabricated venue claims, signature footer, graded embeds, page-archetype uniformity).
version: "3.5.0"
argument-hint: "[--clear] [--list] [--scan PATH]"
allowed-tools:
  - Read
  - Write
  - Bash
  - Grep
  - Glob
  - AskUserQuestion
---

<objective>
Provide comprehensive functionality to identify, track, and manage AI-generated content that may be low-quality, repetitive, or misleading — commonly referred to as "AI slop". This skill helps maintain content quality by detecting slop patterns, tracking them persistently with severity scoring and auto-fix tagging, and offering remediation actions. Extends the pass-based scan system with new categories (Imagery I2-I6, Motion M4-M18, Copy P5-P9, Accessibility A1-A16, Design-Token D1-D6), severity-based trending scores, and cross-skill debt-integration. Integrates lessons from anti-ui-slop (ritmex-skills), anti-slop (miqdadbadjer/anti-slop), and the motion principles popularised by Emil Kowalski (transform/opacity-only, named transitions, duration scaled to distance and frequency). Adds six generic patterns valid in every domain — V18 border-as-hierarchy crutch, V19 ungraded third-party embed, V20 unscrimmed ambient video, L10 default 50/50 split reflex, F7 hover-locked essential data, and Q9 contract-breaking refactor (blocker, Stage 1) — the H13-H18 hospitality block, a generic↔hospitality mapping, a section-by-section archetype ledger, and an explicit clause that H15 (fabricated venue facts) has no exemption and outranks every visual finding. v3.4.0 appends 17 more generic patterns — motion M9-M13 (blanket `transition: all`, animated high-frequency actions, ease-in entrances, motion-only state indication, untuned easing curve), colour C7-C10 (pure black/white surfaces, zero-chroma neutrals, full-chroma dark fills, unassigned colour roles), typography T16-T18 (proportional numerals in tabular context, untracked uppercase labels, no modular scale), layout L11-L13 (orphan grid cells, above-fold hero overflow, CTA competition), composition X5-X6 (page-level archetype uniformity, no visual anchor) — and pairs the catalog with `references/design-spec.md`, the construction-side workflow, motion/colour/type token tables, and blocking pre-emit gate. v3.5.0 adds the **source-code surface**: a new Engineering category E1-E58 covering React state and effects, data fetching, lifecycle and leaks, component architecture, TypeScript quality, markup and styling, security and privacy hygiene, performance and framework handling, testing and project structure, and form ownership — plus A9-A16 (code-level accessibility: clickable divs, positive tabindex, missing focus traps, unlabelled icon controls, unassociated errors), C11 (premium-consumer palette reflex), M14-M18 (physical motion: `scale(0)` entrances, keyframes where a transition belongs, center-scaled popovers, compositor-breaking technique, ungated hover motion), V21 (geometric mask faking an organic cutout), Q10 (unthemed browser surfaces), X7-X10 (unbroken zigzag, split-header reflex, unearned production chrome, numbered-pagination chrome). Paired with `references/code-slop-guide.md` — grep signatures, per-framework equivalents, and the enforcement checklist.
</objective>

<role>
Audit as an elite Creative Director and Lead UI/UX Frontend Architect.

**Core objective, all domains:** flag low-effort "AI-generated template" tropes, boilerplate
Bootstrap/SaaS defaults, and unstyled third-party widgets. Every layout must feel
deliberate, bespoke, and tailored to the actual product domain. A pattern correct for one
product is a defect in another — judge fit, not tidiness.

**Tightened standard for ultra-luxury hospitality, Michelin-grade culinary estates, and
architectural web design:** judge against bespoke, editorial haute-hospitality standards —
not the grammar of automated website builders, SaaS dashboards, and food-delivery
aggregators. The domain question for every finding is **"does this say *this place exists*,
or *this place is listed*"**. Anything that reads as a listing is the defect, however
polished.
</role>

<routing>

| Flag | Action | Description |
|------|--------|-------------|
| (none) | Scan mode | Scan files for slop indicators |
| --clear | Clear resolved slop entries | Remove tracked slop items from storage |
| --list | List tracked slop items | Show all previously identified slop with status, grouped by category |
| --scan PATH | Scan specific path | Scan a specific directory path |

</routing>

<process>

<step name="init_context">
Ensure directories exist in the project being scanned (current working directory):
```bash
# All data stored in the current working directory — never in a global/home path
mkdir -p ./.slop-tracker
mkdir -p ./.slop-tracker/references
mkdir -p ./.slop-tracker/notes
```
</step>

<step name="scan">
Scan files in the specified path (or current directory) for AI slop indicators using four passes. Passes 1-3 read the **rendered artifact** — every finding in them is judgeable from a screenshot. Pass 4 reads the **source** that produced it, and none of it is visible in the render. Run all four — a deliverable can pass every render-side check and still fail E. A deliverable is not done because the screenshot is good.

### Pass 1: Quality Failures
Check for issues that reduce accessibility, readability, robustness, or performance:
- Poor color contrast (WCAG AA: 4.5:1 for normal, 3:1 for large text)
- Text touching viewport edge on mobile
- Justified text without hyphenation support
- Low contrast text on colored backgrounds
- Skipped heading levels in rendered outline
- Tight line height below 1.3 for body text
- Body text below 12px readability risk
- Wide letter spacing on body paragraphs (>0.05em)
- Non-functional interactive elements (buttons that do nothing)
- Happy path only design (no empty, loading, or error states)
- Irrelevant FAQ with generic template questions
- Assumed logo & profile photos without instructions
- Navbar links to nowhere
- File/CSS patching via scripts
- Contract-breaking refactor (Q9): classes, IDs, or data-attributes renamed/removed while JS, data fetching, or event listeners still reference them. **Blocker** — ships a silent runtime regression, not a visual one. Diff the selectors the JS uses against the markup before clearing a style refactor
- Above-the-fold hero overflow (L12): at 1440×900 the hero headline or primary CTA falls below the fold, or the header exceeds ~80px or wraps to a second row
- Blanket `transition: all` (M9): transition declared on every property rather than the properties meant to animate — animates unintended properties and makes timing unpredictable. Sibling of M2 (layout-property animation); a blanket transition usually hides one

**Source-level failures** (read the code, not the render — full detail in `references/code-slop-guide.md`):
- Uncleaned effect (E14-E17): timer, interval, event listener, subscription, or object URL with no cleanup — keeps firing and keeps its closure alive after unmount
- Unguarded async (E7, E8, E12, E18): fetch or promise resolving into an unmounted tree, no abort, no error path
- Happy-path-only request (E9): no loading, error, or empty branch on a fetch
- Secret reachable from client code (E39): key, token, or signing secret behind `NEXT_PUBLIC_*`, `VITE_*`, or in client source. **Blocker**
- `dangerouslySetInnerHTML` on user-controlled input (E38): **Blocker**
- No validation at a trust boundary (E41): client-supplied values used server-side unchecked. **Blocker**
- Type escape at a boundary (E26, E27, E30): `any`, an unexplained suppression comment, or `!` standing in for a real check
- Client boundary at the root (E11, E44): `'use client'` on the route or layout, so the whole tree ships as client JavaScript
- `console.log` of user data, tokens, or response bodies left in shipped code (E40)

### Pass 2: Slop Signals (Catalogued Patterns)
Check for 245 AI slop patterns across categories:

#### Visual Details
- V1: Border accent on rounded element (>=2px border + >=12px radius)
- V2: Glassmorphism everywhere (translucent cards + backdrop-filter: blur)
- V3: Side-tab accent border (colored border >=3px on card side)
- V4: Hairline border with wide shadow (1px border + >=20px shadow blur)
- V5: Repeating-gradient stripes on surfaces
- V6: Extreme border radius (>=24px turns cards into blobs)
- V7: Amateurish hand-drawn SVG (complex inline SVG with rough geometry)
- V8: Generic AI icons (sparkle, star, magic, lightning, diamond, robot, AI orb)
- V9: Lucide icons (every icon from same thin-stroke library)
- V10: Colored left stripe (thin vertical bar on card/section edges)
- V11: Small arrows (→/↗) on almost every button as decoration
- V12: AI capsule badges (pill shape, thin border, glow, uppercase "AI Powered", etc.)
- V13: Generic AI typography (large monospace headings, HOW IT WORKS uppercase with wide tracking)
- V14: Typeface chosen without reason (font picked as AI default, not brand fit)
- V15: Generic illustrations (Undraw, Storyset, 3D blob with no product connection)
- V16: Mixed icon styles in one view (outline + filled + duotone)
- V17: Leftover placeholder images (lorem-picsum, unsplash-random) in production build
- V18: Border-as-hierarchy crutch (a border or outline drawn around every text block, card, and label instead of establishing hierarchy typographically). Fix: font size, weight, letter-spacing, muted opacity `0.6`–`0.8`. Reserve borders for structural separation
- V19: Ungraded third-party embed (raw default-styled map, calendar, calculator, or booking/chat widget dropped into a custom palette with no filter stack, overlay, or vendor theme)
- V20: Unscrimmed ambient video (video as section underlay with no gradient overlay, so foreground text contrast varies frame to frame). Fix: linear **and** radial scrims verified for AAA contrast across every frame
- V21: Geometric mask faking an organic cutout (circle, polygon, or radial-gradient cutout approximating a photographic subject's edge — reads worse than omitting the effect). Fix: derive an alpha matte from the actual image, or produce a real cut-out asset

#### Typography
- T1: Flat type hierarchy (adjacent roles differ <1.25x, lack contrast)
- T2: Icon tile stacked above heading (repeated feature cards)
- T3: Italic serif display headline (serif italic >=32px in generic startup hero)
- T4: Hero eyebrow or pill chip (tracked uppercase text above hero heading)
- T5: Repeated section kickers (3+ sections with same uppercase tracked label)
- T6: Oversized hero headline (8+ words with >=48px or text-5xl styling)
- T7: Crushed letter spacing (display tracking < -0.05em, body with negative tracking)
- T8: Overused font (Inter, Geist, Space Grotesk, Instrument Serif as default identity)
- T9: Single font for everything (one family with identical weight/width/spacing across roles)
- T10: All-caps body text (paragraphs/long blocks >=20 words in uppercase)
- T11: "It's not just X, it's Y" / "more than just a Z" construction
- T12: Buzzword stacking (seamless, leverage, unlock, elevate, robust, cutting-edge)
- T13: Em-dash overuse as a sentence-structure crutch (3+ em-dashes in body block)
- T14: Inflated fake urgency/stats with no source ("only 3 spots left", "10,000+ happy customers")
- T15: Rule-of-three headline tic ("Fast. Reliable. Secure.")
- T16: Proportional numerals in tabular context (metrics, prices, dates, table columns without `font-variant-numeric: tabular-nums`, so digit widths vary and columns jitter)
- T17: Uppercase label without compensating tracking (uppercase micro-labels at default or negative tracking; needs 0.05–0.12em). Q8 is the inverse defect — wide tracking on running body copy
- T18: No modular type scale (sizes chosen ad hoc, no ratio relating adjacent roles). D4 is the *drift* case — a defined scale components ignore; report T18 only when no scale exists

#### Color and Contrast
- C1: AI color palette (purple/violet gradients or cyan-on-dark without brand evidence)
- C2: Dark mode with glowing accents (dark surfaces with colored box shadows/neon text)
- C3: Gradient text (background-clip: text + transparent fill on headings/metrics)
- C4: Gray text on colored background (neutral gray on chromatic surface, loses contrast)
- C5: Cream/beige reflex (warm off-white as whole page's generic surface without palette)
- C6: Contrast failing WCAG AA (<4.5:1 normal text, <3:1 large text)
- C7: Pure black/white surfaces (`#000` background or `#fff` surface used as-is, no tint, no lifted floor). Fix: tint neutrals toward the brand hue, lift the dark floor — `oklch(0.18 0.012 250)`, not `#000`
- C8: Zero-chroma neutrals against a chromatic brand (neutral ramp at 0 chroma while brand hue is saturated — greys read as a second, unrelated system). Fix: a small chroma at the brand hue, well under half the brand's chroma
- C9: Full-chroma accent fill on a dark surface (brand accent at maximum saturation as a large fill, vibrating against the surface). C2 is *glow* (shadow, neon text); C9 is *fill saturation*
- C10: Palette with no role assignment (colours exist as a flat hex list; nothing distinguishes brand, action, status, emphasis, surface). Detected by absence — report when one chromatic colour serves two jobs, or status and action share a colour
- C11: Premium-consumer palette reflex (the same "tasteful" warm-neutral-plus-terracotta scheme with no brand evidence: backgrounds `#f5f1ea`/`#f7f5f1`/`#fbf8f1`/`#efeae0`/`#ece6db`/`#e8dfcb`, accents `#b08947`/`#b6553a`/`#9a2436`/`#9c6e2a`/`#bc7c3a`/`#7d5621`, ink `#1a1714` family). C5 is the surface-only cream reflex; C11 is the full three-role scheme — report one, never both. Fix: pick a named, committed world

#### Layout and Space
- L1: Hero metric layout (centers large number, tiny label, supporting stats with generic treatment)
- L2: Identical card grids (4+ siblings with identical icon/heading/paragraph sizing)
- L3: Monotonous spacing (same gap/padding token for item/group/component/section boundaries)
- L4: Nested cards (3+ nested levels repeating borders/fills/radii/shadows)
- L5: Numbered section markers (01/02/03 labels for independent sections)
- L6: Line length too long (>80 chars per line running text)
- L7: Content overflowing its container (text/media spills, clips, accidental horizontal scroll)
- L8: Positioned child clipped by overflow container (tooltip/menu/popover cut by overflow:hidden/clip)
- L9: Non-semantic markup (div-soup instead of button/nav/main/header)
- L10: Default 50/50 split reflex (section after section resolves to a predictable 50% text / 50% media split). Fix: vary the rhythm — full-width banner, asymmetric 40/60, tabular data rows, interactive multi-panel viewport
- L11: Orphan or decorative grid cell (grid cell count ≠ item count: an empty cell remains, or a filler cell was inserted to square the layout). Fix: redistribute the item span or change structure — a bento is composed *from* content, never filled *to fit*
- L12: Above-the-fold hero overflow (hero headline or primary CTA below the fold at 1440×900, or header exceeding ~80px / wrapping to a second row). Render check
- L13: CTA competition (two or more CTAs at equal visual weight, or two expressing the same intent — "Get started" beside "Start free trial")

#### Motion
- M1: Bounce or elastic easing (dialogs/cards/routine controls with overshoot/spring effects)
- M2: Layout-property animation (width/height/padding/margin/top/left causing reflow)
- M3: Image hover transform (generic cards repeatedly scale/rotate on hover)
- M4: Blanket scroll-fade-in applied to every section indiscriminately
- M5: Uniform 300ms ease-in-out duration regardless of element size/distance
- M6: Reflexive hover:scale-105 / lift-shadow on non-interactive cards
- M7: No respect for prefers-reduced-motion
- M8: Staggered-list-reveal cliché applied to non-list content
- M9: Blanket `transition: all` (transition declared on every property rather than the ones meant to animate). Fix: name them — `transition: transform 160ms, opacity 160ms`
- M10: Motion on a high-frequency action (keyboard shortcut, search, filter, tab switch, row hover, or menu open animating long enough to be felt as lag). Fix: resolve in roughly 100–150ms, or immediately
- M11: Entrance on ease-in or over 300ms (entering elements should decelerate and stay short; ease-in belongs on exits)
- M12: Motion-only state indication (a state change carried solely by animation, with no persistent static difference once motion ends). A7 is the colour-only case; M12 is the motion-only case
- M13: Untuned default easing curve (every state transition using the browser default `ease` or `linear`, no curve chosen per interaction). M5 covers the duration axis, M13 the curve axis — a page can fail either alone
- M14: Entrance from `scale(0)` (element appearing from nothing, or a pure opacity pop with no scale offset). Fix: start at `scale(0.9–0.97)` plus `opacity: 0` — the non-zero start is what reads as physical
- M15: Keyframes where a transition belongs (a retriggerable element — toast, toggle, dynamic list — animating via `@keyframes`, so an interrupted run restarts from zero instead of retargeting). Fix: transitions interrupt and retarget; reserve keyframes for predetermined one-shot motion
- M16: Popover scaling from its own center rather than from the trigger that opened it. Fix: set `transform-origin` to the trigger. Modals are the exemption — centered in the viewport, they keep `transform-origin: center`
- M17: Compositor-breaking animation technique (child transforms driven by a CSS variable on the parent, `will-change` left on permanently, or a rAF loop writing into React state). Fix: set `transform` directly on the animating element, scope `will-change` to the animating window
- M18: Ungated hover motion (hover transform that also fires on touch, where the tap produces a false hover and the element moves under the finger). Fix: gate behind `@media (hover: hover) and (pointer: fine)`

#### Copy
- P1: Em-dash overuse (3+ em-dash characters in body block)
- P2: Marketing buzzword (supercharge, streamline, empower, world-class, enterprise-grade, seamlessly, unlock)
- P3: Aphoristic cadence (Multiple sections repeat Not X. Y. or Less X. More Y. patterns)
- P4: Theater framing copy (Marketing dismisses category as theater without explaining practical failure)
- P5: "It's not just X, it's Y" / "more than just a Z" construction
- P6: Buzzword stacking (supercharge, leverage, unlock, elevate, robust, cutting-edge)
- P7: Em-dash overuse as a sentence-structure crutch
- P8: Inflated fake urgency/stats with no source ("only 3 spots left", "10,000+ happy customers")
- P9: Rule-of-three headline tic ("Fast. Reliable. Secure.")

#### Imagery
- I1: Broken or placeholder image (missing/empty src, #, known placeholder services, placeholder API paths)
- I2: Generic stock-photo tells (diverse-team-laughing-at-laptop, handshake close-up, lightbulb-idea shot)
- I3: AI-image generation artifacts (garbled embedded text, malformed hands, uncanny facial symmetry, inconsistent lighting/shadows)
- I4: Mixed icon styles in one view (outline + filled + duotone)
- I5: Inconsistent aspect ratios/crops across a grid or gallery
- I6: Leftover placeholder images (lorem-picsum, unsplash-random) in production build

#### General Quality
- Q1: Cramped padding (text/controls within ~8px of bordered/colored edge)
- Q2: Body text touching viewport edge (especially on mobile)
- Q3: Justified text without effective hyphenation/language support
- Q4: Low contrast text (misses WCAG AA: 4.5:1 normal, 3:1 large text)
- Q5: Skipped heading level (rendered outline jumps h1 to h3 or similar)
- Q6: Tight line height (body text below 1.3, display headings role-based exceptions)
- Q7: Tiny body text (below 12px, 12-13px readability risk)
- Q8: Wide letter spacing on body (tracking above 0.05em)
- Q9: Contract-breaking refactor (existing HTML classes, IDs, or data-attributes renamed, removed, or restructured while JS, data fetching, or event listeners depend on them). **Blocker.** Preserve every technical hook; if a rename is genuinely required, update both sides in the same change
- Q10: Unthemed browser surfaces (text selection, caret, scrollbar, focus ring, underline offset, and tabular numerals still carrying browser defaults, so the parts the designer did not draw belong to no design system). Fix: theme them from the palette — the cheapest signal that a page was built rather than assembled

#### Hospitality / Luxury Domain (applies only when the target is luxury hospitality, fine dining, or a destination venue — do not report outside that scope)
ID blocks: forbidden patterns H1-H7, H13-H15 (detected by presence); mandatory codes H8-H12, H16-H18 (detected by absence). IDs are stable across versions — never renumber.

Forbidden patterns:
- H1: Card-grid aggregation (rows of identical rounded cards with cropped image header + plain text body). Second tell: box-in-a-box thumbnail (photo with its own radius inside a card that already has border + radius). Uniform `border-radius: 12-16px` + generic shadow + flat fill applied to halls, gardens, dishes alike reads as an admin tool
- H2: Generic status pills (pill with pulsing green/red dot: "Open Today • 12:00 PM – 11:30 PM", "Live", "Active Now") — reads as an automated server health monitor
- H3: Fake chrome around media (mock window headers, tab strips with close buttons, "Sky Hi Dining • Live Map" bars around video/maps)
- H4: Raw embedded maps (unstyled Google Maps, pastel roads, yellow highways, cyan water inside a dark palette)
- H5: Circular icon lists (icon-in-circle beside address/contact/hours rows — pin, telephone, clock, car)
- H6: Fintech sans pairing (Outfit, Poppins, Inter, DM Sans, Montserrat set against a luxury serif)
- H7: Hover-gated core content (dish photos, ingredients, tasting notes, pricing hidden behind a hover reveal — also unreachable on touch, where no hover exists)
- H13: Commerce price pills (price `₹200` trapped in a rounded pill, chip, or grey button box — checkout-counter grammar)
- H14: Retail menu chrome (search toolbar with magnifier, pill filter chips, "Expand All" toggle on a dining menu)
- H15: Spatial/factual hallucination ("rooftop venue", "high-rise", "skyline balcony" claimed for a ground-level estate; invented floors, views, amenities, capacity). **Blocker. No exemption.** Always Candidate at source level — escalate only after diffing against the brief

Mandatory design codes (violation = the required treatment is absent):
- H8: Canvas over card (missing full-bleed backgrounds, hairline borders `1px solid rgba(223, 191, 108, 0.18)`, asymmetric split viewports; boxed containers instead)
- H9: Editorial typographic scale (numbers, coordinates, labels not treated as design; missing Roman/serif numerals "01"/"02", tracking 0.2em, small caps)
- H10: Discreet indicators (heavy VEG/NON-VEG badges — or the FSSAI bordered square with green/red dot — instead of 4-6px jewel dots framed in glass discs)
- H11: Cinematic media integration (background video without calibrated linear + vignette gradient scrims bleeding into the dark palette; also fast-cut or high-contrast footage — must stay slow, atmospheric, low-contrast)
- H12: Tabular concierge hierarchy (arrival, contact, venue data not presented as key-value dossier: category key left, Cormorant Garamond serif values right)
- H16: Architectural signature footer (missing masthead, direct telephone booking line via real `tel:` link, tabular nav, and large low-opacity background watermark stamp; generic 3-column template with floating SVG social circles instead)
- H17: Curated third-party grading (any embed — map, review strip, booking widget, social feed — left ungraded in its own palette, or placed raw beside a contact card in a 50/50 box split). Equation: `filter: invert(92%) hue-rotate(185deg) saturate(35%) brightness(78%) contrast(125%);`
- H18: Page-archetype uniformity (one generic template reused site-wide: 3 feature boxes on events, 3×3 grid on gallery, static paragraphs + icon bullets on experience, 50/50 split on location). Each page owns its architecture — per-page ledger in `references/luxury-hospitality-protocol.md`

Detection is source-level; no render required for H1-H7, H13-H15 except H4/H11/H17 overlay verification. H8-H12 and H16-H18 are detected as absences — search for the required treatment and report when missing. H-IDs supersede their base-catalog equivalents in this domain (H1>L2/V6/I5/L4, H2>V12/M4/M6, H4>C1/C4, H5>A4/L9, H6>T8/V14, H10>V12, H8>L4, H9 inverse of T5/L5, H13>V12, H16>L2/V8/V9, H18>L2/L3/L4). H4 vs H17: report H4 for a raw default-styled map, H17 for every other embed, never both for one element. Report the H-ID, never both — double-firing inflates the score and sends the fixer at the wrong layer. Full heuristics, exemption rules, section ledger, and report rows: `references/luxury-hospitality-protocol.md`.

### Pass 3: Composition-Level Checks
- X1: Motion saturation (many elements enter, float, pulse, wiggle, or bounce)
- X2: Decorative priority inversion (icon containers, badges, ornaments > message weight)
- X3: Redundant UX writing (label, sublabel, helper, placeholder, hint repeat same fact)
- X4: Modal abuse (scrolling multi-column multi-section workflow in modal)
- X5: Page-level archetype uniformity (same section skeleton repeating down the page and across routes — identical splits, card rows, section heights). Generic scope; report H18 instead in hospitality, never both
- X6: No visual anchor (every element in a section at equal weight; nothing is memorable and no element reads as the point). Give one element dominance by content priority
- X7: Unbroken zigzag alternation (every media/text section flips side in strict alternation; the third consecutive zigzag makes the page read as a template). Cap consecutive zigzags at two
- X8: Split-header reflex (heading left, supporting paragraph right, repeated as the opening of section after section). Let the header take the shape its content implies
- X9: Unearned production chrome (version labels in the hero, scroll cues, decorative status dots, locale/time/weather strips, decoration text strips, floating top-right sub-text, photo-credit captions, version footers, micro-meta-sentences, generic step labels). Delete unless the item carries real, decision-relevant information — this chrome is what makes a page read as a demo rather than a product
- X10: Numbered-pagination chrome (`01 / 4` counters, scoring bars with filled tracks, step indicators on content that is not a real sequence). See L5 — numbering only where order itself is information

### Pass 4: Source Pass (E1-E58)

Read the source, not the render. Grep signatures, per-framework equivalents, and the enforcement checklist live in `references/code-slop-guide.md`; the full rows are in `references/slop-patterns.md`.

- **State and effects (E1-E6):** derived state in `useState`+`useEffect`, duplicated state, `useCallback`/`useMemo` cargo cult, `useEffect` as an event handler, continuous input in React state, undivided context
- **Data fetching (E7-E13):** unguarded fetch in an effect, no `AbortController`, happy-path-only, no cache or dedup, `'use client'` on everything, `async` passed to `useEffect`, sequential awaits where parallel is available
- **Lifecycle and leaks (E14-E18):** uncleaned timer, uncleaned listener or subscription, unrevoked object URL, module-level mutable state holding request or user data, unawaited promise with no error path
- **Component architecture (E19-E25):** god component, prop drilling 3+ layers, array index as key, missing key, boolean-prop configuration, inline handler per row in a hot list, no module boundaries
- **TypeScript (E26-E31):** `any`, suppression comment without a reason, absent prop types, `as` cast to silence a mismatch, `!` as a substitute for a check, permissive types
- **Markup and styling (E32-E37):** utility class soup, arbitrary values everywhere, specificity escalation, inline `style` for token values, duplicated utility clusters, no responsive strategy
- **Security and privacy (E38-E43):** `dangerouslySetInnerHTML` on unsanitized input, secret in the client bundle, user data in `console.log`, no boundary validation, sensitive data in URL/query/`localStorage`, unvalidated redirect or fetch target
- **Performance and framework (E44-E50):** client boundary at the root, raw `<img>` for large assets, unsized media, heavy import with no dynamic boundary, render blocked on non-critical data, fonts via `<link>` to a CDN, bundle growth with no accounting
- **Testing and structure (E51-E55):** no test on a correctness-critical path, snapshot-only tests, tests asserting implementation detail, dead code and unused exports, duplicated logic
- **Forms (E56-E58):** controlled/uncontrolled switch, file input value in state, form state not reset after submit

**Meta-tell.** Four or more of these together means the tree was generated without a human review pass: component file doubling as its own data layer, `any` at every boundary, fetch inside `useEffect`, `console.log` still present, unused exports, index keys, `'use client'` on the root, no test on a path that matters. Use it to set severity and justify a rewrite — report the individual E-IDs, not the gestalt.

</step>

<step name="track">
Add detected slop to the tracker database (project-local first, legacy fallback):
- Record: file path (relative to cwd), line number(s), slop type/category, timestamp, status (pending/reviewed/removed)
- Add evidence: source line citation, pattern ID (V1, T4, C2, I3, D2, etc.), confidence level (Confirmed/Probable/Candidate)
- Add severity: blocker/major/nit tag per finding
- Add auto_fix: boolean flag (true = mechanically fixable, e.g. swap emoji-icon for real icon component; false = requires human judgment)
- Maintain: versioned log with entries array, each entry has: id, file, line, pattern, confidence, severity, evidence, action_taken, resolved_at, auto_fix, project_score_history
- Auto-increment: entry IDs start at 1, increment by 1
- Path: ./.slop-tracker/slop-log.json — always the current working directory of the scanned project. Never write to a home or global path.
- Project score history: append run timestamp, entry count, trending slop score (computed across scan runs, not just flat incident list)
</step>

<step name="review">
Show tracked slop items with full detail:
- List all entries with: ID, file path, pattern ID, confidence, severity (blocker/major/nit), auto_fix (bool), status
- Group by: confidence level (Confirmed/Probable/Candidate), severity (blocker/major/nit), category (Visual/Typography/Color/Layout/Motion/Copy/Imagery/Quality/Accessibility/Design-Token/Hospitality-Luxury)
- Provide options per entry:
  - Mark as reviewed
  - Mark as removed
  - Add notes/remediation action
  - Export report
- Show exemptions: patterns that communicate real meaning, follow established brand, or serve functional interaction
- Context-aware exemptions: config flag/allowlist so intentional stylistic choices (e.g. deliberate brand gradient) don't re-trigger every scan
</step>

<step name="remediate">
Apply fixes to slop-infested content with priority ordering:

### Fix Order (apply in sequence to prevent cosmetic churn):
1. **Broken behavior, accessibility, overflow, readability, correctness, and false claims** (Q1-Q10, C4, C6, C7, L6-L9, L12, M2, M9, A1-A16, H15). Source-side: E7, E8, E12, E14-E18 (races, uncleaned lifecycle, silent failures), E38, E39, E41-E43 (security and privacy — blocker-class where input is user-controlled), E56-E58 (form ownership and reset)
2. **Information architecture and unsupported content** (X3, X6, P2, P4, I1, L10, L13, V19, V20, F7, H4, H7, H11, H14, H17). Source-side: E9, E11, E44, E46
3. **Repeated page templates, card anatomy, and container depth** (L2, L4, L11, V2, V6, X5, X7-X10, H1, H3, H5, H8, H12, H16, H18). Source-side: E19-E25 — the render-side repetition usually has a component-side cause
4. **Type hierarchy, spacing rhythm, and color roles** (T1-T18, C1-C5, C8-C11, L3, L5, H6, H9). Source-side: E1-E6, E26-E31 — fix after the structure is settled, since a rewrite invalidates both
5. **Decorative borders, gradients, glows, radii, icons, and motion** (V1-V8, V18, V21, M1-M4, M14-M18, H2, H10, H13)
6. **Copy cadence and redundant labels** (P1-P9)
7. **Imagery and placeholder issues** (I1-I6)
8. **Motion and layout violations** (M5-M8, M10-M13). Source-side: E32-E37 — the cosmetic layer of the source, last by design

Hospitality ordering rule: never fix H2 (pill pulse), H10 (badge weight), or H13 (price chip) before H1 or H18. Removing the pulse from a card grid that should not exist is wasted work — Stage 3 deletes the card, and the pill goes with it. Re-setting a price into a dot-leader while the menu page is still a filtered catalog (H14) is also churn: the catalog gets replaced, and the price treatment is rewritten with it.

Stage 1 blockers outrank every aesthetic finding on the page:
- **Q9** (contract-breaking refactor) — a renamed class the JS still queries is a silent runtime regression. Verify the contract before clearing any style refactor.
- **H15** (fabricated venue facts, hospitality only) — a claim the property cannot honour is a correctness defect, not a style one.
- **E38, E39, E41-E43** (source-side security and privacy) — unsanitized HTML on user input, a secret in the client bundle, an unvalidated trust boundary, sensitive data in the URL, an unvalidated redirect target. These are blocker-class where the input is user-controlled, and they never yield to a design finding.

After each system-level change, rescan the page. One primitive edit can clear many local symptoms.

### Replacement Principles:
- **Structure follows content**: comparable items suit tables/aligned lists; sequences suit steps; narrative content suits editorial flow; interactive choices suit controls with clear boundaries
- **Hierarchy uses a system**: establish explicit roles for display, heading, body, label, and data; single font family supports strong hierarchy through size, weight, width, optical size, spacing; second family earns place through clear role
- **Color carries a job**: assign colors to brand, action, status, emphasis, and surfaces; repeated decorative gradients/glows dilute those jobs
- **Spacing communicates relationship**: related items sit close; sections receive larger separation; repeated spacing tokens remain useful when semantic roles differ visibly
- **Motion communicates state, continuity, or causality**: default surfaces stay still; one meaningful transition carries more value than many ornamental effects
- **Decoration carries meaning**: domain artifacts, real product UI, data, photography, diagrams, and brand motifs provide specificity; generic icons and blobs disappear when carrying no information
- **Asymmetry follows priority**: give more space to more important content; avoid arbitrary card-size variation created solely to appear designed

</step>

<step name="report">
Generate report in standardized format:

```text
DIRECTION
Internal logistics dashboard: dense, calm, utilitarian; operational data carries priority.

FINDINGS
Location              | Rule    | Confidence | Severity | Auto-Fix | Evidence                    | Action
src/Form.tsx:31       | C4      | Confirmed  | major    | true     | gray text on blue surface   | use blue-tinted light text
src/Card.tsx:12       | V3      | Probable   | nit      | false    | border-l-4 on status card   | replace with status dot
src/page.tsx:44       | L2/T2   | Probable   | major    | false    | six identical feature cards | use ranked comparison list

NEEDS RENDERED VERIFICATION
src/Popover.tsx:18    | L8      | Candidate  | nit      | verify open state at mobile width

EXEMPTIONS
src/Steps.tsx:9       | L5      | Exempted   | nit      | markers label a real sequence

TRENDING SLOP SCORE (per-project across runs):
Run 2026-09-09: 12 findings, score 67 (majority nit)
Run 2026-08-26: 8 findings, score 42

VALIDATION
typecheck passed; mobile render checked; no horizontal overflow

Keep reports concise. Explain why a change belongs to this product. A list of removed classes provides less value than the resulting design logic.

</step>

</process>

<success_criteria>
- [ ] Scan directory for AI slop patterns (4-pass: quality failures, catalogued patterns, composition-level, source-level E1-E58)
- [ ] Track detected items in ./.slop-tracker/slop-log.json with full metadata (file, line, pattern, confidence, severity, evidence, auto_fix, project_score_history)
- [ ] Review tracked items with status updates, severity tagging, auto_fix flags, and exemption tracking
- [ ] Remediate following prioritized fix order (8+ stages, expanded from 6)
- [ ] Maintain persistent slop tracking across sessions via ./.slop-tracker/slop-log.json in the scanned project's working directory
- [ ] Generate standardized report format with direction/findings/needs verification/exemptions/validation/trending score
- [ ] Integrate 245 catalogued slop patterns across visual, typography, color, layout, motion, copy, imagery, quality, accessibility, design-token, state coverage, real-content stress, responsive, interaction feedback, dark-mode parity, composition, hospitality/luxury, and engineering
- [ ] Pre-emit gate: run the Stage 5 checklist in `references/design-spec.md` before clearing any delivery — contract, access, layout, motion, type, colour, states, content, source, domain. A failed check blocks, it does not annotate
- [ ] Motion gate: no `transition: all` (M9), no high-frequency animation (M10), entrances ease-out and short (M11), state never carried by motion alone (M12), curves chosen not defaulted (M13), no `scale(0)` entrance (M14), transitions over keyframes where retriggerable (M15), popovers origin-aware (M16), compositor-safe technique (M17), hover motion gated for touch (M18)
- [ ] Colour gate: no `#000`/`#fff` as large planes (C7), neutrals tinted to brand hue (C8), dark fills de-saturated (C9), every colour carries a job (C10), no premium-consumer palette reflex (C11)
- [ ] Layout gate: no orphan or filler grid cell (L11), hero CTA above the fold at 1440×900 with header under 80px (L12), one primary CTA per view (L13)
- [ ] Composition gate: page archetype varies by route (X5), each section has one anchor (X6), zigzag capped at two consecutive (X7), no split-header reflex (X8), no unearned production chrome (X9), no pagination chrome on non-sequences (X10)
- [ ] Source gate (E1-E58): every effect that subscribes, times, or fetches returns a cleanup; every fetch has loading/error/empty branches; no secret reachable from client code; no `dangerouslySetInnerHTML` on user input; every trust boundary validated server-side; no `any`/suppression/`!` at a boundary; stable list keys; client boundary at the leaf; browser surfaces themed (Q10); one correctness test on any money, auth, parse, or permission path
- [ ] Security blockers (E38, E39, E41-E43) reported blocker-class, Stage 1, never outranked by a design finding
- [ ] Evidence labelling: every material statement carries exactly one of Observed / Measured / Inferred / Unknown; inferred and unknown never promoted to factual claims
- [ ] Authorship discipline: report observable slop signals, never an inference that AI authored the artifact
- [ ] Completion rule: stop pruning when the next removal would reduce task completion, comprehension, evidence, identity, component or interaction behavior, action discoverability, migration safety, or accessibility
- [ ] Supersession honoured for the new generic IDs: X5>H18 in hospitality; T18 vs D4 (absence vs drift); M13 vs M5 (curve vs duration); M12 vs A7 (motion-only vs colour-only); C9 vs C2 (fill vs glow); C11 vs C5 (full scheme vs surface only); M14 vs M1 (start scale vs overshoot curve); A16 vs F5 (programmatic vs visual error mapping) — report one per defect
- [ ] Q9 gate: before clearing a refactor, diff the classes/IDs/data-attributes the JS, data fetching, and event listeners reference against the markup; report blocker on any break
- [ ] Domain-fit judgement: report a pattern only where it is a defect for *this* product's domain — a pattern correct elsewhere is not a finding here
- [ ] Support exemption tracking for patterns that communicate real meaning or follow established brand
- [ ] Context-aware exemptions: config flag/allowlist for intentional stylistic choices
- [ ] Cross-skill hook: D-category (drift) findings are technical debt -- option to also log to ponytail-debt format instead of duplicating tracker
- [ ] Auto-fix tagging: mark which findings are mechanically fixable vs. require human judgment
- [ ] Compute and maintain trending slop score per project across scan runs
- [ ] Storage: every artifact written to ./.slop-tracker/ under the scanned project's working directory; never a home, global, or tool-install path
- [ ] Luxury hospitality scope: apply H1-H18 when the target is luxury hospitality, fine dining, or a destination venue; skip H-patterns elsewhere
- [ ] Hospitality supersession: report the H-ID, never both H-ID and its superseded base pattern (H1>L2/V6/I5/L4, H2>V12/M4/M6, H4>C1/C4, H5>A4/L9, H6>T8/V14, H8>L4, H10>V12, H13>V12, H18>L2/L3/L4)
- [ ] H15 gate: every spatial and factual venue claim diffed against the supplied brief; unverifiable claims reported blocker with auto_fix false
- [ ] H4/H17 split: raw maps fire H4, all other embeds fire H17, never both for one element
- [ ] Generic↔hospitality mapping honoured: V19↔H17/H4, V20↔H11, F7↔H7, L10↔H18, V18↔H8, L9↔H5 — report at the scope of the target, never both
</success_criteria>

<pattern_catalog_summary>
Total patterns: 245

Visual: V1-V21 (21 patterns)
Typography: T1-T18 (18 patterns)
Color and Contrast: C1-C11 (11 patterns)
Layout and Space: L1-L13 (13 patterns)
Motion: M1-M18 (18 patterns)
Copy: P1-P9 (9 patterns)
Imagery: I1-I6 (6 patterns)
General Quality: Q1-Q10 (10 patterns)
Composition: X1-X10 (10 patterns)
Accessibility: A1-A16 (16 patterns)
Design-Token / Drift Detection: D1-D6 (6 patterns)
State Coverage: S1-S8 (8 patterns)
Real-Content Stress Test: R1-R6 (6 patterns)
Responsive/Breakpoint Consistency: B1-B5 (5 patterns)
Interaction Feedback Audit: F1-F7 (7 patterns)
Dark Mode / Theme Parity: DM1-DM5 (5 patterns)
Hospitality / Luxury: H1-H18 (18 patterns)
Engineering / Source-Code: E1-E58 (58 patterns)

Render-side total (V/T/C/L/M/P/I/Q/X/A/D/S/R/B/F/DM/H): 187
Source-side total (E): 58

</pattern_catalog_summary>

<references>
- references/slop-patterns.md — 245 AI slop phrase, design, and source-code patterns with risk levels, corrections, convergence pairs, and fix order
- references/design-spec.md — Construction-side counterpart: Stage 1 brief and design read, motion/colour/type token tables, build rules, polish passes, and the Stage 5 pre-emit gate that blocks delivery
- references/code-slop-guide.md — Source-side companion to the E category: grep signatures, per-framework equivalents, lint rules, and the pre-commit checklist
- references/content-quality-guidelines.md — Quality standards for content evaluation
- references/fix-order.md — Prioritized fix sequence to prevent cosmetic churn
- references/report-template.md — Standardized report format with direction/findings structure
- references/luxury-hospitality-protocol.md — H1-H18 luxury hospitality detection catalog: source heuristics, section-by-section archetype ledger, supersession table, exemption rules, fix-order placement
- references/hallmark-gates.md — Deterministic pre-emit design gates (hallmark) mapped to catalog IDs
- references/ponytail-debt-format.md — Debt doc format for cross-skill hook (optional logging)
</references>

<related_skills>
- content-pipeline — AI UGC video production (may generate slop)
- capture — Note capture workflow
- gsd:progress — GSD progress checking and routing
- ponytail-debt — Technical debt tracking (cross-skill hook format)
</related_skills>