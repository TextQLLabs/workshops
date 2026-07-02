# Customer Success — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Customer Success" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/customer-success/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · Map Your Customer Data

**Prompt for the learner to run:**
```
Describe the customer data you can see: which source holds accounts and renewal dates, which holds product usage, which holds support tickets, and which holds invoices? For each, name the actual objects/tables and the key fields — account ID, renewal date, usage measure, ticket severity, invoice status. Then tell me how the sources link together (what joins an account to its usage and tickets?).
```

> ✅ You'll see: your data's real shape, including the join keys — the thing that makes one-prompt snapshots possible. Fill in the mapping; every later prompt uses the left column:

**Checkpoint before moving on:**
- [ ] You know which of the four pillars are connected and what's missing
- [ ] The translation table is filled in, including how sources join
- [ ] A sentinel account is chosen — one whose health you could state from memory

## Module 1 · Health Snapshot

**Prompt for the learner to run:**
```
How is [sentinel account] doing?
```

> ✅ You'll see: Ana either ask what you mean — health? usage? sentiment? commercials? — or state explicitly which lens she's answering through. That clarification is the lesson: "how's the account" is four different questions, and the snapshot below asks all four on purpose.

**Prompt for the learner to run:**
```
Build an account health snapshot for [sentinel account]: (1) usage trend — [your usage measure] over the last 90 days vs the prior 90; (2) support load — open and recent tickets with severity, and whether volume is rising; (3) sentiment signals — anything notable in recent ticket text or meeting notes; (4) commercials — ARR, renewal date, invoice status. End with a one-line health read: green/yellow/red and the single biggest factor.
```

> ✅ You'll see: the four pillars assembled into one card with a verdict. Calibrate: does the verdict match what you know? If Ana says green and your gut says yellow, ask her what she can't see — the answer is usually a pillar that isn't connected (champion sentiment living in your head, for example).

**Prompt for the learner to run:**
```
For that snapshot: how did you compute the usage trend — which table, which measure, what time windows? And which tickets did you count as recent?
```

> ✅ You'll see: the methodology in plain English. One spot-check buys trust for the whole book.

**Prompt for the learner to run:**
```
Across all accounts I own, build the same four-pillar read and rank by risk: declining usage, rising ticket load, negative signals, or invoice problems — weighted toward accounts with renewals in the next two quarters. Top 10 riskiest, with the one-line reason each.
```

> ✅ You'll see: your book, triaged. The Monday-morning question — "where do I spend this week?" — answered in one prompt. Module 5 schedules it.

**Checkpoint before moving on:**
- [ ] The vague question surfaced the four-lens ambiguity instead of guessing
- [ ] The sentinel snapshot's verdict matched your own read — or the divergence taught you what data is missing
- [ ] You spot-checked the methodology behind one snapshot
- [ ] The risk ranking surfaced at least one account you weren't already watching

## Module 2 · QBR Prep

**Prompt for the learner to run:**
```
Prepare QBR material for [account] covering [last quarter]: (1) value delivered — usage growth, features adopted, measurable outcomes, stated in their terms not ours; (2) the usage story — trend chart with annotations for notable changes; (3) open issues — unresolved tickets and their status, honestly framed; (4) expansion hooks — unused products/seats they're a fit for, usage patterns suggesting upsell readiness; (5) proposed agenda — three discussion topics based on all of the above. Format as a deck-ready summary: section headers, bullets, one chart per section where useful.
```

> ✅ You'll see: a structured QBR draft in about a minute. It will not be perfect — it will be the 80% that takes 80% of the time, leaving you the 20% that's actually your job: the relationship judgment, the framing, the asks.

**Prompt for the learner to run:**
```
For each claim in the value-delivered section: what's the evidence? Show the underlying number and comparison. Remove or soften anything that's plausible but not demonstrated in the data.
```

> ✅ You'll see: claims tightened to what's defensible. A QBR that overstates value once loses the room for a year — this pass is what makes the speed safe.

**Prompt for the learner to run:**
```
Produce that as a [PDF / slide deck], branded simply, with the account name and quarter on the title page.
```

> ✅ You'll see: the artifact, meeting-ready. (Report formats and theming are covered in depth in the Dashboards & Reporting workshop.) Now compare against your 2.1 number — the before/after is the story you tell your team.

**Checkpoint before moving on:**
- [ ] A full QBR draft generated for a real account, all five sections
- [ ] Every value claim survived the evidence pass (or was cut)
- [ ] The output is formatted and meeting-ready
- [ ] You know your before/after prep time, in hours

## Module 3 · Churn Signals

**Prompt for the learner to run:**
```
Scan my book for churn leading indicators: usage down more than [20%] quarter-over-quarter; key contacts gone quiet or departed; ticket volume spiking — or dropping to zero after being active; seats contracting; payment delays. List accounts with two or more signals, worst first.
```

> ✅ You'll see: the early-warning list. Two-plus signals is the threshold worth working; single signals are usually noise.

