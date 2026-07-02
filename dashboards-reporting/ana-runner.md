# Dashboards Reporting — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Dashboards Reporting workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Dashboards Reporting" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/dashboards-reporting/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop for anyone who turns analysis into something others consume: charts that communicate, polished reports, and live interactive dashboards — built entirely through conversation, then published, shared, refreshed, and maintained like real products.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Output Landscape | Rule of thumb: if the consumer will ask *follow-up questions of the data*, build a dashboard. If they want *your interpretation*, write a report. If they ask every week, schedule it (playbook for reports — see the Automation workshop; refresh schedule for dashboards — Module 4). |
| 1 | Charts That Communicate | Show me [your metric from 0.3] by [week/month] for the last 12 months as a chart. |
| 2 | Reports: Narrative Outputs | Turn this thread's analysis into a report with this structure: (1) Executive summary — three bullets max, findings not descriptions; (2) The headline number with its comparison; (3) Key charts, each with a one-paragraph takeaway underneath; (4) What we should do — up to three recommendations with owners; (5) Methodology appendix — sources, definitions, filters, time windows. |
| 3 | Build Your First Dashboard | Requires Enable Dashboards (Settings → Capabilities; admin, once per org). Dashboards are in Public Preview — interfaces may evolve. |
| 4 | Publish, Share & Operate | Two viewers can legitimately see different numbers on the same dashboard if row-level security scopes them differently. That's a feature — but tell your viewers, or you'll get "the dashboard is wrong" tickets that are actually RLS working. |
| 5 | Data Sources & Performance | When a governed definition exists, prefer Ontology SQL for headline KPIs. A dashboard that computes "active users" its own way is how two dashboards end up disagreeing in the same meeting — exactly what the ontology exists to prevent. |
| 6 | Troubleshooting Clinic | Query the underlying table directly: what is the max [timestamp column]? Does it match what the dashboard's last-refresh implies it should be? |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
