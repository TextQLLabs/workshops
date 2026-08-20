# Build Data Apps with Ana — Ana-Led Runner (BETA)

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). Module list for the Data Apps workshop.
> **Data Apps is in beta** — confirm it's enabled in the workspace before starting.
> Delivery: **inline** or give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Build Data Apps with Ana" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/data-apps/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

Dashboards show people answers; data apps let them interact. The arc: a trusted analysis → a generated app → viewer-driven inputs and what-ifs → shared under the viewer's own permissions → maintained conversationally, with every metric traced to a governed definition.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | Dashboards vs Data Apps | What can I build from this workspace beyond charts and dashboards? Explain what a data app is here, what it can and can't do in this beta, and show me one example structure based on my connected data. |
| 1 | From Analysis to App | Take the analysis in this thread [or: rebuild my analysis of [metric] by [dimension]] and turn it into a data app: the headline number up top, the trend chart, and the breakdown table — with the layout you'd recommend. Show me a preview before saving it. |
| 2 | Make It Interactive | Add interactivity: a [region/team] filter, a date-range selector defaulting to the last 90 days, and a what-if input for [assumption] that recalculates the projection live. Wire them so every component honors the selections. |
| 3 | Governance & Sharing | Explain how this app behaves for different viewers: what does a user in the [restricted role] see vs an [analyst role]? Then share the app with [team/role] and show me exactly what they'll see and do. |
| 4 | Operate & Iterate | Update the app: add [a new metric card], change the default date range, and tell me which governed ontology definitions each component uses — propose ontology patches for anything ad hoc. |
| 5 | Import a Power BI / Tableau Dashboard | Two paths: (A) upload the [.pbit / .twb] file, or (B) read the connected Power BI/Tableau source directly. EITHER WAY attach 2–3 screenshots of the original — they drive look & feel. Read the model, measures, and layout + study the screenshots; don't build yet. Map each measure to a governed ontology metric (flag any that don't map), propose the app layout matching the screenshots, and name the tables/joins each metric needs. Then build and reconcile the default numbers exactly + one filtered slice to the dollar vs a fresh warehouse query. |
| 6 | Make It Fast & Troubleshoot | This app is slow / times out on filter. Trace one filter click: how many warehouse queries fire, on which tables, how many rows — is the bottleneck scans or payload? Then re-architect static-first (pre-aggregate once at refresh → snapshot → filter in-browser → one bounded query for un-cacheable distinct counts), check every dimension for >1 row per key (double-count), and reconcile default + a filtered slice to the dollar. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