**Prompt for the learner to run:**
```
[Account]'s health dropped from green to yellow this quarter. Decompose why: did usage fall (which features, which users?), did support degrade (what are the tickets about?), did the relationship thin (who stopped engaging?), or did commercials change? Rank the contributing factors and show the evidence for each.
```

> ✅ You'll see: the why behind the score — which factor moved, concentrated where. A health score tells you to worry; the decomposition tells you what to do.

**Prompt for the learner to run:**
```
For [at-risk account]: what would have to be true for this account to renew? Based on their usage pattern, open issues, and engagement, list the conditions that correlate with renewal in accounts like this one — and which conditions are currently false.
```

> ✅ You'll see: a renewal thesis with the gaps named — your save-plan skeleton. Note what this is and isn't: a structured read of the evidence, not a prophecy. If you ask "will they churn?", expect a probability framing with assumptions, never a yes/no — that honesty is a feature.

**Checkpoint before moving on:**
- [ ] The signal scan flagged accounts on two-plus indicators, and at least one was news to you
- [ ] One health change decomposed into ranked, evidenced factors
- [ ] The renewal-truth prompt produced conditions you can act on
- [ ] A "will they churn" question got probabilities and assumptions, not false certainty

## Module 4 · Renewal & Expansion

**Prompt for the learner to run:**
```
All accounts with renewals in the next [two quarters]: renewal date, ARR, health read (from the four pillars), and a risk flag — red if two-plus churn signals, yellow if one, green otherwise. Order by renewal date. Total the ARR at risk in each color.
```

> ✅ You'll see: the renewal runway with dollars attached — the view your leadership actually wants, and the agenda for your renewal forecast call.

**Prompt for the learner to run:**
```
Across my book: which accounts have [product A] but not [product B]? Which use fewer than [60%] of their licensed seats? Rank by expansion potential.
```

**Prompt for the learner to run:**
```
Which of my accounts look like our best customers — similar [size, industry, usage intensity] to our top-spending accounts — but have contracts well below that tier? Rank the gap.
```

> ✅ You'll see: the product-gap list and the looks-like-but-spends-less list. The second cut is the subtler and usually richer one — it finds accounts whose ceiling is high, not just whose checklist has gaps.

**Prompt for the learner to run:**
```
For the top 5 whitespace accounts: check readiness signals — usage trending up, healthy support picture, recent engagement, no invoice friction. Which are ready for an expansion conversation this quarter, and which need health work first?
```

> ✅ You'll see: the whitespace list split into "propose now" and "stabilize first" — the difference between expansion motion and tone-deaf upsell. Hand the "propose now" list to your account executive counterpart (the pipeline side of that motion lives in TextQL for Sales & GTM ).

**Checkpoint before moving on:**
- [ ] The renewal runway shows dated, dollar-totaled risk in three colors
- [ ] Both whitespace cuts ran — product gaps and looks-like-but-spends-less
- [ ] The top whitespace accounts are split by readiness, not just fit
- [ ] ARR-at-risk totals are ready for your next forecast conversation

## Module 5 · Automate the Book

> **Note** — The mechanics — templates with backing tables, the 3-preview rule, agent editorial bars — live in the Automation Deep-Dive workshop. This module applies them to a book of business.

**Prompt for the learner to run:**
```
Build my backing table: every account I own, with columns Account, CSM, RenewalDate, and Tier, as a CSV I can download and use as the template's backing table.
```

**Prompt for the learner to run:**
```
Create a playbook "Weekly Account Brief" using this backing table, running Mondays at 7am, one report per row: for {{Account}}, the four-pillar health snapshot (usage trend, support load, signals, commercials), what changed since last week, and the one recommended action for {{CSM}}. If {{Account}} shows no activity at all this week, say so in one line — don't pad it. Note renewal proximity using {{RenewalDate}}.
```

> ✅ You'll see: one brief per account, every Monday, centrally collected per batch run. The no-activity edge case keeps quiet accounts from generating noise — and a quiet account near renewal is itself a signal the brief will surface.

**Prompt for the learner to run:**
```
Create a feed agent "Book Watch" that checks my book daily and posts ONLY when an account's trajectory changes: health color changes in either direction; usage moves more than [25%] week-over-week; a [sev-1] ticket opens at an account within [90] days of renewal; a key contact goes dark; or an invoice goes past due. Include the account, what changed, the renewal date, and one sentence of why it matters. Silent otherwise — including for accounts that are stably red; I know about those.
```

> ✅ You'll see: an exception-only watcher. The "stably red doesn't repost" clause is the editorial bar that keeps it readable — you need to hear about changes , not be reminded of known problems daily.

**Checkpoint before moving on:**
- [ ] The backing table covers your book, and the templated playbook generates one brief per account
- [ ] You previewed on the sentinel and the quietest account before running the full book
- [ ] Book Watch posts only on trajectory changes — with the stably-red clause in place
- [ ] You can sketch the full CS rhythm and what each piece replaces

