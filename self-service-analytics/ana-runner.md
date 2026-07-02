# Self-Service Analytics — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This file is the **module list** (the WHAT)
> for the Self-Service Analytics workshop. Delivery: **inline** (paste it), Markdown not PDF.
> Facilitation happens **in-thread, interactively — the learner runs each prompt, Ana coaches.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (the learner pastes this + the module list)

```
Hey Ana — facilitate the "Self-Service Analytics" workshop with me in this thread, on the data connected here.
Run it interactively: look at my data first (2–3 lines), then go ONE module at a time. For each module,
DON'T run the prompt yourself — give ME the prompt to copy and run as my next message, tell me what to look
for, then wait. When I run it, coach me on the result and hand me the next one. Start with what you see,
then Module 0.

[paste the module list below]
```

## Methodology (what this teaches)

A business user can get **trustworthy, governed answers themselves** — no SQL, no analyst. The arc:
ask in plain language → **verify** the answer → visualize & go deeper → turn answers into **assets**
(reports, dashboards) → **automate** → make Ana smarter over time.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) | What to look for (success check) |
|---|---|---|---|
| 0 | Setup & Orientation | *(Ana does this)* — "What data do I have here, and what are 5 good questions I could ask it?" | You see what data exists + 5 real questions |
| 1 | Your First Question | "What was **[main metric]** last month vs the month before?" then "**[a metric the team debates]** — tell me which definition you used." | A clear number + comparison, plain English |
| 2 | Trusting the Answer | "Explain exactly how you got that: table, filters, time window, definition — and which parts are **governed** vs computed **ad hoc**." | Ana shows the math + flags governed vs ad hoc |
| 3 | Visualize & Go Deeper | "Show **[metric]** by month for 12 months as a chart." then "Segment by **[dimension]** — which one drove the change?" | A chart renders; the driver is named |
| 4 | From Answers to Assets | "Turn this into a polished report (summary, charts, methodology)." then "Build a dashboard with filters for **[date]** and **[dimension]**." then "Export as CSV." | A report / dashboard / export is produced |
| 5 | Automate It | "Create a playbook '**[Team] Weekly Snapshot**' that runs Mondays 9am: **[metrics]** vs last week + anything unusual, posted to **[#channel]**." | A scheduled playbook (or feed) exists |
| 6 | Make It Smarter | "Our definition of **[metric]** is **[definition]** — save it (propose the patch)." then "Remember my preferences: **[concise / timezone / default window]**." | A definition/preference saved (proposed) + reused |

## When done

Recap in 3 bullets. Offer to save the learner's **Weekly Snapshot** as a Playbook for reuse. Point to the full
workshop site for reference. Do **not** write the workshop into the governed ontology.
