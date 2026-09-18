# Content Quality Evaluation Guidelines

Standards for evaluating content quality and identifying AI slop, integrated from multiple skill references.

## Quality Markers (Good Content)

- **Specific**: Contains concrete details, examples, or data specific to the topic
- **Grounded**: Claims are supported by evidence, citations, or personal experience
- **Varied**: Sentence structures vary; not formulaic or repetitive
- **Context-aware**: Shows understanding of the specific situation/problem
- **Actionable**: Provides clear next steps or specific guidance

## Slop Red Flags (AI-Low Quality)

- **Generic assertions** without supporting evidence
- **Formulaic transitions** that could apply to any topic
- **Overused phrases** that AI models favor (see slop-patterns.md)
- **Empty claims** like "the best," "state-of-the-art," "industry-leading" without qualification
- **Repetitive sentence structures** starting with the same patterns
- **Lack of source citations** for factual claims
- **Vague time/place references** that don't ground the content

## Remediation Guidance

When slop is detected, follow the 8-stage fix order:

1. **Broken behavior, accessibility, overflow, readability, correctness, false claims** — Fix critical issues first. Q9 (broken JS contract) and H15 (fabricated venue claim) are blockers
2. **Information architecture and unsupported content** — Remove redundant/unnecessary sections, ungraded embeds, unscrimmed video, hover-gated content
3. **Repeated page templates, card anatomy, container depth** — Address system-level repetition
4. **Type hierarchy, spacing rhythm, color roles** — Apply consistent typography and spacing
5. **Decorative borders, gradients, glows, radii, icons, motion** — Remove decorative defaults without purpose
6. **Copy cadence and redundant labels** — Fix formulaic copy patterns
7. **Imagery and placeholder issues** — Assets, AI artifacts, crops, media ratios
8. **Motion and layout violations** — Scroll-fade, uniform duration, reduced-motion, staggered reveals

## Tracking

All detected slop should be logged in `./.slop-tracker/slop-log.json` (in the scanned project's working directory) with:
- File path and line number
- Pattern ID (V1-V21, T1-T18, C1-C11, L1-L13, M1-M18, P1-P9, I1-I6, Q1-Q10, X1-X10, A1-A16, D1-D6, S1-S8, R1-R6, B1-B5, F1-F7, DM1-DM5, H1-H18, E1-E58)
- Confidence level (Confirmed/Probable/Candidate)
- Evidence class (Observed/Measured/Inferred/Unknown) — what the claim rests on, separate from how sure you are. Never promote inferred or unknown material into a factual claim, and never infer authorship from style alone
- Severity (blocker/major/nit)
- Evidence: source line citation
- Remediation action taken
- Status: pending/reviewed/removed
- Exemption: if pattern communicates real meaning, follows established brand, or serves functional interaction