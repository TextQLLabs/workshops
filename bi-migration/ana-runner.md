# Bi Migration — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Bi Migration workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Bi Migration" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/bi-migration/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A workshop of short, hands-on modules for analytics engineers and BI admins moving from Tableau, Looker, or Power BI toward TextQL — whether that's a full migration or long-term coexistence. The framing throughout: migrate questions, not artifacts. Your BI tool was the right tool for its era; this…

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | Inventory the Estate | From these files: how many dashboards exist in total, and how many had zero views in the last 90 days? Of the rest, what share of total views does the top 10% of dashboards get? List the top 20 by usage, and separately the zombie list — no views in 90+ days — with owners. |
| 1 | Translate the Semantic Layer | From this catalog, propose governed ontology metrics for the [15] metrics that power the top-20 dashboards. For each: the plain-English definition, the source tables and join logic in our warehouse, the dimensions it should support, and the original [LookML/calc/measure] it translates. Where the original embeds tool-specific behavior (a default date filter, a tool-side aggregation quirk), call it out explicitly rather than silently translating it. |
| 2 | Rebuild the Flagship | Rebuild this dashboard connected to our warehouse: match the layout, panels, and chart types; compute every metric through the governed ontology definitions we created (not ad-hoc SQL); add the same filters. List any panel you couldn't map to our data or definitions rather than improvising it. |
| 3 | Parity Testing | Create a golden parity table: for each of these [12] metric/period pairs from the old tool [paste them: metric, filters, period, value], compute the same figure through our governed definitions and record old value, new value, and the difference. Closed periods only. |
| 4 | What Doesn't Migrate | Take our top-20 dashboard list with its question mapping. Classify each: (a) rebuild as a live dashboard — genuinely monitored regularly; (b) replace with a scheduled playbook — it's really a recurring report; (c) replace with a feed agent — it's really a watch-for-changes; (d) retire to on-demand asking — it was an in-case-someone-asks. Recommend with one line of reasoning each. |
| 5 | Coexistence & Cutover | Using the [Tableau/Power BI] connector: what dashboards and views can you see? For [a not-yet-migrated dashboard], summarize what it shows and what question it answers. |
| 6 | Measure Adoption | Create a playbook "Migration Adoption Weekly" that runs Mondays at 8am: (1) TextQL-side — threads created, distinct and repeat users, shares, rebuilt-dashboard views, each vs the prior week; (2) old-tool side from the latest usage export — total views and views on migrated dashboards, trending; (3) the crossover chart — old-tool views vs TextQL activity on one timeline; (4) flags — any migrated dashboard whose old-tool usage ISN'T declining, and any week where new-tool activity drops. Deliver to [#migration-channel]. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
