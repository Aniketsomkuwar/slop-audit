# Fix Order — Prioritized Fix Sequence to Prevent Cosmetic Churn

Apply fixes in this exact sequence. Each stage, if applied out of order, can trigger
re-work in later stages. After each system-level change, rescan the page.

## Stage 1 — Broken behavior, accessibility, overflow, readability

Fix all issues that affect functionality, screen reader access, or basic readability:

- **Q1-Q8** — General quality: cramped padding, viewport-edge text, justified text without hyphenation, low contrast text, skipped heading levels, tight line height, tiny body text (<12px), wide letter spacing on body
- **Q9** — Contract-breaking refactor: a class, ID, or `data-*` attribute renamed, removed, or restructured while JS, data fetching, or event listeners still reference it. **Blocker, auto_fix false** — diff the selectors the JS uses against the markup before clearing any style refactor. Silent runtime regression, not a visual one
- **C4** — Gray text on colored background: verify contrast, adjust foreground/background/weight/size
- **L6** — Line length too long: constrain prose to ~60–75ch; code/tables/deliberate data exempt
- **L7** — Content overflowing container: wrap, constrain, truncate with accessible affordance; provide deliberate scrolling
- **L8** — Positioned child clipped by overflow: move layer outside clipping context; use portal; change overflow where safe
- **M2** — Layout-property animation: prefer transform and opacity; use bounded disclosure for necessary height changes
- **M9** — Blanket `transition: all`: name the properties — `transition: transform 160ms, opacity 160ms`. A blanket transition animates unintended properties and usually hides an M2 offender; fixing M9 first surfaces it
- **L12** — Above-the-fold hero overflow: at 1440×900 the headline or primary CTA must be reachable without scrolling, and the header must hold one row under ~80px. Render check
- **C7** — Pure black/white surfaces: lift and tint the dark floor (`oklch(0.18 0.012 250)`, not `#000`) before verifying any contrast ratio — a pure floor makes every ratio fail against a different frame than the one it was tuned on
- **H15** — Spatial/factual hallucination: diff every venue claim (rooftop, high-rise, skyline, floor, view, capacity) against the supplied brief. Unverifiable claims are **blocker, auto_fix false**. Where the brief is silent, omit the claim rather than keeping the aspirational default. This sits in Stage 1 regardless of visual weight — a claim the property cannot honour is a correctness defect and outranks every aesthetic finding

**Source-side, Stage 1.** These read the code rather than the render, and the security tier is blocker-class wherever the input is user-controlled:

- **E38** — `dangerouslySetInnerHTML` on unsanitized input: render as text, or sanitize through a maintained library at the boundary. **Blocker where the input is user-controlled**
- **E39** — Secret in the client bundle (a key, token, or signing secret behind `NEXT_PUBLIC_*`, `VITE_*`, or in client code): move it server-side. Anything shipped to the client is public. **Blocker**
- **E41** — No validation at the trust boundary: validate server-side regardless of what the client checks. **Blocker**
- **E42** — Sensitive data in the URL, query string, or `localStorage`: keep credentials out of URLs; prefer httpOnly cookies
- **E43** — Unvalidated redirect or fetch target taken from a parameter: restrict to known origins or paths
- **E7 / E8** — Fetch in an effect with no guard, and no `AbortController`: an async request resolving after unmount or a dependency change, racing the next one. Fix with an `ignore` flag or an abort on cleanup
- **E12** — `async` passed directly to `useEffect`, so the effect returns a Promise and cleanup is not a function. Fix with an inner async function called from a synchronous outer one
- **E14 / E15 / E16 / E17** — Uncleaned timer, listener or subscription, object URL, or module-level mutable state holding request or user data. Every effect that subscribes, times, or fetches declares its cleanup
- **E18** — Unawaited promise with no error path, so the rejection is never surfaced
- **E56 / E57 / E58** — Controlled/uncontrolled switch, file input value held in state, and form state not reset after submit. Form ownership is a correctness question, not a polish one

**Stage 1 blockers outrank every aesthetic finding on the page: Q9, H15, and the E security tier (E38, E39, E41-E43).** Q9 ships a silent runtime regression; H15 ships a false claim the client cannot honour; the E tier ships a leak or an exploitable boundary. None is a style question, and none may be deferred behind a redesign.

## Stage 2 — Information architecture and unsupported content

Fix structural and content-level issues:

