# Build Your Ontology — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Build Your Ontology workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Build Your Ontology" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/build-your-ontology/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop on turning the context your organization already has — databases, documents, spreadsheets, diagrams, transcripts — into a governed, queryable ontology with Ana, and keeping it current as new information arrives.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | Concepts & The Method | *(Ana orients you)* — explain **Concepts & The Method** in 3 bullets, grounded in my connected data, then move on. |
| 1 | The Before/After Baseline | [A hard, definition-sensitive question from your domain — e.g. "What's our 30-day readmission rate for the last complete quarter?" or "What was net revenue retention last quarter?"] Note which definition you used and show the SQL. |
| 2 | Track A: Discover & Draft | Connect to the data source and pull the information schema first. List the tables and key columns, and map how the core entities relate (the join keys). Don't run heavy scans yet. |
| 3 | Track A: First Governed Metric | Using the schema and the docs we just drafted, propose the ontology: the core entities, the metrics to govern, and the dimensions. Draft the first query-surface .tql, give every metric a stable alias, and render the SQL before executing. |
| 4 | Track A: Explore & Reconcile | Two teams define this metric differently (for example, a stricter vs. broader window, or a prorated vs. full-count denominator). Inspect the data, recommend one governed definition, author it as a .tql surface, and record the decision — and the rejected alternative — in a notes file. Keep the alternative available under a separate, clearly-named metric. |
| 5 | Track A: Update Without Rebuilding | Update the ontology: add a new variant of an existing metric and break a metric out by a new dimension. Then handle a schema change (a renamed column). Show me the diff before applying, and open the changes for review. |
| 6 | Track B: Build from Documents | Read these documents — SOPs, a metrics document, data dictionaries, process-flow diagrams, and call transcripts. Propose an ontology: the core entities, the metrics worth governing, and the dimensions. Explain where each input lands — which became an entity, a metric, a note, or an access rule — then draft the files. |
| 7 | Track B: Validate & Govern | Use these golden datasets and validation cases to check the metrics. Where a metric is ambiguous, reconcile it to the governed definition and add a golden-query test that pins the expected value. |
| 8 | Track B: The N+1 Document | Here's one new document. Figure out where it fits in the existing ontology, make a targeted edit to the right metric/entity/notes — not a rebuild — and open a focused change describing exactly what changed and why. Don't touch unrelated files. |
| 9 | Role-Based Access | [A broad question in your domain — e.g. "How are our patients doing on hospital utilization and cost?"] |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
