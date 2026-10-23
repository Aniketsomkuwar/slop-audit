# Copywriting - Patterns, Guardrails, and Voice

Derived from antislop (MIT, `miqdadbadjuber/anti-slop`), merged and adapted. Use with the P, T, and Q patterns in the catalog. The rules cited here are defined in `rule-gate.md`.

Two rules cover everything below:

- **Never invent a fact.** A rewrite adds no name, number, date, quote, or citation that is not in the source or supplied by the user. Specificity comes from the source, not from the rewrite.
- **Do not over-sterilize.** Copy with no voice is as machine-made as copy full of tells. When the user supplies a voice, keep it.

## Tone and vocabulary

| Pattern | Before | After |
|---|---|---|
| Empty AI vocabulary (unlock, elevate, empower, delve, showcase, testament, journey, robust, game-changer, next-level, seamless, cutting-edge, revolutionary) | Unlock the power of seamless collaboration to elevate your team's journey to the next level. | Work with your team in one shared space. |
| Significance inflation ("the future of X", "pivotal moment", "a new era of") | The platform is marking a pivotal moment in the evolution of team productivity, ushering in a new era of work. | The platform cuts the time your team spends on status meetings. |
| Evidence-free social proof | Trusted by thousands of teams worldwide. Industry-leading technology loved by customers everywhere. | Used by the support teams at [real customer names]. If there are none, cut the claim. |
| Weasel attribution | Experts say this approach dramatically improves conversion. | Name the source and the real figure, or cut the sentence. |
| Persuasive authority tropes ("at its core", "the real question is", "what really matters") | At its core, what really matters is whether your team can move faster. | Whether your team can move faster depends on how quickly you can merge changes. |
| Chatbot closers | Here is an overview of our pricing. I hope this helps! Let me know if you'd like a breakdown. | Here is our pricing. The Starter tier includes three seats and community support. |
| Fake-candid openers ("Honestly?", "Here's the thing", "Real talk") | Is it worth the price? Honestly? It depends how often you use it. | Whether it is worth the price depends on how often you use it. |
| Signposting ("Let's dive in", "Here's what you need to know") | Let's dive into how caching works. Here's what you need to know. | Next.js caches data at multiple layers: request memoization, the data cache, and the router cache. |
| All-caps emphasis inside a paragraph | This is our last chance, and WE MUST ACT IMMEDIATELY. | This is our last chance. If we do not respond today, they will choose a competitor. |
| Actorless passive | The pricing page was updated to reflect the new tiers. | We rewrote the pricing page to show the new tiers. |
| Inanimate subject with a human verb (understands, knows, decides, wants, believes, cares) | The dashboard understands what your team needs and surfaces the right numbers. | The dashboard opens on the three metrics your team checks every morning. |

Passive and product verbs are not banned. The tell is passive chosen by default when the actor was available, or a verb that needs a mind behind it.

## Rhythm and structure

| Pattern | Before | After |
|---|---|---|
| Forced rule of three | Attendees can expect innovation, inspiration, and industry insights. | The event includes talks, panels, and time for informal networking. |
| Negative parallelism ("not just X, it's Y") and tailing negations ("no guessing") | It's not just a dashboard, it's a command center. No guessing. | The dashboard shows the data you select, so you do not have to guess. |
| Aphorism formulas ("X is the language of Y") | Symmetry is the language of trust. | Symmetric layouts feel more predictable to users. |
| Staccato drama (a run of clipped fragments) | Then the old rules were gone. No templates. No defaults. No safety. | The old rules no longer applied, and every page had to be designed from scratch. |
| Synonym cycling | The checkout is fast. The process is quick. The flow is speedy. | The checkout is fast. Everything happens in three clicks. |
| False ranges ("from X to Y" with no scale) | From first touch to final invoice, and everything in between. | Handles quotes, invoices, and payment reminders. |

## Honesty

| Pattern | Fix |
|---|---|
| Fabricated specifics (numbers, testimonials, names, dates, features) | Remove them. A specific-looking fabrication is a defect even when it reads more human than the vague original. |
| Speculative gap-filling ("likely founded in the 1990s") | State that the fact is not documented, or omit the sentence. |
| Generic positive conclusion ("the future looks bright") | End on the last concrete fact, or state real plans. |

## Hygiene

| Pattern | Fix |
|---|---|
| Em dash, spaced em dash, or `--` used as a connector | Period, comma, colon, parentheses, or restructure. See R-02. |
| Boldface on every key term | Bold only where the reader must not miss it. |
| Quotes used as default emphasis or hedging | Quotes for dialogue, real citations, and titles only. |
| Inline-header list items (`- **User Experience:** ...`) | Write the sentence, or use a table when the fields are genuinely parallel. |
| Emoji in headings, bullets, or UI copy | Remove. Use a relevant icon or nothing. |
| Filler phrases ("in order to", "due to the fact that", "it is important to note") | "to", "because", "now", or delete. |
| Stacked hedges ("could potentially possibly") | One qualifier. |

## What not to flag

A human writer can hit several of these without AI involvement. Do not gut legitimate prose.

- Perfect grammar and consistent style. Professional editing is not a tell.
- Mixed casual and formal registers.
- Dry or "robotic" prose without the specific tells.
- Formal vocabulary. AI overuses specific words, not all fancy words.
- One "however" or "additionally". They count when piled up.
- Curly quotes alone. Most editors and CMSes curl by default.
- Em dashes alone. They count only inside a cluster. R-02 still bans them in output, but do not rewrite the user's deliberate style without saying so.
- One short emphatic sentence. Staccato is a run, not a line.
- Unsourced claims. Most of the web is unsourced.
- Secondhand text: quotations, titles, proper names, and examples being discussed.

Look for clusters, not isolated tells.

## Signs of human writing (preserve)

- Specific, unusual, hard-to-fabricate detail: a real address, a weird quote, an exact date.
- Mixed feelings and unresolved tension.
- Dated, era-bound references.
- Variety in sentence length.
- Genuine asides and self-corrections.

## Voice calibration

If the user supplies a writing sample, read it first: sentence lengths, vocabulary, openings, punctuation, recurring phrases. Match those habits instead of only deleting patterns. Do not upgrade casual words or regularize deliberate quirks. The sample outranks these rules, including em dash frequency, but only for text the user authorized.

## Draft, audit, final

1. **Draft.** Rewrite, read it aloud, vary sentence length, keep the register.
2. **Audit.** Answer two questions: what makes this obviously AI generated, and does it state any fact not in the source? A fabrication is a defect even when it sounds better.
3. **Final.** Revise against both answers. Check em and en dashes one last time.

## Checklist

- [ ] No fabricated numbers, testimonials, names, dates, or claims.
- [ ] No R-16 buzzwords or empty AI vocabulary.
- [ ] No em dashes in output, unless the user's sample voice uses them.
- [ ] Quotes carry meaning, not emphasis.
- [ ] No all-caps emphasis clauses.
- [ ] Every sentence names its actor where an actor exists.
- [ ] CTAs name the action (R-15).
- [ ] No forced trios, negative parallelism, staccato runs, aphorism formulas, or false ranges.
- [ ] The copy has a voice, not a sterile default.
- [ ] It sounds like a person wrote it.