- **I1** — Broken or placeholder image: use real asset, generated asset with authorization, fallback, or remove element
- **X3** — Redundant UX writing: remove duplicate labels/sublabels/helpers/placeholders/hints that repeat the same fact
- **P2** — Marketing buzzword: replace generic claims (supercharge, streamline, empower, world-class, enterprise-grade, seamlessly, unlock) with exact action, object, user, and result
- **P4** — Theater framing copy: state the behavior, limitation, or consequence directly instead of dismissing a category as "theater"
- **V19** — Ungraded third-party embed: grade the embed into the surrounding palette with a CSS filter stack, custom color overlay, or the vendor's own theme. Report H4/H17 instead when the target is a hospitality page
- **V20** — Unscrimmed ambient video: add calibrated linear **and** radial gradient scrims; verify text contrast against every frame, not just the first. Report H11 instead in hospitality
- **L10** — Default 50/50 split reflex: break the unvarying half-and-half with a full-width banner, asymmetric 40/60, tabular rows, or an interactive multi-panel viewport. Report H18 when the split repeats page-wide
- **F7** — Hover-locked essential data: move core information (price, spec, primary action) into static presence; hover may add, never gate. It is unreachable on touch. Report H7 instead in hospitality
- **L13** — CTA competition: one primary action per view. Equal-weight CTAs, or two expressing the same intent, mean no action is actually primary
- **X6** — No visual anchor: give one element per section dominance, chosen by content priority rather than size alone. A section where everything is equally weighted has no point
- **H4** — Raw embedded map: apply the curated grading filter stack plus a dark overlay, or redraw as purpose-built cartography. Never leave a default-styled map in a dark palette
- **H7** — Hover-gated core content: dish photos, ingredients, tasting notes, and pricing stay static. Hover may *add*; it must never *gate*
- **H11** — Cinematic media integration: background video carries calibrated linear + vignette scrims bleeding into the palette; footage stays slow-motion, atmospheric, low-contrast
- **H14** — Retail menu chrome: remove search toolbars, pill filter chips, and "Expand All" toggles from a dining menu; navigate by editorial index, chapter curtain, or video chapter
- **H17** — Curated third-party grading: grade every embed into the house palette and frame it asymmetrically. Never leave a raw embed in a 50/50 box split beside a contact card

**Source-side, Stage 2.** The states and boundaries that belong to the architecture, not the polish:

- **E9** — Happy-path-only fetch: loading, error, and empty branches are part of the component (S1, S2, S4), not a follow-up
- **E11 / E44** — `'use client'` on everything, and the client boundary at the route root: the directive and the resulting payload are the same mistake at two levels. Push the boundary to the interactive leaf
- **E46** — Unsized media causing layout shift: reserve the space. The largest single contributor to a failing CLS
- **E10 / E13** — No cache or dedup, and sequential awaits where parallel is available (`Promise.all` for independent work)

## Stage 3 — Repeated page templates, card anatomy, and container depth

Fix repeated structural patterns across the page:

- **L2** — Identical card grids: choose structure from content (ranked layout, table, list, accordion, or genuine uniform controls). Avoid 4+ siblings with identical icon/heading/paragraph sizing
- **L4** — Nested cards: remove redundant surfaces; group with spacing, headings, dividers, or one shared container. Avoid 3+ nested levels repeating borders/fills/radii/shadows
- **V2** — Glassmorphism everywhere: use opaque surfaces; express hierarchy through contrast, spacing, elevation instead of repeated translucent cards + backdrop-filter: blur
- **V6** — Extreme border radius: use role-based radius scale; common cards: 8–16px. Avoid >=24px that turns small cards/sections/inputs into blobs
- **L11** — Orphan or decorative grid cell: cell count must equal item count. Redistribute the item span or change structure; never insert a filler cell to square the layout
- **X5** — Page-level archetype uniformity: fix this first when two or more routes share a section skeleton. Each route owns the structure its content implies, and the local L2/L3/L4/L10 findings on those routes usually resolve with it. Report H18 instead in hospitality
- **H18** — Page-archetype uniformity: fix this first when two or more pages share a grid signature. Each page gets its own venue-logic architecture per the section ledger — the local L2/L3/L4 findings on those pages usually resolve with it
- **H1** — Card-grid aggregation: replace uniform rounded card sets with full-bleed canvas, asymmetric split viewport, or editorial index. Includes box-in-a-box thumbnails — one frame, not two
- **H8** — Canvas over card: `1px solid rgba(223, 191, 108, 0.18)` hairlines and full-bleed planes where boxed containers were used
- **H16** — Architectural signature footer: masthead, direct telephone booking line (`tel:`), tabular nav, low-opacity watermark stamp — replacing the generic 3-column template

**Source-side, Stage 3.** Render-side repetition usually has a component-side cause, so fix the component and the local L2/L4/L11 findings resolve with it:

