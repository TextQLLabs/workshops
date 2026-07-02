# Ontology Operations — Ana-Led Runner (v2: gated)

> A **gated** runner (v2). This workshop has steps you do in the **console/CLI**, so Ana inspects your
> workspace state, **gates on each prerequisite**, hands you the setup action and **waits**, then re-checks
> — and resumes if you step away. Pairs with `../ana-workshop-facilitator.md`. Delivery: inline / URL.
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this)

```
Hey Ana — facilitate the "Ontology Operations" workshop with me on this workspace. It has steps I do in the
console/CLI, so: first inspect the current state (connectors / roles / context / git, as relevant) and
tell me where we're starting. Then go ONE step at a time — for analysis steps, hand me the prompt to
run; for console/setup steps, tell me exactly what to do, then WAIT until I say "done" and re-check
before moving on (don't assume a setup step is done). If I leave and come back, re-inspect and resume.
Start by telling me what you see.
```

## Methodology (what this teaches)

The follow-on to Build Your Ontology, End to End: you've built a governed semantic layer — this workshop is about running it in production. Git sync and contribution workflows, folder-level access control, accuracy testing with golden datasets, the thread-driven improvement loop, and scaling one…

## Instructions to Ana (the v2 HOW)

1. **Inspect current state first** — connectors / roles / context / git, as relevant; tell me where we start.
2. **Gate on each prerequisite.** For a console/setup step, hand me the exact action and **WAIT**; re-check on return; **never fake completion**.
3. **One step at a time.** Analysis steps → hand me the prompt + what to look for. Setup steps → the console action, then wait.
4. **Persist progress** so a new thread resumes where we left off.
5. **Adapt to my actual workspace; never fabricate.**

## Modules — Ana gates on setup steps, hands analysis prompts to the learner

| # | Module | Step — Ana hands you the prompt, or the console action |
|---|---|---|
| 0 | The Operating Question | Assess our ontology's operational health: (1) When was each top-level folder last modified? (2) How many files have no clear owner or no explanatory note? (3) Are there pending review patches older than a week? (4) Which governed metrics have no test pinning their expected value? (5) Which folders are open to everyone vs restricted? |
| 1 | Git-Backed Operations | *(Ana orients you)* — explain **Git-Backed Operations** in 3 bullets, grounded in my connected data, then move on. |
| 2 | Access Control at Scale | What do you know about [a metric in a folder this role shouldn't see]? |
| 3 | Accuracy Testing | From recent threads, identify the 10 most-asked governed metrics. For each, compute the value for [a fixed, closed period — e.g., last full quarter] through the ontology surface, and record: the question, the exact parameters, the expected value, and the .tql file it routes through. Format as a golden-dataset table I can save. |
| 4 | The Improvement Loop | Review threads from the last 30 days for moments where a user corrected a definition, restated business logic, or re-explained a term mid-conversation. List the correction, how often it recurs across threads and users, and whether the ontology currently covers it. Rank by recurrence. |
| 5 | Scaling Across Teams | Scan the team folders for definitions that duplicate or near-duplicate each other, or that threads from other teams keep referencing. Recommend which should be promoted to shared/, and flag any whose duplicates have drifted apart — those need reconciliation, not just promotion. |
| 6 | The Runbook | Create runbook.md at the ontology root using this outline. Pre-fill everything you can observe — folder owners from file history, the access map from folder settings, the golden suite playbook and its schedule — and mark the rest [TODO]. Open it as a patch for review. |

## When done

Recap in 3 bullets. Offer to save anything built as a **Playbook**. Do **not** write the workshop into the governed ontology.
