# Finance Teams — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Finance Teams" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/finance-teams/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · Map Your Finance Data

### 0.1 · What's connected?

**Prompt for the learner to run:**
```
What financial data sources can you see in this thread? Identify which holds the general ledger, which holds billing/invoices, and which holds bookings or pipeline. For each, tell me the date range covered and how fresh the latest data is.
```

> ✅ You'll see: an inventory of your GL/ERP, billing, and CRM sources with freshness. If a system you expected is missing, it wasn't attached before this thread's first message — start a new thread with it selected from the "+" menu, or ask your admin to connect it (the Connect Your Data workshop covers setup).

### 0.2 · Teach the fiscal calendar
Finance time is not calendar time, and this is the single highest-leverage thing to establish first.

**Prompt for the learner to run:**
```
Our fiscal year [starts February 1 / matches the calendar year]. Our fiscal quarters are [Q1: Feb–Apr, ...]. When I say "this quarter" or "Q3," always interpret it fiscally. Confirm by telling me today's fiscal period and what date range "last quarter" means.
```

> ✅ You'll see: Ana restate the calendar and resolve "last quarter" to exact dates. In Module 6 you'll save this permanently to the ontology so every user gets it automatically; for now it holds for this thread.

### 0.3 · The reconciliation — tie one number
Pick a number you trust absolutely: last quarter's revenue from the financial statements, or last month's closed opex from the close package.

**Prompt for the learner to run:**
```
What was [total revenue / total opex] for [last closed fiscal period]? I'm going to check this against [the income statement / close package] — show the number and exactly which source and accounts you used.
```

> ✅ You'll see: the number plus its derivation. If it matches: calibration confirmed. If it doesn't: the difference is almost always definitional — accrual vs cash timing, excluded entities or account ranges, or unposted journals in a fresher source. Triage it:

**Prompt for the learner to run:**
```
Your number differs from my statement by [amount]. Diagnose: are you including [intercompany / unposted journals / all entities]? Which accounts are in your total that might not be in mine? Show the reconciling items.
```

### 0.4 · Orientation

**Prompt for the learner to run:**
```
Given this finance data, give me 8 example questions I could ask — two each for close/flux, revenue, spend, and cash.
```

> ✅ You'll see: a menu calibrated to your actual data — a preview of Modules 1–3.

**Checkpoint before moving on:**
- [ ] Ana correctly identified the GL, billing, and bookings sources with freshness dates
- [ ] "Last quarter" resolves to the correct fiscal dates, not calendar dates
- [ ] One headline number ties to a trusted statement — or every reconciling item is explained
- [ ] The example-question menu matches what your team actually needs to ask

## Module 1 · Close & Flux

### 1.1 · The variance table

**Prompt for the learner to run:**
```
For [last closed month], show actuals vs budget and vs same month last year for each P&L line: revenue, COGS, gross margin, then opex by category. Flag any line where the variance exceeds [5%, or $50k] in either direction.
```

> ✅ You'll see: the flux table with breach flags — the skeleton of the close package, produced in seconds. If budget data lives in a file rather than a system, attach the budget spreadsheet to the thread first and say so in the prompt.

### 1.2 · Drill to the driver
Pick the worst breach. Variance analysis earns its keep in the drill-down:

**Prompt for the learner to run:**
```
Opex is over budget. Drill down: which account drove it? Within that account, which department? Within that department, which vendor or transaction? Keep drilling until you reach the specific driver, and show the chain.
```

> ✅ You'll see: a chain like opex → professional services → engineering → [vendor] → contract that started mid-quarter . This account → department → vendor descent is the workhorse move of this module — it works on any breach.

### 1.3 · Is it timing or is it real?
The controller's question for every variance:

**Prompt for the learner to run:**
```
For that variance: is this a timing difference (expense landed in a different month than budgeted) or a true run-rate change? Check the trailing 3 months and whether an offsetting swing appears in adjacent months.
```

> ✅ You'll see: a timing-vs-real verdict with evidence. Timing variances get a one-line explanation; run-rate changes get escalated — and the distinction is exactly what reviewers ask about.

### 1.4 · The flux narrative

**Prompt for the learner to run:**
```
Write the flux narrative for the close package: one paragraph per material variance, stating the amount, the driver we found, whether it's timing or run-rate, and the expected effect next month. Professional tone, no speculation beyond the data, and append a methodology footnote: sources, accounts included, and the period.
```

