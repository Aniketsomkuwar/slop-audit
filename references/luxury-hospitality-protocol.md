# Luxury Hospitality Protocol — Anti-Slop Patterns (H1-H18)

Detection and remediation reference for ultra-luxury hospitality, Michelin-grade
restaurants, and architectural destination venues.

**Scope gate:** H-patterns apply only when the target context is luxury hospitality,
fine dining, or a destination venue. A B2B dashboard, dev tool, or e-commerce
aggregator does not trigger them. Do not report H-findings outside that scope.

**ID blocks:** forbidden patterns are H1-H7 and H13-H15 (detected by presence); mandatory
codes are H8-H12 and H16-H18 (detected by absence of the required treatment). IDs are
stable — new patterns append, they never renumber. Existing slop-log.json entries keep
their meaning across versions.

## Why the generic catalog misses this

V/T/C/L/Q patterns catch generic AI slop — the failure mode they were written for is
"SaaS template applied to everything". Luxury hospitality has a different failure mode:
**aggregator grammar applied to a domain whose entire value is sensory specificity.**
Card grids, live-status pills, and pastel maps do not read as *generic* — they read as
*booking site*. That is a distinct, detectable defect the base catalog does not name.

The base catalog scores these as at worst mild instances of L2 or V12. In this domain
they are blocker-severity, because they make a destination venue indistinguishable
from a listing aggregator.

---

## Forbidden Patterns (H1-H7, H13-H15)

| ID | Pattern | Tell | Confidence | Correction |
|----|---------|------|------------|------------|
| **H1** | Card-grid aggregation | Rows of identical rounded cards: cropped image header + plain text body | Source | Full-bleed canvas, asymmetric split viewport, or editorial index/ledger. Structure follows the venue's actual logic, not a uniform tile |
| **H2** | Generic status pills | Pill with pulsing green/red dot: "Open Today • 12:00 PM – 11:30 PM", "Live", "Active Now" | Source | State as a typographic ledger line (`Open · 18:00–23:00`) or a meaning-bearing jewel dot. Never animate the indicator |
| **H3** | Fake chrome around media | Mock window headers, tab strips with close buttons, `Sky Hi Dining • Live Map` bars wrapped around video/maps | Source | Render media as a full-bleed architectural plane. No simulated OS or browser UI. Exempt only when the product genuinely is an OS or browser simulator |
| **H4** | Raw embedded maps | Unstyled Google Maps — pastel roads, yellow highways, cyan water — inside a dark palette | Source | Monochromatic CSS filter + dark overlay, or purpose-built vector/static cartography |
| **H5** | Circular icon lists | Icon-in-a-circle stacked next to label rows for address/contact/hours | Source | Tabular concierge ledger, architectural key-value rows, or editorial prose |
| **H6** | Fintech sans pairing | Geometric startup sans (Outfit, Poppins, Inter) set against a luxury serif | Source | Humanist, Roman, or sharp editorial: Plus Jakarta Sans, Tenor Sans, Syne, Manrope. **This is a *pairing* rule, not a ban on Inter** — Inter is legitimate for a clean/technical identity (report T8 there, not H6) |
| **H7** | Hover-gated core content | Dish photography, ingredients, tasting notes, or pricing hidden behind a hover reveal | Source | Static, confident presence. Hover may *add* information; it must never *gate* it. Generic case: F7 |
| **H13** | Commerce price and tag pills | A price (`₹200`), tag, or category trapped in a rounded pill, chip, or grey button box | Source | Lightweight text labels, subtle dots, or an integrated typographic scale. In the dining context: editorial ledger — Cormorant Garamond title, hairline dot-leader, standalone gold serif price |
| **H14** | Retail menu chrome | Search toolbar with magnifier, pill filter chips, "Expand All" toggle on a dining menu | Source | Editorial category index, chapter curtain, or video chapter transition |
| **H15** | Spatial/factual hallucination | "Rooftop venue", "high-rise", "skyline balcony" claimed for a ground-level garden estate; invented floors, views, amenities, awards, capacity | Source + Judgment | Verify every spatial/factual claim against the supplied brief. Where the brief is silent, **omit the claim** — never default to the aspirational one |

