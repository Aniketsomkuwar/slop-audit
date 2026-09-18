# Code Slop Guide — Detection Method and Enforcement

Companion to the **E category** (E1-E58) in `references/slop-patterns.md`. The catalog says
what the defects are. This file says how to find them in a real tree, what each framework
changes about them, and what to wire up so they stop recurring.

Both skills carry this file byte-identical.

---

## Why source-side slop is a separate category

The V/T/C/L/M/P/I/Q/X families describe a **rendered artifact**. Everything in them can be
true of a screenshot. The E family describes the **source** that produced it, and none of it
is visible in the render.

The two failure modes are independent:

- A page can pass every render-side check and still leak a secret, race a fetch, and carry
  `any` through its entire boundary. It looks finished.
- A page can have immaculate source and still be visually generic. It is well-built slop.

Report both. A deliverable is not done because the screenshot is good.

**The gestalt tell.** Source-side slop is more reliably read as a pattern than as individual
findings. Four or more of these together means the tree was generated without a review pass:

component file doubling as its own data layer · `any` at every boundary · fetch inside
`useEffect` · `console.log` still present · unused exports · array-index keys · `'use client'`
on the root · no test on a path that matters.

Use the gestalt to set severity and to justify a rewrite over a patch — never as the finding
itself. Report the E-IDs.

---

## Detection method

E-patterns need the source, not a screenshot. Three passes, cheapest first.

1. **Grep pass.** Most E-IDs have a signature that is one regex away. Run these before
   reading anything:
   - `: any\b|as any|<any>` → E26
   - `@ts-ignore|@ts-expect-error` → E27
   - `\w+!\.` and `!\)` / `!;` in TS → E30
   - `dangerouslySetInnerHTML` → E38
   - `console\.(log|debug|info)` → E40
   - `NEXT_PUBLIC_|VITE_` near `key|secret|token|password` → E39
   - `setInterval|setTimeout|addEventListener|subscribe` → E14, E15, E17
   - `createObjectURL` → E16
   - `key=\{index\}|key=\{i\}` → E21
   - `useEffect\(\s*async` → E12
   - `!important|\bimportant:` → E34
   - `w-\[|h-\[|mt-\[|text-\[` → E33
2. **Read pass.** Read every file the grep pass flagged, plus every file that fetches,
   authenticates, or parses. Check each effect for its cleanup, each fetch for its error and
   empty branches, each boundary for its validation.
3. **Boundary pass.** Walk the trust boundaries explicitly: client → server, server →
   database, user input → DOM, parameter → redirect. E38, E39, E41, E42, E43 live here, and
   they are the only E-IDs that are blockers by default.

**Not every E-ID is a defect in every project.** E47 and E48 are judgment calls about a real
performance budget. E51 and E55 are absence-detected — they require knowing which paths
matter. State the scope you actually inspected and mark the rest unknown.

---

## What each framework changes

The E-IDs are React-shaped because that is where most generated UI lands. The equivalents:

| Stack | Differences that matter |
|---|---|
| **React + Vite** | E7-E12 all apply. No server layer, so E39/E41 move entirely to whatever backend the app calls — verify it exists rather than assuming it does |
| **Next.js App Router** | E11 and E44 are the dominant defects, and they are the same mistake at two levels: the directive placement and the resulting payload. E49 has a first-party fix (`next/font`). E48 becomes streamable via Suspense. Server Actions move E41 from "no route handler validation" to "no action validation" |
| **Next.js Pages Router** | No client/server split. E44 does not apply; everything else does. E49 still applies |
| **Vue / Svelte** | Rename the hooks: E1-E6 map to `computed`/`watch` misuse (`watch` where `computed` belongs is the same defect as E1), E7-E9 to `onMounted` fetch without abort, E14-E17 identically. E21-E23 are the same. E26-E31 become the framework's type layer |
| **React Native** | E32-E37 do not apply — there is no Tailwind or DOM. E38 becomes WebView injection. E45-E49 become image caching and bundle size |
| **No framework** | E1-E25 largely do not apply. E32-E43 and E51-E55 apply in full, and E15/E16/E17 get worse because nothing manages lifecycle for you |

State the stack you scanned. An E-ID reported against the wrong stack is a false positive.

---

## Enforcement

Detection finds the defect once. These stop it recurring.

**Lint rules that already exist** — enable rather than write:

| Rule | Covers |
|---|---|
| `@typescript-eslint/no-explicit-any` | E26 |
| `@typescript-eslint/ban-ts-comment` | E27 |
| `@typescript-eslint/no-non-null-assertion` | E30 |
| `react-hooks/exhaustive-deps` | E1, E4, E7 |
| `react/jsx-key` | E22 |
| `react/no-array-index-key` | E21 |
| `no-console` (warn, allow `warn`/`error`) | E40 |
| `no-restricted-syntax` on `dangerouslySetInnerHTML` | E38 |

**Pre-commit checklist** — the items no linter covers:

- [ ] Every `useEffect` that subscribes, times, or fetches returns a cleanup
- [ ] Every fetch has a loading, error, and empty branch
- [ ] Every trust boundary validates server-side, regardless of client checks
- [ ] No secret, token, or key reachable from client code
- [ ] The smallest correctness test exists for money, auth, parsing, and permissions
- [ ] `package.json` matches actual imports — no dead dependency, no uninstalled import
- [ ] The client boundary sits at the interactive leaf, not the route root

---

## Applying this to the fix order

E-IDs land across all eight stages rather than in one. The mapping is in
`references/slop-patterns.md` under **Fix Order**. Two rules hold:

- **Security and leaks are Stage 1.** E38, E39, E41, E42, E43 are blockers where the input is
  user-controlled. They outrank every aesthetic finding, the same way Q9 and H15 do.
- **Markup and styling are Stage 8.** E32-E37 are the cosmetic layer of the source. Fixing
  them before the architecture (E19-E25) means the extraction gets rewritten anyway.

---

## Scope and honesty

- **This is not an authorship detector.** Same rule as the rest of the catalog: report the
  observable defect, never an inference about what wrote the code. Generated code can be
  clean, and hand-written code can have every E-ID.
- **Static reading has limits.** E7, E8, E18, and E46 describe runtime behavior that source
  inspection can only suggest. Where the runtime was not exercised, say so and mark the
  finding Candidate rather than Confirmed.
- **Do not pad the list.** A tree with four real E findings is a better report than one with
  twenty, where sixteen are E33 arbitrary values in a file nobody will change again.
