# Hallmark Design Gates — Reference for Anti-AI-Slop Integration

Hallmark (nutlope/hallmark, v1.1.0) provides a deterministic design gate system that complements the probabilistic pattern-matching in slop-audit. While slop-audit detects and tracks slop with confidence levels, hallmark provides pre-emit design gates that refuse to hand back code matching generic LLM-trained defaults.

## Hallmark's 57 Slop-Test Gates (High-Level Categories)

### Philosophy Gates
- Rejects on-distribution LLM defaults
- Enforces "tactile rebellion" against generic UI patterns
- Refuses color-swapped clone templates

### Hierarchy Gates
- Establishes explicit display/heading/body/label roles
- Single font family supports hierarchy through size/weight/width/optical size/spacing
- Second family earns place through clear role definition

### Execution Gates
- 61-rule deterministic detector (from impeccable skill)
- Overused font guard: Inter/Geist/Space Grotesk without brand justification
- Purple/violet gradient dilution: action/status/surface/accent roles must be derived from existing product context
- Nested card depth: beyond 2 levels repeat borders/fills/radii/shadows unnecessarily
- Bounce/elastic easing on routine controls: overshoot/spring effects rejected
- Extreme border radius: >=24px turns cards into blobs, compounds with nesting

### Specificity Gates
- Rejects "Revolutionize your workflow" hero clichés
- No generic "AI Powered" capsule badges without functional reason
- No generic AI icons (sparkle, star, magic, lightning, diamond, robot, AI orb) without product relevance

### Restraint Gates
- One meaningful transition carries more value than many ornamental effects
- Default surfaces stay still; motion communicates state, continuity, or causality
- Decoration carries meaning: domain artifacts, real product UI, data, photography, diagrams, brand motifs

### Variety Gates
- Rejects identical card grids (4+ siblings with identical icon/heading/paragraph sizing)
- Rejects monotonous spacing (same gap/padding token for all gap boundaries)
- Rejects hero metric layout centers on large number with tiny label and generic supporting stats

### Source Gates (E1-E58)

Hallmark's gates read the render. The E category reads the source, and these are the gates that
have no render-side equivalent — a page can pass all 57 of the above and still fail every one of
these. Full rows in `references/slop-patterns.md`; detection method and grep signatures in
`references/code-slop-guide.md`.

**Security gates — blocker-class where the input is user-controlled:**
- No secret, key, or token reachable from client code (`NEXT_PUBLIC_*`, `VITE_*`, or plain
  client imports): E39
- No `dangerouslySetInnerHTML` on unsanitized input: E38
- Every trust boundary validates server-side, regardless of client checks: E41
- No sensitive data in the URL, query string, or `localStorage`: E42
- No unvalidated redirect or fetch target taken from a parameter: E43

**Correctness gates:**
- Every effect that subscribes, times, or fetches returns a cleanup: E7, E14-E17
- Every fetch ships loading, error, and empty branches: E9
- No `async` passed directly to `useEffect`: E12
- One owner per fact — no derived or duplicated state kept in sync by an effect: E1, E2
- Stable list keys; no array index on a reorderable list: E21, E22
- Form ownership decided at first render, and form state reset on success: E56, E58

**Architecture gates:**
- Client boundary at the interactive leaf, never the route root: E11, E44
- One component, one responsibility: E19
- No prop drilling through three or more layers: E20
- Data access, business logic, and presentation separated: E25

**Type gates:**
- No `any`, `as` cast, or `!` standing in for a real type at a boundary: E26, E29, E30
- Every suppression comment carries its reason: E27

**Delivery gates:**
- No `console.log` left in the build: E40
- Unsized media reserved to prevent layout shift: E46
- Fonts self-hosted through the framework loader, not a render-blocking `<link>`: E49
- `package.json` matches actual imports — no dead dependency, no uninstalled import: E50
- One correctness test on money, auth, parse, and permission paths: E51
- Dead code and unused exports deleted: E54

## Integration with slop-audit

### How hallmark gates complement our skill:

1. **Pre-emit filtering**: hallmark gates run before code is handed back, preventing slop from entering the codebase
2. **Post-detection validation**: slop-audit can scan output after hallmark gates for residual patterns
3. **Confidence escalation**: hallmark's definitive "pass/fail" gates can upgrade slop-audit Candidate findings to Confirmed/Probable
4. **Theme-aware remediation**: hallmark's 20 themes provide concrete design alternatives when our skill detects V1-V21, T1-T18, C1-C11, L1-L13, M1-M18 patterns

### Combined workflow:

```
Step 1: Hallmark build() — runs 57 gates, picks macrostructure, applies rule-set, runs slop test
Step 2: If gates pass → run the Source Gates (E1-E58) → if those pass, code handed back
Step 3: If gates fail → slop-audit scan() detects specific patterns (V1-V21, T1-T18, C1-C11, L1-L13, M1-M18, E1-E58, etc.)
Step 4: slop-audit track() records findings in slop-log.json with confidence/severity
Step 5: slop-audit remediate() applies 8-stage fix order
Step 6: slop-audit report() generates standardized report with Direction/Findings/Exemptions/Validation
```

Hallmark's gates and the E category are not substitutes. Hallmark reads the render and refuses
the generic default; the E category reads the source and refuses the leak, the race, and the
unchecked boundary. A build that clears 57 gates and ships `any` through its whole boundary has
passed the wrong exam.

### Hallmark-inspired additions to slop-audit fix order:

Add after Stage 8 (Motion duration, frequency, and state signalling) as a final gate, and after Stage 1 for the source security tier:
- **Stage 9 — Hallmark design gates**: run hallmark's 57-gate pre-emit check plus the Source Gates (E1-E58) on the generated UI; fail-fast on any gate violation before code is persisted. The E security tier (E38, E39, E41-E43) belongs in Stage 1 instead — a leak does not wait for the last gate

### Report template enhancement:

Add Hallmark section to report-template.md:
```
HALLMARK
Gate results summary: PASS / FAIL count
Failed gates: [list with categories: philosophy/hierarchy/execution/specificity/restraint/variety]
Theme applied: [if any] | Rationale: [brief reason]
```

### Anti-pattern cross-reference table (expanded):

| Hallmark Gate Category | slop-audit Patterns | Example Anti-Pattern |
|------------------------|---------------------------|----------------------|
| Philosophy | — | "Revolutionize your workflow" hero headline |
| Hierarchy | T1, T8, T9 | Flat type hierarchy, single font for everything |
| Execution | V6, M1, C1 | Extreme border radius, bounce easing, purple gradients |
| Specificity | V8, V12, P2 | Generic AI icons, AI capsule badges, marketing buzzwords |
| Restraint | M3, Q6 | Image hover transform, tight line height |
| Variety | L2, L3, L4 | Identical card grids, monotonous spacing, nested cards |
| Source — security | E38, E39, E41-E43 | Secret in the client bundle, `dangerouslySetInnerHTML` on user input, unvalidated boundary |
| Source — correctness | E1, E7, E9, E14-E17 | Derived state, fetch with no abort, happy-path-only fetch, uncleaned lifecycle |
| Source — architecture | E11, E19, E25, E44 | `'use client'` on the root, god component, no module boundaries |
| Source — type | E26, E27, E29, E30 | `any`, reasonless suppression, `as` cast, `!` for a check |
| Source — delivery | E40, E46, E49, E51, E54 | `console.log` left in, unsized media, Google Fonts via `<link>`, no correctness test, dead code |