**H15 severity: blocker.** It is the only pattern in the catalog a guest can falsify by
arriving. Generic styling loses a booking; a fabricated rooftop loses trust permanently,
at the door. Report it as blocker with `auto_fix: false` — the fix requires venue facts the
scanner does not have.

**H1 has two tells — count both:**
- *Uniform container reflex* — 3+ sibling rounded containers (`border-radius: 12-16px` + a
  generic shadow + flat grey/dark fill) wrapping halls, gardens, gazebos, dishes, events
  alike. Reads as an admin tool or pricing matrix.
- *Box-in-a-box thumbnail* — `<img>` carrying its own rounded radius inside a parent that
  already has border + radius. Two frames where zero are needed. Report H1, not L4 —
  the fix layer is the card itself.

---

## Mandatory Design Codes (H8-H12, H16-H18)

These are violations of *absence*. The defect is that the required treatment is missing.

| ID | Code | Violation tell | Correction |
|----|------|----------------|------------|
| **H8** | Canvas over card | Generic boxed containers where full-bleed would serve; no hairline architectural borders | Full-bleed backgrounds, `1px solid rgba(223, 191, 108, 0.18)` hairline borders, asymmetric split viewports — not boxed cards |
| **H9** | Editorial typographic scale | Numbers, coordinates, and labels set as incidental UI text rather than design elements | Treat them as design: Roman or serif numerals ("01", "02"), wide tracking `0.2em`, small caps. `01 — Rogan Josh`, not `1. Rogan Josh` |
| **H10** | Discreet dietary indicators | Heavy "VEG" / "NON-VEG" badges, or the FSSAI bordered square with a green/red dot inside | 4-6px glowing jewel dot framed in a glass disc. Meaning stays; the shout goes. The FSSAI square is a packaging mark — on a website it signals cloud kitchen or delivery aggregator |
| **H11** | Cinematic media integration | Background video with hard edges, no scrim, or a scrim that fights the palette; fast-cut or high-contrast stock footage | Calibrated linear **and** vignette gradient scrims bleeding seamlessly into the dark palette. Footage atmospheric, slow-motion, low-contrast — tandoor embers, a simmering rogan |
| **H12** | Tabular concierge hierarchy | Arrival, contact, and venue data as scattered prose or icon rows | Five-star concierge dossier: category key on the left in letter-spaced uppercase sans, refined Cormorant Garamond serif values on the right |
| **H16** | Architectural signature footer | Generic 3-column template: tiny corner logo, three plain link lists, floating circular SVG social icons | Grand brand masthead, direct telephone booking line as primary action (real `tel:` link), tabular navigation, large low-opacity background watermark brand stamp |
| **H17** | Curated third-party grading | Any embed — map, review strip, booking widget, social feed — left in its own palette, or placed raw beside a contact card in a 50/50 box split | Grade into the house palette and frame asymmetrically (floating glass Concierge Dossier over full-canvas cartography). Equation, tune per palette: `filter: invert(92%) hue-rotate(185deg) saturate(35%) brightness(78%) contrast(125%);` |
| **H18** | Page-archetype uniformity | One generic template reused site-wide: 3 feature boxes on events, 3×3 grid on gallery, static paragraphs + three icon bullets on experience, every section a 50/50 split | Each page built from the venue's own spatial logic — see the section ledger below. Generic case: L10 |

### H18 Section Ledger — the correction, per page

