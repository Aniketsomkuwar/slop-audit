# Fix Order Summary (8 Stages)

Apply fixes sequentially to prevent cosmetic churn. After each system-level change, rescan the page — one primitive edit can clear many local symptoms.

## Stage 1: Broken behavior, accessibility, overflow, readability, correctness, false claims, leaks
- Q1-Q10, C4, C6, C7, L6-L9, L12, M2, M9, A1-A16, H15
- Source-side: E7, E8, E12, E14-E18, E38, E39, E41-E43, E56-E58
- Fix: accessibility failures, overflow, above-fold hero reachability, readability issues, broken JS contracts, blanket `transition: all`, pure black/white surface floors, fabricated claims, then the source security tier — XSS, a secret in the client bundle, an unvalidated trust boundary, an uncleaned effect, a racing fetch
- **Q9** (contract-breaking refactor), **H15** (fabricated venue claim), and the E security tier (**E38/E39/E41-E43**) are blockers that outrank every aesthetic finding. Where the input is user-controlled, a leak is not a polish item

## Stage 2: Information architecture and unsupported content
- I1, X3, X6, P2, P4, L10, L13, V19, V20, F7, H4, H7, H11, H14, H17
- Source-side: E9, E10, E11, E13, E44, E46
- Fix: broken IA, sections with no visual anchor, competing CTAs, unsupported patterns, placeholder images, ungraded embeds, unscrimmed video, 50/50 reflex, hover-gated content, happy-path-only fetches, untaken client boundaries, unsized media

## Stage 3: Repeated page templates, card anatomy, container depth
- L2, L4, L11, V2, V6, X5, X7-X10, H1, H8, H12, H16, H18
- Source-side: E19-E25, E45, E47, E48
- Fix: redundant templates, page-level archetype uniformity, orphan grid cells, deep nested cards, extreme radius, card-grid aggregation, unboxing, god components, prop drilling, index keys, raw image tags, missing dynamic imports

## Stage 4: Type hierarchy, spacing rhythm, color roles
- T1-T18, C1-C5, C8-C11, L3, L5, H6, H9
- Source-side: E1-E6, E26-E31, E51, E55
- Fix: type hierarchy, modular scale, tabular numerals, uppercase tracking, spacing tokens, colour job assignment, tinted neutrals, dark-fill chroma, serif/sans pairing, editorial numerals, derived and duplicated state, `any`/`as`/`!` at the boundary, missing correctness tests

## Stage 5: Decorative borders, gradients, glows, radii, icons, motion
- V1-V8, V18, V21, M1-M4, M14-M18, H2, H10, H13
- Fix: decorative borders, gradients, glow effects, radii, icons, motion, status pills, dietary badges, price pills, `scale(0)` entries, origin-blind popovers, unbounded stagger, ungated hover motion

## Stage 6: Copy cadence and redundant labels
- P1-P9
- Fix: em-dash overuse, marketing buzzwords, aphoristic cadence, theater framing

## Stage 7: Imagery and placeholder issues
- I1-I6
- Fix: broken/placeholder assets, AI artifacts, inconsistent crops, media ratios

## Stage 8: Motion duration, frequency, and state signalling
- M5-M8, M10-M13
- Source-side: E32-E37, E40, E49, E50, E52-E54
- Fix: blanket scroll-fade, uniform duration, untuned easing curve, animated high-frequency actions, ease-in entrances, motion-only state indication, missing `prefers-reduced-motion`, staggered-reveal cliché, utility class soup, arbitrary values, specificity escalation, inline styles, `console.log` left in, render-blocking font links, unaccounted bundle growth, snapshot-only tests, dead code

## Final gate
Run the Stage 5 checklist in `references/design-spec.md`, plus the Source Gates in `references/hallmark-gates.md`. A failing check blocks delivery; it is not a note.

## Key Principle
After each system-level change, rescan. One primitive edit can clear many local symptoms.

**Never apply Stage 5 H-fixes before Stage 3.** Removing a decorative pill from a card grid that should not exist is wasted work — Stage 3 deletes the card and the pill goes with it.

**Never fix the source's cosmetic layer before its architecture.** E32-E37 land in Stage 8 for the same reason: extracting a class soup out of a component that Stage 3 is about to delete means doing it twice.
