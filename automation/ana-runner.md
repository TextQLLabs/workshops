# Automation — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Automation workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Automation" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/automation/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop on making analysis run without you: scheduled playbooks that deliver reports, templates that batch one analysis across many entities, and feed agents that watch your data continuously, collaborate with each other, and post what matters.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Automation Map | List the playbooks and feed agents that already exist in this org: name, owner, schedule, and delivery target. Flag anything that looks duplicated or that hasn't run successfully recently. |
| 1 | Your First Playbook | Turn this analysis into a playbook called "[name]" that runs every [Monday at 9am]: same metrics, same breakdowns, comparing to the prior period. Email it to me. |
| 2 | Reliable Prompts | Rewrite my "[name]" playbook prompt into the four-part structure: objective, explicit steps with table names, edge-case handling for nulls and incomplete periods, and an output format section with a sample report. Show me the before and after. |
| 3 | Delivery | *(Ana orients you)* — explain **Delivery** in 3 bullets, grounded in my connected data, then move on. |
| 4 | Templates | Turn my "[name]" playbook into a template backed by the attached CSV: treat each column header as a {{variable}} and reference {{Account}}, {{Segment}}, and {{CSM}} in the prompt. Run it for the [Acme Corp] row only as a test batch and show me where the batch results are collected. |
| 5 | Feed Agents | Create an agent that watches [your domain — e.g., weekly revenue and pipeline]. Every [weekday at 8am], check for meaningful changes vs. the trailing average, investigate anything anomalous one level deeper, and post to the Feed only when there's something genuinely worth knowing. If nothing notable happened, don't post. |
| 6 | Operate | List all playbooks and agents I own with their last run status and delivery target. For each, when was the output last meaningfully engaged with (Slack reactions/replies, feed comments)? Recommend what to pause. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