> ✅ You'll see: close-package-ready prose with a methodology footnote. That footnote is the habit to keep for anything audit-facing — every number Ana produces carries inspectable SQL underneath (expand the collapsed cells above any answer to see it), which means your flux narrative has an audit trail by default.

### 1.5 · The trust check
Before this narrative goes anywhere official:

**Prompt for the learner to run:**
```
For the largest variance in that narrative: show me exactly how you calculated both the actual and the budget figure — source, accounts, filters, period. What assumptions did you make?
```

> ✅ You'll see: the full derivation in plain English. Make this a reflex for any number leaving the finance team.

**Checkpoint before moving on:**
- [ ] The variance table flags the same breaches your close process would have caught
- [ ] One breach was drilled to a specific named driver (account → department → vendor)
- [ ] A variance was correctly classified as timing vs run-rate with evidence
- [ ] The flux narrative includes a methodology footnote, and you saw the SQL behind its largest number

## Module 2 · Revenue Questions

### 2.1 · Three numbers that are not the same number
The most common source of cross-team revenue arguments is three metrics wearing one name:

**Prompt for the learner to run:**
```
For [last quarter], show bookings, billings, and recognized revenue as three separate numbers, each labeled with its source system. Explain in one sentence each why they differ for us.
```

> ✅ You'll see: three distinct numbers with sources. If any two come back identical or from the same source, stop — either a system isn't connected or the definitions are conflated. That's a Module 6 patch waiting to happen, and you just caught it before it cost an argument.

### 2.2 · Deliberately vague, on purpose
Ask the question the CEO asks:

**Prompt for the learner to run:**
```
How did revenue do last quarter?
```

> ✅ You'll see: one of two good behaviors — Ana either asks which revenue you mean (recognized? bookings?) or states her assumption explicitly before answering. Either way, the ambiguity surfaces instead of hiding. This is the behavior that prevents the wrong number reaching a board deck.

### 2.3 · Retention: NRR and GRR

**Prompt for the learner to run:**
```
Calculate net revenue retention and gross revenue retention for [last quarter]: starting recurring revenue from the cohort active a year prior, plus expansion, minus contraction and churn. Show the bridge — starting ARR, expansion, contraction, churn, ending — not just the percentages.
```

> ✅ You'll see: the NRR/GRR bridge. The bridge format matters: percentages get quoted, bridges get understood. If your definition differs (monthly cohorts, logo-based), state it in the prompt — and save it in Module 6.

### 2.4 · Cohorts and segments

**Prompt for the learner to run:**
```
Break recurring revenue into annual cohorts by start year. For each cohort: starting ARR, current ARR, and net retention to date. Then cut current ARR by [segment / plan / region] — which segment is expanding fastest and which is leaking?
```

> ✅ You'll see: the cohort table and segment cut — where "how's revenue" becomes "which revenue, from whom, and is it durable."

### 2.5 · The question the data can't answer

**Prompt for the learner to run:**
```
What will churn be next quarter?
```

> ✅ You'll see: Ana decline to state it as fact — offering instead a trend-based projection with explicit assumptions and uncertainty, clearly labeled. No invented certainty. If a finance tool ever answers a forward question without caveats, that's when to worry.

**Checkpoint before moving on:**
- [ ] Bookings, billings, and revenue came back as three distinct, separately-sourced numbers
- [ ] The vague revenue question surfaced the ambiguity instead of silently guessing
- [ ] The NRR bridge components sum correctly (start + expansion − contraction − churn = end)
- [ ] A forward-looking question got a labeled projection with assumptions, not a fake fact

## Module 3 · Spend & Vendors

### 3.1 · The vendor table

**Prompt for the learner to run:**
```
Top 25 vendors by total spend over the trailing 12 months: total, monthly average, and the trend (growing/flat/shrinking). Flag any vendor whose last-3-month run rate exceeds their prior-9-month run rate by more than [20%].
```

> ✅ You'll see: the vendor landscape with run-rate creep flagged — the renewals-negotiation hit list.

### 3.2 · Creep, decomposed

**Prompt for the learner to run:**
```
For [flagged vendor]: monthly spend for 18 months, annotated with when the run rate shifted. Is the increase price (same items, higher cost), volume (more usage/seats), or new line items? Show the decomposition.
```

> ✅ You'll see: price vs volume vs scope — three different conversations with the vendor, and the data tells you which one to have.