| Page / section | Uniform-template defect | Venue-logic architecture |
|---|---|---|
| Menu cards | 4 identical rounded cards, cropped image headers, hover drawers | 1:1 square media frames with inset gold bezels, discreet jewel dots, static tasting notes, bold serif numerals `01`, `02` |
| Individual dish | Rounded thumbnail, delivery badge, title, price in a chip | Editorial dining ledger: framed 1:1 thumbnail, floating jewel dot, Cormorant Garamond title, hairline dot-leader, standalone gold serif price |
| Experience | Static paragraphs with 3 generic icon bullets (Hall/Garden/Gazebo) | Full-screen 3-column vertical curtain — `flex: 1`, expanding to `flex: 1.7` on hover — individual looping video per column, gold dividers, cinematic scrims |
| Location / arrival | 50/50 box split, fake browser bar, raw bright map, vertical icon rows | Full-canvas monochromatic dark cartography (CSS filters) with an asymmetrical floating glass Concierge Dossier (key-value ledger) |
| Footer | Generic 3-column template, floating SVG circles | Architectural signature — masthead, telephone booking line, tabular nav, background watermark stamp (H16) |
| Menu page | Dry catalog: search box, status pills, static table | Culinary theater: ambient header video loop (tandoor embers / rogan simmer), category-switching video curtain, cursor-tracking dish portals |
| Event pages | 3 generic feature boxes with checkmark lists | Spatial occasions lookbook: alternating full-width visual spreads (Gazebo, Garden, Hall Buyout), each with guest-capacity specifications |
| Gallery | Uniform 3×3 grid plus generic lightbox | Asymmetric exhibition filmstrip: alternating portrait (4:5) and landscape (16:9) frames, stills mixed with living video cinemagraphs |

---

## Relationship to the Existing Catalog

H-patterns deliberately **refine or supersede** base patterns in this domain. Report the
H-ID, not both — double-firing inflates the score and sends the fixer at the wrong layer.

| H-ID | Overlaps | Resolution |
|------|----------|------------|
| H1 | L2 (identical card grids), V6 (extreme radius), I5 (inconsistent crops), L4 (nested cards) | H1 supersedes L2 when the cards carry image headers, and absorbs box-in-a-box nesting. Fix H1; L2/V6/I5/L4 usually resolve with it |
| H2 | V12 (capsule badges), M4/M6 (reflexive motion) | H2 supersedes V12 when the badge reports venue status. The pulse is the H2 tell, not a separate M finding |
| H3 | — | New. No base pattern names simulated chrome |
| H4 | C1 (color palette clash), C4 (contrast on chromatic surface) | H4 is the specific case; C1/C4 fire only on the residual contrast failure after the map is filtered |
| H5 | A4/L9 (non-semantic markup) | H5 supersedes when the icon list encodes contact/venue data |
| H6 | T8 (overused font), T14/V14 (unjustified typeface) | H6 supersedes T8 for this domain — it names the *pairing* failure, which T8 does not |
| H7 | — | New. No base pattern covers hover-gated content |
| H8 | L4 (nested cards), V6 | H8 is the positive-side framing; report H8 when the fix is "unbox", L4 when it is "un-nest" |
| H9 | T5 (repeated kickers), L5 (numbered markers) | H9 is the inverse: L5/T5 flag decorative overuse, H9 flags editorial treatment that is absent |
| H10 | V12 (capsule badges) | H10 supersedes V12 when the badge is dietary — including the FSSAI square form. Same fix layer, so never report both |
| H11 | C2 (glowing accents), C3 (gradient text) | H11 is about media scrims; C2/C3 about UI accents. Distinct — do not merge |
| H12 | X3 (redundant UX writing) | H12 is structural; X3 is duplication. Both may fire on the same block |
| H13 | V12 (capsule badges) | H13 when the capsule contains a price. Price is not a badge — H13 supersedes V12 |
| H14 | X3 (redundant UX writing) | H14 is the control apparatus itself; X3 is duplication within copy. Distinct |
| H15 | — | New. No base pattern covers fabricated venue facts |
| H16 | L2 (identical columns), V8/V9 (generic social icons) | H16 supersedes when the icon circles or link columns sit in the footer |
| H17 | H4 (raw embedded maps) | H4 for a raw default-styled map; H17 for every other embed, or for any embed that is ungraded/box-split. Never both for one element |
| H18 | L2, L3, L4, L10 (local symptoms of one shared template) | H18 is page-level. Fix H18 first — the L-findings usually resolve with it |
| H13 | V12 (capsule badges) | H13 when the capsule contains a price or a tag. Neither is a badge |
| H7 | F7 (hover-locked essential data) | H7 is the hospitality case; F7 is generic. Report at the scope of the target, never both |
| H17 | V19 (ungraded third-party embed), H4 (raw maps) | H17 is the hospitality case; V19 is generic. H4 for a raw default-styled map, H17/V19 for every other embed |

