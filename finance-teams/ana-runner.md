# Finance Teams — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Finance Teams workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Finance Teams" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/finance-teams/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A workshop of short, hands-on modules for FP&A, controllers, finance ops, and CFO teams: close and flux analysis in plain English, revenue questions with the definitions done right, spend and vendor analysis, a board pack that assembles itself, a monthly rhythm that runs automatically — and the…

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | Map Your Finance Data | What financial data sources can you see in this thread? Identify which holds the general ledger, which holds billing/invoices, and which holds bookings or pipeline. For each, tell me the date range covered and how fresh the latest data is. Then find our account master / chart of accounts and, from now on, resolve every "total" I ask for through its parent-child hierarchy — never from account-ID ranges or name guesses. |
| 1 | Close & Flux | For [last closed month], show actuals vs budget and vs same month last year for each P&L line: revenue, COGS, gross margin, then opex by category. Flag any line where the variance exceeds [5%, or $50k] in either direction. |
| 2 | Revenue Questions | For [last quarter], show bookings, billings, and recognized revenue as three separate numbers, each labeled with its source system. Explain in one sentence each why they differ for us. |
| 3 | Spend & Vendors | Top 25 vendors by total spend over the trailing 12 months: total, monthly average, and the trend (growing/flat/shrinking). Flag any vendor whose last-3-month run rate exceeds their prior-9-month run rate by more than [20%]. |
| 4 | The Board Pack | Build a board pack from this thread's analysis: (1) executive summary — three findings, not descriptions; (2) P&L summary with variance vs budget and prior year; (3) revenue section — the bookings/billings/revenue view, NRR bridge, cohort table; (4) opex and vendor highlights; (5) methodology appendix — every source, definition, fiscal period, and filter used. Charts where they beat tables. |
| 5 | Automate the Rhythm | Create a playbook "Monthly Flux" that runs on the [5th business day] of each month: the variance table (actuals vs budget vs prior year, breaches flagged), the drill-down on the top two breaches with the timing-vs-run-rate verdict, and the flux narrative with methodology footnote. Use our fiscal calendar. If the period isn't fully closed, say so prominently rather than reporting partial numbers as final. Email it to [me and the controller]. |
| 6 | Govern the Definitions | Save to the ontology: our fiscal year [definition], fiscal quarters [list], and the rule that "quarter," "year," and "YTD" always mean fiscal periods unless someone explicitly says calendar. Propose this as a patch for review. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