### 3.3 · Duplicates and anomalies

**Prompt for the learner to run:**
```
Scan the last 6 months of payables for anomalies: potential duplicate payments (same vendor, same or near-same amount, within 10 days), vendors with suspiciously similar names, amounts just under approval thresholds, and any vendor paid for the first time this quarter at over [$25k].
```

> ✅ You'll see: an exceptions list. Most items will have innocent explanations — the point is that the scan takes thirty seconds instead of never happening. Anything that survives a first look gets the methodology-footnote treatment before escalation.

### 3.4 · Budget vs actual, by owner

**Prompt for the learner to run:**
```
Budget vs actual year-to-date by budget owner: amount over/under, percent consumed vs percent of year elapsed, and projected full-year landing at current run rate. Order by projected overrun, worst first.
```

> ✅ You'll see: accountability-ready numbers — who's pacing hot before it becomes a Q4 surprise. This exact prompt becomes the budget-break feed agent in Module 5.

**Checkpoint before moving on:**
- [ ] The top-vendor table surfaced at least one run-rate creep you'll act on
- [ ] One creep decomposed into price vs volume vs scope
- [ ] The anomaly scan ran and exceptions got explained or escalated
- [ ] Budget-vs-actual by owner shows projected landings, not just YTD positions

## Module 4 · The Board Pack

### 4.1 · Assemble the report

**Prompt for the learner to run:**
```
Build a board pack from this thread's analysis: (1) executive summary — three findings, not descriptions; (2) P&L summary with variance vs budget and prior year; (3) revenue section — the bookings/billings/revenue view, NRR bridge, cohort table; (4) opex and vendor highlights; (5) methodology appendix — every source, definition, fiscal period, and filter used. Charts where they beat tables.
```

> ✅ You'll see: a structured pack assembled from work you already did. The methodology appendix is non-negotiable for a board artifact — it's the difference between a deck and a defensible deck.

### 4.2 · Format it

**Prompt for the learner to run:**
```
Produce that as a PDF, under [10] pages, charts embedded, with [our fiscal period] in the title and a date stamp.
```

> ✅ You'll see: a downloadable PDF. Want slides instead? Ask for a deck version — one finding per slide. Reports are snapshots: date-stamp, distribute, regenerate next period (the Dashboards & Reporting workshop covers report craft in depth).

### 4.3 · The live companion dashboard
Board members and executives ask follow-ups. Give them a place to self-serve:

**Prompt for the learner to run:**
```
Build a dashboard companion to this pack: KPI row (revenue, gross margin, opex, cash) with vs-budget deltas; the P&L variance table; revenue trend with budget line; top-10 vendors. Add filters for fiscal period and [entity/region] that apply to everything. Default to the last closed period.
```

> ✅ You'll see: a live dashboard (depending on your workspace configuration, an admin may need to enable Dashboards under Settings → Capabilities). Publish it, set its refresh schedule to match your close cadence — and state that cadence on the dashboard itself ("Data as of close, refreshed monthly") so nobody mistakes mid-close numbers for finals.

### 4.4 · Verify before it ships

**Prompt for the learner to run:**
```
Before this pack goes out: recompute the headline revenue and total opex independently with fresh queries and confirm they match the pack. Then confirm both filters on the dashboard actually change the data shown.
```

> ✅ You'll see: an independent verification pass. A board pack with a wrong number costs more credibility than ten board packs build.

**Checkpoint before moving on:**
- [ ] The pack has all five sections, with the summary written as findings
- [ ] The PDF is date-stamped with the fiscal period in the title
- [ ] The companion dashboard filters work and its refresh cadence is stated on it
- [ ] Headline numbers survived independent recomputation before distribution

## Module 5 · Automate the Rhythm

> **Note** — The mechanics (the 3-preview rule, prompt structure, delivery options) live in the Automation Deep-Dive workshop. This module applies them to the finance calendar.

### 5.1 · The monthly flux playbook

**Prompt for the learner to run:**
```
Create a playbook "Monthly Flux" that runs on the [5th business day] of each month: the variance table (actuals vs budget vs prior year, breaches flagged), the drill-down on the top two breaches with the timing-vs-run-rate verdict, and the flux narrative with methodology footnote. Use our fiscal calendar. If the period isn't fully closed, say so prominently rather than reporting partial numbers as final. Email it to [me and the controller].
```