**Generic ↔ hospitality mapping.** The same defect reports at the scope of the target.
Outside the domain, report the generic ID; inside it, the H-ID — never both.

| Generic | Hospitality | Relationship |
|---------|-------------|--------------|
| V19 Ungraded third-party embed | H17 (any embed), H4 (maps) | Same defect, house-palette severity |
| V20 Unscrimmed ambient video | H11 | H11 adds the slow/atmospheric footage rule |
| F7 Hover-locked essential data | H7 | H7 names dish photos, ingredients, tasting notes |
| V18 Border-as-hierarchy crutch | H8 (inverse) | H8 is the positive framing: canvas over card |
| L10 Default 50/50 split reflex | H18 | H18 is page-level; L10 is the section-level symptom |
| L9 Non-semantic markup | H5 | H5 when the icon list encodes contact/venue data |

---

## Fix-Order Placement

H-findings route into the existing 8-stage sequence:

| Stage | H-patterns | Why here |
|-------|-----------|----------|
| 1 — Broken behavior, accessibility, readability | H15 | A false claim about the property is a correctness defect, not a style one. It outranks every visual finding |
| 2 — Information architecture | H4, H7, H11, H14, H17 | Media integration, content gating, and menu control apparatus are IA-level, not cosmetic |
| 3 — Templates and container depth | H1, H3, H5, H8, H12, H16, H18 | Card anatomy, container structure, footer, page archetype |
| 4 — Type hierarchy and color roles | H6, H9 | Typographic system |
| 5 — Decoration | H2, H10, H13 | Indicator and price treatment |

Never fix H2 (pill pulse), H10 (badge weight), or H13 (price chip) before H1 or H18.
Removing the pulse from a card grid that should not exist is wasted work — Stage 3 deletes
the card, and the pill goes with it. Likewise, re-setting a price into a dot-leader while
the menu page is still a filtered catalog (H14) is churn: the catalog gets replaced, and
the price treatment is rewritten with it.

---

## Detection Heuristics

Source-level, no render required:

- **H1**: 3+ sibling elements sharing a rounded-container class, each containing an `<img>`
  and a short text block, with uniform padding/radius across the set. Also: `<img>` carrying
  a radius class whose ancestor also carries border + radius (box-in-a-box)
- **H2**: `animate-pulse`, `@keyframes` on a `rounded-full` dot, or status strings
  matching `(Open|Live|Active|Open Now)` inside a bordered pill
- **H3**: media element wrapped in a parent carrying `rounded-lg`/`shadow-xl` plus a
  child strip with three dots, or text matching `(LIVE|PREVIEW|localhost:)` near media.
  Check whether the product is genuinely an OS/browser simulator before reporting
- **H4**: `google.com/maps/embed`, `maps.googleapis.com`, or an `<iframe>` with
  `maps` in src, with no `filter:` applied on an ancestor
- **H5**: repeated `rounded-full` icon wrapper as a direct sibling of an address, phone,
  email, or hours string
- **H6**: `Outfit`, `Poppins`, `Inter`, `DM Sans`, `Montserrat`, `Raleway` declared alongside
  `Cormorant`, `Playfair`, `EB Garamond`, `Tenor Sans`, or any display serif
- **H7**: `group-hover:opacity-100`, `hover:reveal`, or `:hover` rules controlling
  `opacity`/`visibility` on elements containing `<img>`, price strings, or ingredient lists
- **H13**: price string (`₹`, `$`, `€` + digits) inside an element carrying `rounded-full`,
  `rounded-lg`, `badge`, `chip`, `pill`, or `btn`; or a price inside `<button>`
- **H14**: `<input type="search">`, `placeholder` matching `(search|find a dish)`, or a
  control matching `(Expand All|Show All|Filter)` on a page containing dish markup
