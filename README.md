# slop-audit

Find AI-generated slop in a UI, a codebase or a piece of copy, with evidence and a fix order.

![slop-audit](assets/hero.png)

A page can pass every visual check and still leak a secret, race a fetch and carry `any` through its whole boundary. A codebase can be immaculate and still render the same card grid as every other generated product. This skill scans both surfaces: what the user sees, and the source that produced it.

It is the operational half of [slop-design](https://github.com/Aniketsomkuwar/slop-design). Design prevents; audit detects, tracks and orders the fixes.

## How it works

Four passes, run in order, each finding carrying a pattern ID, a file and line citation, an evidence class, a confidence level and a severity.

1. **Quality failures.** WCAG AA contrast, text against the viewport edge, justified text without hyphenation, skipped heading levels, tiny body text, dead interactive elements, happy-path-only states.
2. **Catalogued patterns.** 245 patterns across 18 categories: visual, typography, colour, layout, motion, copy, imagery, quality, composition, accessibility, design tokens, state coverage, real content, responsive, interaction, dark mode, hospitality and engineering.
3. **Composition.** The page as a whole: archetype uniformity, card-grid and price-pill aggregation, signature footer, graded embeds, retail menu chrome. For hospitality targets, the H1-H18 venue contract.
4. **Source.** The E1-E58 code contract: derived state, effects without cleanup, fetch in `useEffect` with no abort, god components, index keys, `any` and non-null assertions at the trust boundary, secrets in the client bundle, `dangerouslySetInnerHTML`.

Stage 1 blockers outrank every aesthetic finding: Q9 (broken JS contract), H15 (fabricated venue claim) and the E security tier (E38, E39, E41-E43).

## What is in the repo

| Path | Purpose |
|---|---|
| `SKILL.md` | The skill entry point: modes, passes, evidence model, report format |
| `references/slop-patterns.md` | The full 245-pattern catalog with detection rules |
| `references/design-spec.md` | The construction contract the audit measures against |
| `references/code-slop-guide.md` | E1-E58 source contract, grep signatures, per-framework equivalents |
| `references/fix-order.md` | Prioritised fix sequence to prevent cosmetic churn |
| `references/fix-order-summary.md` | Condensed eight-stage sequence with per-stage pattern IDs |
| `references/report-template.md` | Direction / findings / needs verification / exemptions / validation |
| `references/hallmark-gates.md` | 57 slop-test gates and 20 theme checks |
| `references/luxury-hospitality-protocol.md` | H1-H18 detection catalog, heuristics, exemptions |
| `references/content-quality-guidelines.md` | Copy and content quality rules |
| `references/regex-debugging.md` | Pattern-extraction pitfalls for the scan |

## Install

### With an AI agent

Paste [`prompt.md`](prompt.md). It detects the OS, asks where skills live, copies the skill, verifies the frontmatter and tells you how to run a first scan.

### Manually

```bash
git clone https://github.com/Aniketsomkuwar/slop-audit.git
mkdir -p ~/.agents/skills/slop-audit
cp -R slop-audit/SKILL.md slop-audit/references slop-audit/README.md ~/.agents/skills/slop-audit/
```

On Windows PowerShell, the same target is `%USERPROFILE%\.agents\skills\slop-audit`. If your agent reads skills from another directory, use that one instead.

Restart the session, then:

```text
/skill slop-audit --scan .
```

## Use

| Command | What it does |
|---|---|
| `/skill slop-audit` | Audit the current working directory |
| `/skill slop-audit --scan PATH` | Audit a specific directory |
| `/skill slop-audit --list` | List tracked findings with status, grouped by category |
| `/skill slop-audit --clear` | Remove resolved entries from the tracker |

A scan writes to the target project, never to this repo:

```bash
mkdir -p ./.slop-tracker/references ./.slop-tracker/notes
```

Findings are stored in `./.slop-tracker/slop-log.json` with an exemption trail. Read `references/report-template.md` before presenting results, and `references/fix-order.md` before fixing anything: it stops a cosmetic pass from hiding a Stage 1 blocker.

## Evidence model

Every finding states how it was established, not just that it exists:

- **Evidence:** Observed, Measured, Inferred or Unknown.
- **Confidence:** Confirmed, Probable, Candidate or Exempted.
- **Severity:** blocker, major or nit.

An inference is never promoted to a fact. Unknown scope is stated, not filled in.

## The pair

[slop-design](https://github.com/Aniketsomkuwar/slop-design) is the contract an agent reads before generating UI. This repo audits what already exists. Install both for the full loop.

Each repo is self-contained. A few references are byte-identical between them so either can be installed alone.

## Scope

- Reads the target project; writes only inside its `.slop-tracker/` directory.
- No network calls, no telemetry, no remote upload.
- It reports and orders fixes; it does not edit the audited project unless you ask.

## Credits and lineage

Created by Aniket Somkuwar. Integrates slop patterns from `miqdadbadjuber/anti-slop`, `discountry/ritmex-skills` (anti-ui-slop), and the motion principles popularised by Emil Kowalski: transform/opacity only, named transitions, duration scaled to frequency.

v3.5.0 extends the source-code catalog with material from `Vanszs/Anti-AI-UI` (MIT), `design-taste-frontend`, `emilkowalski/skills`, `impeccable` (Apache 2.0) and `leeskills` (MIT): the E category, A9-A16, and the evidence-class and completion-rule discipline.

Third-party material remains under its own license. See [`LICENSE`](LICENSE) for this repository's terms.

## Version

3.5.0. The skill frontmatter is the source of truth.
