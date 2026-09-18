# slop-audit v3.5.0

Detect and flag AI-generated low-quality or misleading content (slop) with comprehensive pattern catalog and tracking.

## Quick Start

```bash
# Load the skill
/skill slop-audit

# Scan current directory for slop
/skill slop-audit --scan

# Scan specific path
/skill slop-audit --scan /path/to/project

# List tracked slop items
/skill slop-audit --list

# Clear resolved entries
/skill slop-audit --clear
```

## 4-Pass Scan System

**Pass 1: Quality Failures** — accessibility, readability, robustness, performance issues
- WCAG AA contrast, viewport-edge text, justified text without hyphenation, skipped heading levels, tight line height, tiny body text (<12px), wide letter spacing (>0.05em), non-functional interactive elements, happy-path-only design

**Pass 2: Catalogued Patterns** — 245 patterns across 18 categories
- Visual: V1-V21 (borders, glassmorphism, icons, radius, gradients, illustrations, ungraded embeds, unscrimmed video, geometric mask faking a cutout)
- Typography: T1-T18 (hierarchy, fonts, tracking, all-caps, pairing, tabular numerals, untracked uppercase, no modular scale)
- Color: C1-C11 (palettes, dark mode, gradient text, contrast issues, pure black/white planes, zero-chroma neutrals, full-chroma dark fills, unassigned colour roles, the terracotta reflex)
- Layout: L1-L13 (identical grids, spacing, overflow, clipped children, 50/50 reflex, orphan grid cells, hero below the fold, CTA competition)
- Motion: M1-M18 (bounce easing, layout animations, hover transforms, `transition: all`, animated high-frequency actions, ease-in entrances, motion-only state, untuned curve, `scale(0)` entry, keyframes where a transition belongs, origin-blind popovers, unbounded stagger, ungated hover motion)
- Copy: P1-P9 (em-dash overuse, buzzwords, cadence, theater framing)
- Imagery: I1-I6 (broken/placeholder images, AI artifacts, inconsistent crops, media ratios)
- Quality: Q1-Q10 (padding, contrast, line height, tiny text, letter spacing, contract-breaking refactors, unthemed browser surfaces)
- Composition: X1-X10 (motion saturation, decorative priority inversion, hierarchy crutches, page-archetype uniformity, no visual anchor, unbroken zigzag, split-header reflex, unearned production chrome, pagination chrome)
- Accessibility: A1-A16 (alt text, focus, keyboard nav, labels, ARIA, clickable divs, positive tabindex, missing focus traps)
- Design-Token: D1-D6 (radius/hex/spacing drift, near-duplicate values)
- State: S1-S8 (missing loading/empty/error/disabled coverage)
- Real-Content: R1-R6 (stress-testing with realistic content lengths)
- Responsive: B1-B5 (breakpoint gaps, touch targets, horizontal overflow)
- Interaction: F1-F7 (feedback gaps, hover-locked essential data)
- Dark-Mode: DM1-DM5 (dark-mode parity failures)
- Hospitality/Luxury: H1-H18 (activates only for luxury hospitality, fine dining, or destination venues — see below)
- Engineering/Source-Code: E1-E58 (effects without cleanup, fetch in `useEffect` with no abort and no error branch, `'use client'` on the root, god components, index keys, `any`/`@ts-ignore`/`!` at the boundary, secrets in client code, `dangerouslySetInnerHTML`, unvalidated trust boundaries)

**Pass 3: Composition-Level Checks** — higher-level structural issues
- Motion saturation, decorative priority inversion, redundant UX writing, modal abuse, page-archetype uniformity, missing visual anchor

**Pass 4: Source Pass (E1-E58)** — reads the code, not the render
- Effects declare their cleanup (E14-E17); every fetch ships loading, error, and empty branches (E9); one owner per fact (E1, E2, E25); types at the boundaries (E26, E29, E30); no secret reachable from the client (E39); no `dangerouslySetInnerHTML` on user input (E38); server-side validation (E41); stable list keys (E21, E22); client boundary at the interactive leaf (E11, E44); one correctness test on money, auth, parse, or permission paths (E51)
- Method, grep signatures, per-framework equivalents, and the enforcement checklist: `references/code-slop-guide.md`

## Two Surfaces

Passes 1-3 read the **rendered artifact** — every one of those findings can be judged from a screenshot. Pass 4 reads the **source** that produced it, and none of it is visible in the render.

The failure modes are independent: a page can pass every render-side check and still leak a secret, race a fetch, and carry `any` through its entire boundary — it looks finished. A page with immaculate source can still be visually generic — well-built slop. **A deliverable is not done because the screenshot is good.**