- **H15**: spatial nouns (`rooftop`, `skyline`, `high-rise`, `terrace above`, `penthouse`,
  `panoramic view`, `Nth floor`) in copy. **Always a Candidate** — no source-level rule can
  confirm the building. Escalate to Confirmed only after diffing the claim against the
  supplied brief or venue listing

H8-H12 and H16-H18 are detected as absences — search for the required treatment and report
when absent:

- **H8**: count `rgba(*, 0.18-0.2)` / hairline borders and full-bleed sections; zero of either is a finding
- **H9**: no `font-variant: small-caps`, no tracking >= `0.15em` on labels, numerals set in the body font
- **H10**: dietary data rendered as a bordered badge or a bordered square with a dot, or absent where the venue serves dietary-specific menus
- **H11**: `<video>` or background media with no sibling/overlay element carrying a gradient; `playbackRate > 1` or hard-cut `currentTime` jumps
- **H12**: contact/arrival data not structured as a key-value ledger (check for a `<dl>`, or a two-column grid with a label column)
- **H16**: footer with no `tel:` link, no element wider than ~80% viewport in the display serif (masthead/watermark), or 3+ sibling link columns of equal width
- **H17**: an `<iframe>`/embed whose ancestor chain carries no `filter:` and no overlay gradient
- **H18**: count distinct section archetypes per page. Two or more pages sharing the same
  sibling-count and same grid-template signature is a finding

---

## Exemptions

Exempt — and record the reason — when:

- **H1** — the grid *is* the product (a genuine listing/taxonomy view the venue operates)
- **H2** — the status is real-time inventory the user is about to act on (table availability,
  booking window closing). Live data earns a live indicator
- **H4** — an interactive map is the primary interaction *and* has been restyled to the palette
  (the exemption is for the styling requirement, not the map itself)
- **H7** — the user explicitly instructed hover-gated content for a specific component
- **H9** — a house style guide mandates a different numeral treatment; cite the guide
- **H10** — regulatory or allergy-safety requirements mandate explicit dietary labelling.
  Safety labelling is never slop
- **H13** — the venue runs genuine e-commerce (retail jars, gift boxes) where the price is
  an add-to-cart control, not a menu entry
- **H14** — the menu exceeds the scale where composition alone can navigate it, and search
  is a documented user need rather than template reflex
- **H16** — the venue has no telephone booking channel
- **H17** — the embed's own branding is contractually required (a booking partner's widget)

**H15 has no exemption.** A fabricated claim about the property is not a style choice.

---

## Report Rows

```text
FINDINGS
src/sections/Menu.tsx:88    | H1      | Confirmed  | major   | 6 identical rounded dish cards | replace with editorial index; keep 3 signature dishes as full-bleed
src/sections/Menu.tsx:104   | H13     | Confirmed  | major   | price in rounded-full chip    | gold serif price + hairline dot-leader
src/sections/Menu.tsx:12    | H14     | Confirmed  | major   | search box + "Expand All" over dish list | category as editorial index; drop filter apparatus
src/components/Status.tsx:14| H2      | Confirmed  | major   | animate-pulse dot on "Open Today" | static ledger line: "Open · 18:00-23:00"
src/Venue.tsx:51            | H4      | Confirmed  | blocker | raw maps iframe, no filter  | monochrome filter + dark overlay, or static cartography
src/Venue.tsx:80            | H17     | Confirmed  | major   | review widget in 50/50 split | grade to palette + floating asymmetric panel
src/Contact.tsx:22          | H5      | Probable   | major   | 4 icon-in-circle contact rows | concierge ledger: key left, serif value right
src/hero/Video.tsx:9        | H11     | Confirmed  | major   | background video, hard edges | linear + vignette scrim into palette
src/Footer.tsx:3            | H16     | Confirmed  | major   | 3 link columns, no tel: link | masthead + booking line + watermark stamp
src/pages/Gallery.tsx:31    | H18     | Probable   | major   | 3x3 grid, same signature as Events | asymmetric filmstrip: 4:5 / 16:9 alternating

NEEDS HUMAN VERIFICATION
src/About.tsx:18            | H15     | Candidate  | blocker | copy claims "rooftop venue" | ground-level garden estate per brief — confirm then remove claim
```