- **E19** — God component owning layout, data, state, handlers, and rendering. Split by responsibility
- **E20 / E25** — Prop drilling through three or more layers, and no module boundaries: compose (`children`), use context, or colocate the state; separate data access, business logic, and presentation
- **E21 / E22** — Array index as key on a reorderable list, and a missing key entirely. Key on a stable identity
- **E23 / E24** — Boolean-prop configuration accumulating into a flag matrix, and inline handlers recreated per row in a hot list. Use variants or composition; hoist the handler
- **E45 / E47 / E48** — Raw `<img>` for large assets, a heavy import with no dynamic boundary, and render blocked on non-critical data. Use the framework image component, import dynamically at the point of use, and stream or suspend the slow section

## Stage 4 — Type hierarchy, spacing rhythm, and color roles

Fix typographic and color system issues:

- **T1** — Flat type hierarchy: define fewer roles with clearer combined contrast; use ratio (1.25x) as screening heuristic between adjacent roles
- **T2** — Icon tile stacked above heading: align icon and heading; place icon in flow; remove unnecessary container
- **T7** — Crushed letter spacing: restore character shapes; use modest optical tightening around 0 to −0.02em. Avoid display tracking < −0.05em or body with negative tracking
- **C1** — AI color palette: report only as cluster signal; derive action/status/surface/accent from existing product context. Avoid purple/violet gradients or cyan-on-dark without brand evidence
- **C5** — Cream/beige reflex: establish deliberate surface, ink, accent, and status colors. Avoid warm off-white as whole page's generic surface without palette
- **C8** — Zero-chroma neutrals: give the neutral ramp a small chroma at the brand hue — enough to belong, well under half the brand's chroma
- **C9** — Full-chroma accent fill on dark: lower chroma and lift lightness for dark-mode fills; keep full chroma for small indicators only
- **C10** — Palette with no role assignment: give every colour a job (brand, action, status, emphasis, surface) and name the token after the job rather than the hue
- **T16** — Proportional numerals in tabular context: `font-variant-numeric: tabular-nums` on every figure read in a column or refreshed in place
- **T17** — Uppercase label without compensating tracking: 0.05em–0.12em on uppercase micro-labels
- **T18** — No modular type scale: put the roles on one ratio, commonly 1.2–1.333. Define the scale before chasing D4 drift, or the drift has no reference to be measured against
- **H9** — Editorial typographic scale: numerals, coordinates, and labels set as design — Roman/serif numerals ("01"), tracking `0.2em`, small caps

**Source-side, Stage 4.** State correctness and type quality, fixed after the structure is settled — a Stage 3 rewrite invalidates both:

- **E1 / E2 / E3** — Derived state kept in sync by an effect, duplicated state holding the same fact twice, and `useCallback`/`useMemo` cargo cult around trivial values. Compute during render; give each fact one owner; memoize against a measured re-render
- **E4 / E5 / E6** — `useEffect` as an event handler, a continuous input value in React state, and undivided context re-rendering every consumer. Do the work in the handler; hold frame-rate values in a ref; split context by change frequency
- **E26 / E27 / E28 / E29 / E30 / E31** — `any` at a boundary, a suppression comment with no reason, absent or empty prop types, an `as` cast silencing a mismatch, `!` standing in for a check, and permissive types where a union belongs. All the same finding: the compiler was told to stop checking where checking mattered most
- **E51 / E55** — No test on a correctness-critical path, and the same helper or validation copy-pasted with drift. One implementation, one owner, one test that fails if the logic breaks

## Stage 5 — Decorative borders, gradients, glows, radii, icons, and motion

Fix decorative and ornamental issues:

- **V1** — Border accent on rounded element: choose one: neutral 1px border, lower radius, or no border. Avoid >=2px border + >=12px radius on same generic card
- **V3** — Side-tab accent border: use status dot, icon, label, or restrained tint when meaning exists; remove empty decoration. Avoid colored border >=3px on card side
- **V4** — Hairline border with wide shadow: keep crisp edge OR soft elevation (not both). Avoid 1px border + shadow blur >=20px on one element
- **V18** — Border-as-hierarchy crutch: replace borders drawn around text blocks with typographic contrast — size, weight, letter-spacing, and muted opacity (`0.6`–`0.8`). Hierarchy is typographic, not structural
- **V5** — Repeating-gradient stripes on surfaces: use solid surface or existing brand texture. Avoid repeating-linear-gradient or repeating-conic-gradient on surfaces
- **M13** — Untuned default easing curve: choose a curve per interaction class and tokenise it. Setting only the duration (M5) still reads as untuned
- **M1** — Bounce or elastic easing: use restrained ease-out curve and shorter travel. Avoid overshoot curves/repeated spring effects on dialogs/cards/routine controls
- **M3** — Image hover transform: keep imagery still or use restrained overlay, caption, or border response. Avoid generic cards repeatedly scale/rotate on hover
- **H2** — Generic status pills: replace the pulsing dot with a static typographic ledger line (`Open · 18:00–23:00`). Never animate a venue-status indicator
- **H10** — Discreet dietary indicators: replace VEG/NON-VEG badges and FSSAI squares with a 4-6px glowing jewel dot in a glass disc. Safety-mandated labelling is exempt
- **H13** — Commerce price pills: pull the price out of the pill/chip/button box. Cormorant Garamond title, hairline dot-leader, standalone gold serif price