## Luxury Hospitality Domain (H1-H18)

Activates only when the target is luxury hospitality, fine dining, or a destination venue.

**Forbidden (detected by presence):**
- H1 card-grid aggregation (incl. box-in-a-box thumbnails) · H2 pulsing status pills · H3 fake window chrome around media · H4 raw pastel maps · H5 circular icon lists · H6 fintech sans against a luxury serif · H7 hover-gated dish photos/ingredients/prices · H13 prices trapped in pills or chips · H14 search toolbars and "Expand All" on a dining menu · H15 fabricated venue claims (rooftop/high-rise/skyline for a ground-level estate)

**Mandatory codes (detected by absence):**
- H8 canvas over card · H9 editorial numerals · H10 jewel-dot dietary marks (not FSSAI squares) · H11 cinematic scrims and slow atmospheric footage · H12 tabular concierge dossier · H16 architectural signature footer with `tel:` booking line · H17 curated third-party grading · H18 per-page archetypes instead of one shared template

**H15 is blocker severity with no exemption** — it is the only pattern a guest can falsify
by arriving. Every spatial and factual claim is diffed against the supplied brief; where
the brief is silent, the claim is omitted rather than defaulted to the aspirational one.

Full detection heuristics, section-by-section archetype ledger, supersession table, and
exemption rules: `references/luxury-hospitality-protocol.md`.

## Core Objective — All Domains

Avoid low-effort "AI-generated template" tropes, boilerplate Bootstrap/SaaS defaults, and
unstyled third-party widgets. Every layout must feel deliberate, bespoke, and tailored to
the actual product domain. A pattern that would be correct for one product is a defect in
another — judge fit, not tidiness.

Six generic patterns carry this objective, each with a hospitality counterpart. Report at
the scope of the target, never both:

| Generic | Defect | Hospitality counterpart |
|---------|--------|-------------------------|
| V18 Border-as-hierarchy crutch | borders drawn around every text block instead of using size, weight, letter-spacing, and muted opacity (`0.6`–`0.8`) | H8 (inverse) |
| V19 Ungraded third-party embed | map, calendar, or calculator widget left in its vendor palette | H17, H4 |
| V20 Unscrimmed ambient video | background video with no calibrated scrim, or text legible on frame 1 only | H11 |
| L10 Default 50/50 split reflex | every section text-left / media-right in an unvarying half-and-half box | H18 |
| F7 Hover-locked essential data | price, spec, or primary action reachable only on hover — unreachable on touch | H7 |
| Q9 Contract-breaking refactor | classes, IDs, or `data-*` attributes renamed or removed while JS, data fetching, or event listeners still reference them | **none — Stage 1 blocker in both domains** |

**Preserve technical hooks.** Never rename, remove, or modify existing HTML classes, IDs, or
data-attributes when refactoring styles if JavaScript engines, data fetching, or event
listeners depend on them. That is Q9.

**Preserve domain truth.** Never invent physical, architectural, or logistical features the
product does not have — generic as Q9's sibling rule, H15 in hospitality.

## Tracking

Detected slop logged to `./.slop-tracker/slop-log.json` — created in the scanned project's working directory, never in a home or global path — with:
- File path, line number, pattern ID, confidence (Confirmed/Probable/Candidate/Exempted)
- Severity (blocker/major/nit), evidence citation, status (pending/reviewed/removed)
- Exemption tracking for patterns communicating real meaning or following established brand

## Fix Order (8 stages — apply sequentially)

1. Broken behavior, accessibility, overflow, readability, correctness, and false claims (Q1-**Q9**, H15)
2. Information architecture and unsupported content (incl. **V19, V20, L10, F7**, H4, H7, H11)
3. Repeated page templates, card anatomy, container depth (incl. H1, H8, H16, H18)
4. Type hierarchy, spacing rhythm, color roles
5. Decorative borders, gradients, glows, radii, icons, motion (incl. **V18**, H2, H10, H13)
6. Copy cadence and redundant labels
7. Imagery and placeholder issues
8. Motion and layout violations

*After each stage: rescan. One primitive edit can clear many local symptoms.*

**Q9 and H15 are Stage 1 blockers that outrank every aesthetic finding** — Q9 ships a silent
runtime regression, H15 ships a claim the client cannot honour. Neither is a style question.
Never apply Stage 5 H-fixes before Stage 3: removing a decorative pill from a card grid that
should not exist is wasted work.

## Report Format

