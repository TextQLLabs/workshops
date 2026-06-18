# Ontology Operations — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Ontology Operations workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> ⚙️ Setup/operational workshop — some steps are console/config, not data queries; Ana guides rather than runs them.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Ontology Operations" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/ontology-operations/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

The follow-on to Build Your Ontology, End to End: you've built a governed semantic layer — this workshop is about running it in production. Git sync and contribution workflows, folder-level access control, accuracy testing with golden datasets, the thread-driven improvement loop, and scaling one…

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Operating Question | Assess our ontology's operational health: (1) When was each top-level folder last modified? (2) How many files have no clear owner or no explanatory note? (3) Are there pending review patches older than a week? (4) Which governed metrics have no test pinning their expected value? (5) Which folders are open to everyone vs restricted? |
| 1 | Git-Backed Operations | *(Ana orients you)* — explain **Git-Backed Operations** in 3 bullets, grounded in my connected data, then move on. |
| 2 | Access Control at Scale | What do you know about [a metric in a folder this role shouldn't see]? |
| 3 | Accuracy Testing | From recent threads, identify the 10 most-asked governed metrics. For each, compute the value for [a fixed, closed period — e.g., last full quarter] through the ontology surface, and record: the question, the exact parameters, the expected value, and the .tql file it routes through. Format as a golden-dataset table I can save. |
| 4 | The Improvement Loop | Review threads from the last 30 days for moments where a user corrected a definition, restated business logic, or re-explained a term mid-conversation. List the correction, how often it recurs across threads and users, and whether the ontology currently covers it. Rank by recurrence. |
| 5 | Scaling Across Teams | Scan the team folders for definitions that duplicate or near-duplicate each other, or that threads from other teams keep referencing. Recommend which should be promoted to shared/, and flag any whose duplicates have drifted apart — those need reconciliation, not just promotion. |
| 6 | The Runbook | Create runbook.md at the ontology root using this outline. Pre-fill everything you can observe — folder owners from file history, the access map from folder settings, the golden suite playbook and its schedule — and mark the rest [TODO]. Open it as a patch for review. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