**Never apply Stage 5 H-fixes before Stage 3.** Removing the pulse from a card grid that should not exist is wasted work — Stage 3 deletes the card and the pill goes with it. Likewise, re-setting a price into a dot-leader while the menu page is still a filtered catalog (H14) is churn: the catalog gets replaced and the price treatment is rewritten with it.

## Stage 6 — Copy cadence and redundant labels

Final copy-level fixes:

- **P1** — Em-dash overuse: rewrite body block with 3+ em-dash characters or repeated HTML entities using sentence boundaries, commas, colons, or parentheses
- **P3** — Aphoristic cadence: replace repeated patterns (Not X. Y. or Less X. More Y.) with direct product facts and natural sentence rhythm

## Stage 7 — Imagery and placeholder issues

- **I1** — Broken or placeholder image: real asset, authorized generated asset, or remove the element
- **I6** — Leftover placeholder images: no lorem-picsum or unsplash-random URLs in a production build
- **I5** — Inconsistent aspect ratios: standardize by role (1:1 catalog, 16:9 showcase, 4:5 portrait) with `object-fit: cover`
- **I3** — AI-image artifacts: garbled embedded text, malformed hands, inconsistent shadows. Regenerate or use real photography

## Stage 8 — Motion duration, frequency, and state signalling

The motion defects that are not decorative and do not belong in Stage 1:

- **M10** — Motion on a high-frequency action: keyboard shortcuts, search, filters, tab switches, row hovers, and menu opens resolve in roughly 100–150ms, or immediately. Deliberate motion belongs to occasional and rare interactions
- **M11** — Entrance on ease-in or over 300ms: entrances decelerate. Use ease-out or a custom curve and keep the duration short; ease-in belongs on exits
- **M12** — Motion-only state indication: pair the motion with a static affordance that survives `prefers-reduced-motion`. If the state exists only while the animation runs, users who miss the frame or reduce motion never see it
- **M7** — No `prefers-reduced-motion`: honour the query and provide a still equivalent. Motion must never be the only carrier of a state change
- **M5** — Uniform duration regardless of element size or travel distance
- **M6 / M8** — Reflexive hover-scale on non-interactive cards; staggered-list reveal applied to non-list content
- **M14** — `scale(0)` entry: start at `scale(0.9–0.97)` with `opacity: 0`. `scale(0)` reads as the element arriving from nowhere, and it is the tell of an unauthored entrance
- **M15** — Keyframes on a retriggerable surface: use a transition instead, so an interruption retargets rather than restarting from zero
- **M16** — Origin-blind popover: set `transform-origin` from the trigger. Modals are exempt — they scale from center
- **M17** — Unbounded stagger: 30–80ms between items, and never block interaction while it plays
- **M18** — Hover motion ungated by pointer type: wrap it in `@media (hover: hover) and (pointer: fine)`, or a touch device keeps the hover state after the tap

**Source-side, Stage 8.** The cosmetic layer of the source, last by design — fixing markup before the architecture means the extraction gets rewritten anyway:

- **E32 / E36** — Utility class soup (twenty or more utilities on one element) and duplicated utility clusters: extract the component or the class on the second use
- **E33** — Arbitrary values everywhere (`w-[437px]`, `mt-[13px]`): snap to the scale. This is D3/D4 drift in its Tailwind form
- **E34 / E35** — Specificity escalation (`!important`, ever-deeper selectors) and inline `style` for values that belong in tokens. Fix the source of the conflict; inline style is D2 hardcoding wearing a different hat
- **E37** — No responsive strategy: define the collapse per section (B1-B5)
- **E40 / E49 / E50 / E52 / E53 / E54** — `console.log` left in, Google Fonts via `<link>`, bundle growth with no accounting, snapshot-only tests, tests asserting implementation detail, and dead code. Remove before delivery; version control is the archive

Full token table: `references/design-spec.md` (Stage 2, motion tokens).

---

**After each stage:** Rescan the page. One primitive edit can clear many local symptoms.

**Never apply Stage N+1 before completing Stage N.** The stages are ordered to prevent cosmetic churn — fixing a decorative border (Stage 5) before fixing overflow (Stage 1) will just produce a new overflow problem.

**Final gate:** after Stage 8, run the Stage 5 checklist in `references/design-spec.md`. A failing check blocks delivery; it is not a note.