> ✅ You'll see: Module 1, scheduled. The "if not closed, say so" edge case is the finance-specific discipline — a flux on half-posted numbers is worse than no flux. Preview it three times before trusting it, including once mid-close.

### 5.2 · The daily cash position

**Prompt for the learner to run:**
```
Create a playbook "Daily Cash" that runs every business day at 7:30am: cash balance by account, total vs yesterday and vs 30 days ago, large movements (over [$100k]) itemized, and a 13-week trend chart. Concise — five lines and a chart. Post it to [#finance].
```

> ✅ You'll see: the daily cash discipline, automated. Posting to a channel (depending on your workspace configuration, Slack or Teams) beats email here — the team can ask follow-up questions directly in the thread.

### 5.3 · The budget-break and journal watcher
Playbooks report on schedule; for watching , use a feed agent that only speaks when something matters:

**Prompt for the learner to run:**
```
Create a feed agent "Budget Watch" that checks daily and posts ONLY when: a budget owner's projected full-year landing crosses over budget; a single journal entry over [$250k] posts outside the close window; a new vendor exceeds [$50k] in its first 90 days; or month-to-date spend in any category runs more than [30%] ahead of pace. Include the item, the threshold crossed, and one sentence of context. Silent otherwise.
```

> ✅ You'll see: an exception-only watcher. The editorial bar is what makes it useful — finance does not need another daily digest; it needs to hear about the journal entry that posted at 11pm on a Saturday.

### 5.4 · The rhythm, assembled

**Checkpoint before moving on:**
- [ ] The flux playbook handles the not-yet-closed edge case explicitly
- [ ] Daily cash posts somewhere the team can ask follow-ups in thread
- [ ] Budget Watch has hard thresholds and stays silent on normal days
- [ ] You previewed each automation before deploying — including the flux mid-close

## Module 6 · Govern the Definitions

### 6.1 · Why this module pays for the workshop
Every mismatch you triaged today — the reconciliation gap in Module 0, the bookings-vs-revenue conflation risk in Module 2 — came from definitions living in people's heads. Saving them to the Ontology (your org's governed semantic layer) means every user, every playbook, and every dashboard computes them the same way. When Sales asks "what was revenue," they get Finance's number.

### 6.2 · The fiscal calendar, permanently

**Prompt for the learner to run:**
```
Save to the ontology: our fiscal year [definition], fiscal quarters [list], and the rule that "quarter," "year," and "YTD" always mean fiscal periods unless someone explicitly says calendar. Propose this as a patch for review.
```

> ✅ You'll see: a change proposal (patch) created — like a pull request for business logic. An admin or designated reviewer approves it; from then on, Module 0.2 never needs to happen again, for anyone.

### 6.3 · The revenue definitions

**Prompt for the learner to run:**
```
Save to the ontology: "bookings" means [definition, source system]; "billings" means [definition, source]; "revenue" unqualified always means recognized revenue from the GL, accounts [range], excluding [intercompany/etc.]. Also save our NRR definition: [cohort basis, expansion/contraction/churn rules]. Propose as a patch.
```

> ✅ You'll see: the patch that ends the bookings-vs-revenue argument permanently. This is the single most valuable definitional patch most companies can make.

### 6.4 · The hierarchies

**Prompt for the learner to run:**
```
Save to the ontology: our department hierarchy [or: read it from the attached file] — which departments roll into which cost centers and functions; and our entity structure — which entities consolidate, what's eliminated. Propose as a patch.
```

> ✅ You'll see: the rollups codified, so "engineering opex" means the same set of departments in every analysis.

> **Go deeper** — Role personas — Finance sees methodology footnotes by default while executives get summary-first, from one governed model — are the Context Stack workshop.

### 6.5 · Finance as reviewer
Definitions will keep evolving, and other teams will propose changes that touch financial terms. Ask your admin to set the finance lead as a reviewer for finance-related ontology folders (folder-level access and review routing are covered in the Ontology Operations workshop — worth sending to whoever owns your ontology). The operating posture: Finance doesn't just use the governed definitions; Finance owns them.

**Checkpoint before moving on:**
- [ ] The fiscal calendar patch is proposed (or approved)
- [ ] Bookings / billings / revenue / NRR definitions are proposed as patches
- [ ] Department and entity hierarchies are codified
- [ ] A finance owner is designated as reviewer for finance definitions

