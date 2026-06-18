# Data Quality — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Data Quality workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Data Quality" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/data-quality/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A workshop of short, hands-on modules for data engineers and analytics engineers who need to trust — and prove — answer accuracy. The thesis throughout: accuracy is checked, not asserted. Golden datasets pinned to numbers the org already trusts, eval cases that exercise edge conditions, drift…

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Trust Stack | For our [5] most-quoted governed metrics: which have any automated check pinning their value? For each unchecked one, tell me which failure modes above could currently change its value without anyone being alerted. |
| 1 | Golden Datasets | Our audited [revenue] for [last closed quarter] was [exact value] per [the income statement]. Compute the same figure through our governed [revenue] definition — same period, same entity scope. Show the result, the difference if any, and the full derivation: source tables, filters, the rendered query. |
| 2 | Eval Cases | From our governed definitions in the ontology, generate candidate eval cases: for each definition, propose edge cases across empty periods, time boundaries, timezone handling, null semantics, and contrasts with its named variants. For each candidate: the question, why this edge matters for this definition, and how to establish the expected answer. |
| 3 | Drift Detection | Create a playbook "Golden Suite" that runs [weekly, Sunday 11pm]: execute every golden query and eval case under golden/, compare to expected values/behaviors, and report. All pass: one line, nothing more. Any failure: lead with the failing item, expected vs actual, the divergence, the responsible definition file, and the three most likely causes ranked. Deliver to [#data-quality]. |
| 4 | Reconciliation | [System A] reports [X] for [metric, period]; [System B] reports [Y] — a gap of [Z]. Decompose along four axes, in order: (1) Population — pull the entity/row sets from both sides and diff them; what's in one but not the other, and how much of the gap does that explain? (2) Filter — apply each side's inclusion rules to the common population; remaining gap? (3) Timing — align time-windows and conventions; remaining gap? (4) Definition — for the now-identical population, filters, and window, show both formulas side by side on the same rows. Attribute the full gap across the four axes. |
| 5 | The DQ Agent | Create a feed agent "Data Quality Watch" over [the core tables]: check freshness vs expected load windows, row counts vs trailing baselines, null rates on [key columns], and day-over-day discontinuities in [key aggregates]. Post ONLY on genuine issues — one post per incident with the table, symptom, evidence, and likely upstream cause. A late load (<[2h]) is "late," not "failed"; stay silent on all-clear days. |
| 6 | The Quality Runbook | Create golden/RUNBOOK.md in the ontology from this template: fill in the owners, anchors, and cadences you can observe from golden/, the Golden Suite playbook, and the Data Quality Watch agent; mark everything else [TODO]. Open it for review. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