```text
DIRECTION
Internal logistics dashboard: dense, calm, utilitarian; operational data carries priority.

FINDINGS
Location              | Rule    | Confidence | Evidence                    | Action
src/Form.tsx:31       | C4      | Confirmed  | gray text on blue surface   | use blue-tinted light text

NEEDS RENDERED VERIFICATION
src/Popover.tsx:18    | L8      | Candidate  | clipped ancestor found      | verify open state at mobile width

EXEMPTIONS
src/Steps.tsx:9       | L5      | Exempted   | markers label a real sequence

VALIDATION
typecheck passed; mobile render checked; no horizontal overflow
```

## Reference Files (in `references/` directory)

- `slop-patterns.md` — full 245-pattern catalog across 18 categories, with risk levels and corrections, including the E1-E58 source block and the H1-H18 hospitality block
- `code-slop-guide.md` — E-category detection method: grep signatures, per-framework equivalents, lint rules, pre-commit checklist, and the scope/honesty limits. Byte-identical in both skills
- `design-spec.md` — construction-side counterpart: Stage 1 brief (design read + three dials), motion/colour/type token tables, build rules, polish passes, Stage 5 pre-emit gate, evidence labels, and the completion rule. Byte-identical in both skills
- `content-quality-guidelines.md` — Quality standards for content evaluation
- `fix-order.md` — Prioritized fix sequence to prevent cosmetic churn
- `fix-order-summary.md` — Condensed 8-stage sequence with the per-stage pattern IDs, including the source-side Stage 1 security tier
- `report-template.md` — Standardized report format with direction/findings structure
- `hallmark-gates.md` — Hallmark design gates integration (57 slop-test gates, 20 themes)
- `luxury-hospitality-protocol.md` — H1-H18 detection catalog: heuristics, section ledger, supersession, exemptions, fix-order placement
- `regex-debugging.md` — regex debugging notes for the operational scan script (group capture, heading-level and pattern-extraction pitfalls)

## AI Agent Integration

Invoke directly: `/skill slop-audit --scan PATH`

Output is JSON-parsable with pattern IDs, confidence levels, severity, and evidence. Evidence hierarchy: Source/Render/Judgment/Confidence with Confirmed/Probable/Candidate/Exempted levels and blocker/major/nit severity.

## Why This Skill Stands Out

1. 245 documented patterns across 18 categories, covering both the rendered artifact and the source that produced it
2. 4-pass scan system: quality failures → catalogued patterns → composition-level checks → source pass (E)
3. Every detection includes source line citation, confidence level, evidence class, and severity
4. Cross-session slop history in `./.slop-tracker/slop-log.json` with exemption tracking
5. 8-stage fix sequence prevents cosmetic churn when making changes
6. Replacement principles for meaningful substitutions, not just removals
7. Standardized direction/findings/needs verification/exemptions/validation report format
8. Designed for direct invocation by other AI systems with predictable output
9. Domain-scoped H catalog with a generic ↔ hospitality mapping, so one defect reports once
10. Stage 1 blockers — Q9 (broken JS contract), H15 (fabricated venue claim), and the E security tier (E38/E39/E41-E43) — outrank every aesthetic finding
11. Evidence classes (Observed/Measured/Inferred/Unknown) so no inference is promoted into a factual claim, and no scope goes unstated

## Installation

Copy the skill folder into your skills directory, then create the per-project tracking directories
inside the project you intend to scan:

```bash
mkdir -p ./.slop-tracker
mkdir -p ./.slop-tracker/references
mkdir -p ./.slop-tracker/notes
```

All generated data stays in that project's working directory.

## Credits

Created by Aniket Somkuwar. Integrates slop patterns from miqdadbadjuber/anti-slop, discountry/ritmex-skills anti-ui-slop, and the motion principles popularised by Emil Kowalski (transform/opacity-only, named transitions, duration scaled to frequency). Full catalog: visual (V1-V21), typography (T1-T18), color (C1-C11), layout (L1-L13), motion (M1-M18), copy (P1-P9), imagery (I1-I6), quality (Q1-Q10), composition (X1-X10), accessibility (A1-A16), design-token (D1-D6), state (S1-S8), real-content (R1-R6), responsive (B1-B5), interaction (F1-F7), dark-mode (DM1-DM5), hospitality (H1-H18), engineering/source-code (E1-E58).

v3.5.0 extends the source-code catalog with material from Vanszs/Anti-AI-UI (MIT), design-taste-frontend, emilkowalski/skills, impeccable (Apache 2.0), and leeskills (MIT) — the E category, A9-A16, and the evidence-class and completion-rule discipline.