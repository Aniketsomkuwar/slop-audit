# App Screens and Code Comments

Derived from antislop (MIT, `miqdadbadjuber/anti-slop`), merged and adapted. The rules cited here are defined in `rule-gate.md`. The patterns in this file cover two surfaces the landing-page catalog under-serves: signed-in app screens, and the comments left in source.

## App and dashboard screens

The landing-page patterns still apply. These are the shapes an agent reaches for when the screen is a dashboard, admin panel, or any signed-in view. The failure is the same: a layout picked from memory instead of from the work the screen supports.

### Default dashboard shell

- **Tell:** left sidebar, top bar, four stat cards, a chart, a table, chosen before anyone asked what the screen is for. Identical whether it manages invoices, patients, or servers.
- **Why:** it is the landing-page template problem in an app. Swap the labels and it belongs to any product.
- **Fix:** name the screen's job and the one decision the user makes on it, then build the hierarchy around that (C-3, R-20). If the job is "spot the failing job and retry it", the failing jobs are the page and the stat row is a footnote. Cut sections that survive only because dashboards usually have them.

### Stat cards with invented numbers

- **Tell:** four cards reading 12,483 / 94.2% / $48.2K / 1,204, each with a green "+12% this week" delta.
- **Why:** the numbers are decoration and the deltas are a trend claim with no series behind them. Four equal cards is already a hierarchy failure when the metrics do not carry equal weight.
- **Fix:** show real numbers or none (R-17, R-38). Wire the cards to real data, or ship the one metric that is real. A delta appears only when the comparison period is real and named. Label prototype values where the user can see them.

### Filler activity feed

- **Tell:** "Sarah Chen updated a document, 2 hours ago", repeated with rotating names and avatars.
- **Why:** invented people and invented events. It is the testimonial section in a different layout, and it makes an empty product look busy.
- **Fix:** real events or no feed (R-18, R-38). An honest empty state tells the user what to do first (R-27).

### Charts without a question

- **Tell:** a line or donut chart placed because the space looked bare, titled "Overview" or "Performance", with no axis the reader can act on.
- **Why:** a chart is an answer. Without the question it is texture, and it costs more attention than a sentence.
- **Fix:** write the question the chart answers before drawing it, and put the question in the title ("Failed jobs per hour, last 24h"). If a sentence answers it better, write the sentence. Chart segments still need 3:1 contrast against their neighbors (R-25).

### Generic table columns

- **Tell:** Name, Status, Date, Actions, whatever the rows are, with a three-dot menu on every row.
- **Why:** the columns come from the table component, not from the data. The field that decides the user's next move is missing.
- **Fix:** choose columns from the decision the user makes in this table, and put the deciding field early. The row menu holds actions that exist (R-26).

### Filler data in fields and columns

- **Tell:** empty fields and columns filled with plausible but fake data: `John Doe`, `johndoe@example.com`, "Let's build something", invented phone numbers and dates.
- **Why:** fabricated content disguised as real. It reads fine in a mockup and falls apart when a real user looks. It is the strongest tell that the screen was generated, not built.
- **Fix:** leave the cell empty or use an honest placeholder: `Your Name`, `email@example.com`, `Drop your message here...`, `[REAL DATA]` (R-23, R-38). Generic filler copy is buzzword slop and does not belong in a data column (R-16).

### Placeholder empty and loading states

- **Tell:** "No data available" with an illustration, a bare spinner, or a full-page skeleton that mimics a layout the real data never fills.
- **Why:** these technically satisfy R-27 and still tell the user nothing: no cause, no next action, no idea whether this is normal.
- **Fix:** the empty state says why it is empty and the one action that fills it. The loading state says what is loading. The error state says what failed and what to do next. First run, filtered-to-nothing, and permission denied are different screens.

### Endless motion

- **Tell:** elements that pulse, bounce, or float forever with no user trigger.
- **Why:** perpetual motion competes with the content and never lets the user rest. If MOTION is 1 (hover states only), an endless loop contradicts the declared dial.
- **Fix:** motion has a written UX purpose and a trigger (R-19). Animate the moment, not the wallpaper.

### App-screen checklist

- [ ] The screen is built around the one decision the user makes there, not the sidebar plus stat row plus chart plus table default.
- [ ] Every number, delta, feed entry, and table row is real or a labeled placeholder.
- [ ] Every chart answers a named question; every table column serves the decision.
- [ ] Empty fields stay empty or carry honest placeholders, never fake-looking data.
- [ ] Empty, loading, and error states name the cause and the next action.
- [ ] Motion is triggered, bounded, and consistent with the MOTION dial.

## Code comments

Scope guardrail: this section changes comments only. Never modify executable code, identifiers, imports, formatting, indentation, whitespace, control flow, or logic. When in doubt, leave the code untouched.

### Comments that add nothing

- **Decorative separators:** `// =====================`, `/* ---- ROUTES ---- */`, ALL-CAPS banners. The decoration is the message. Replace with one plain line or remove.
- **Restating the obvious:** `// Initialize the variable` above `let count = 0`, `// User class` above `class User {}`. The code already says it. Remove.
- **Workflow narration:** `// Step 1: Validate`, `// Next`, `// Finally`. The control flow is visible. Remove. If the flow is hard to follow, fix the structure.
- **Empty labels:** `// Main logic`, `// Business logic`, `// Helper function`, `// Note: This is important.` A label names a category, not a fact. Remove unless it carries specific information ("Note: retries happen only on 5xx").
- **Vague TODOs:** `// TODO: Improve this`, `// Future improvements`, `// Add more validation`. A vague TODO names a feeling. Keep one only when it names a specific task with context.
- **Signature echo:** a JSDoc block that repeats `@param price The price.` and `@returns Total price.` for a function whose name and parameters already say it. Keep documentation that explains business rules, edge cases, assumptions, algorithms, limitations, side effects, API behavior, or security implications.
- **Decorative emoji:** `// ✅ Validation`, `// 🚀 Performance`. Replace with plain English or remove.
- **End markers:** `} // end if`, `// End processOrder`. The brace already ends the block. Remove.
- **Line-by-line narration:** a comment on every trivial statement. One comment per logical block, or none.
- **Stiff or loud wording:** a sentence-long formal explanation, or `// MAIN LOGIC` in caps. Write short, sentence-case lines in a developer's voice, and explain why rather than what.

### Comments that must stay

Never remove comments that explain:

- business logic and intent
- architectural decisions
- security considerations
- performance trade-offs
- concurrency behavior
- protocol details and API contracts
- workarounds and known bugs
- edge cases and assumptions
- licensing and legal notices

Example that stays:

```js
// Stripe may retry webhook deliveries for up to three days.
// Ignore duplicate events using the event ID.
```

A comment earns its place when it explains something the code does not already show: the reason, the constraint, the non-obvious behavior.

### Comment checklist

- [ ] Every comment adds information the code does not show.
- [ ] No decorative separators, caps banners, or box-drawing.
- [ ] No restating the obvious line, declaration, or signature.
- [ ] No step-by-step workflow narration.
- [ ] No empty labels or vague TODOs.
- [ ] No decorative emoji or end markers.
- [ ] Density is one per logical block, not one per line.
- [ ] Remaining comments are short, natural, and sentence case.
- [ ] Only comments changed; the code is untouched.
