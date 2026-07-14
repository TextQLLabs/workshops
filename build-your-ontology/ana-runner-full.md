# Build Your Ontology — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Build Your Ontology" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/build-your-ontology/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · Concepts & The Method
*🎯 Goal: know what an ontology is here, the principles that make it work, and the field + research case for it*

> **The cardinal rule** — Do not hand a skeptic a form to fill out. Templates, question libraries, and "North Star archetypes" exist to signal expertise (you've solved this in their domain) and to force the right conversation — not as a schema to deploy. Naming domain-specific failure modes ("here's where naive implementations break") earns your opinions. Then bias hard toward action : the answer to "where do we start?" is "let me show you." Establish the North Star → feed existing context → start asking real questions → review & ratify what Ana proposes. The model improves every cycle.

**Prompt for the learner to run:**
```
Here are assets that already encode how our data joins and what our metrics mean: [attach or paste: ERD, canonical SQL queries, dbt models, join documentation]. Seed the ontology from these directly - sources, join paths, and metric definitions - instead of rediscovering them by exploration. Show me everything you extracted as a reviewable change, and flag anything ambiguous or conflicting rather than guessing.
```

> ✅ You'll see: Ana turn your existing SQL/ERD knowledge into governed ontology entries in one pass — discovery reserved for what your assets don't cover.

> **Two rules for a long, live session** — 1 · Checkpoint every couple of modules. Long threads have a ceiling. After every module or two, ask Ana: “Save a handoff document summarizing what we've built, what we decided, and what's next — so we can continue in a new thread.” If a thread ever maxes out, you lose nothing — open a new thread, paste the handoff, keep going. 2 · Pin the scope in every prompt. Name the entity you're analyzing and the source-of-truth tables in each prompt (“…for [entity X], using the [base audit] tables, not the summary table”). Without that, Ana may drift to a convenient summary table or query every source at once — and a plausible-looking answer from the wrong table is worse than no answer.

**Checkpoint before moving on:**
- [ ] You can explain "just files" and why git-backed matters
- [ ] You know the six folders of the repo shape and what goes in each
- [ ] You know where a metrics doc vs. a call transcript lands in the model
- [ ] You can pitch a skeptic: prior work = corpus, Ana writes it, North Star first, never a form
- [ ] You can cite the research: the "amnesia tax" and the ≥30% token reduction from a malleable layer

## Module 1 · The Before/After Baseline
*🎯 Goal: capture a "cold" answer now, so the "warm" answer at the end has something to beat*

**Prompt for the learner to run:**
```
[A hard, definition-sensitive question from your domain — e.g. "What's our 30-day readmission rate for the last complete quarter?" or "What was net revenue retention last quarter?"] Note which definition you used and show the SQL.
```

> ✅ You'll see: a best-effort answer with an improvised definition. Write down four things: the number, the assumed definition, how many steps Ana took , and the time it took. That's your baseline — the step count is the headline .

> **At the end of each track** — Ask the same question in a fresh session with your ontology attached and compare — especially the step count . A cold run that took 16 steps typically collapses to 2 ( read the ontology → execute ). That single number is the clearest proof of what the ontology buys you.

**Checkpoint before moving on:**
- [ ] You recorded a cold answer: number, assumed definition, and time taken
- [ ] You logged the step count — you'll compare it warm at the end of the track

## Module 2 · Track A: Discover & Draft
*🎯 Goal: start from what's actually there — and let Ana draft the documentation you don't have*

**Prompt for the learner to run:**
```
Connect to the data source and pull the information schema first. List the tables and key columns, and map how the core entities relate (the join keys). Don't run heavy scans yet.
```

> ✅ You'll see: Ana return a schema map — tables, keys, relationships — without guessing.

**Prompt for the learner to run:**
```
Profile a couple of the core tables (sampled — no full scans) and draft a data dictionary, a short business glossary, and a list of candidate metrics worth governing. For each metric, flag any definitional ambiguity a human should resolve.
```

> ✅ You'll see: drafted docs and a candidate-metric list — often surfacing a definition that needs a decision (a setup for Module 4's reconciliation).

**Checkpoint before moving on:**
- [ ] Ana mapped the schema — tables, keys, relationships — without heavy scans
- [ ] You have a drafted data dictionary, glossary, and candidate-metric list
- [ ] At least one metric was flagged as definitionally ambiguous

## Module 3 · Track A: First Governed Metric
*🎯 Goal: a first .tql query surface, rendered to inspectable SQL, saved durably to the ontology*

**Prompt for the learner to run:**
```
Using the schema and the docs we just drafted, propose the ontology: the core entities, the metrics to govern, and the dimensions. Draft the first query-surface .tql, give every metric a stable alias, and render the SQL before executing.
```

> ✅ You'll see: generated .tql and the exact, inspectable SQL. Run it to get the answer.

**Prompt for the learner to run:**
```
Take the query surface you just drafted and make it reusable rather than one-off: expose the metrics, dimensions, filters, and time window as typed parameters with approved options (label sets the caller picks from - the ontology owns the SQL). Document what each parameter expects in comments directly above it. If the entities or joins would be reused by other questions, split them into a reusable object module the surface imports. Show me the before/after.
```

> ✅ You'll see: the same metric, but now callers pick metrics/dimensions/filters from approved options instead of triggering a rewrite — one governed surface serving many questions. Joins live once, in a reusable module.

> **Why this matters** — This is also the durable answer to "how do I stop Ana rediscovering our join paths": model joins as reusable fragments once, governed, and every later question routes through them. Reusable parameters are what make Module 5's "update without rebuilding" cheap.

**Prompt for the learner to run:**
```
Save the docs, the metric definitions, and the .tql surfaces into the ontology so they persist and are reusable across sessions.
```

> ✅ You'll see: the ontology files written to the repo — the basis for everything that follows.

**Prompt for the learner to run:**
```
Add a short routing README to the ontology folder you just wrote: what lives here, when to use it, and which files are canonical. Use searchable filenames and headings, and deliberately include the phrases people will actually ask for (the metric names, their synonyms, the team and role names). Link it from the ontology's top-level entry point so the path is broad to narrow: entry point -> overview -> this folder -> the file.
```

> ✅ You'll see: a small README whose prose repeats the likely search terms on purpose — exact-match search is often the fastest retrieval path for a future agent.

**Checkpoint before moving on:**
- [ ] You read the rendered SQL before executing it
- [ ] Every metric got a stable alias
- [ ] The docs and .tql surfaces are saved to the ontology repo
- [ ] Your surface exposes typed parameters (metrics / dimensions / filters / window) — not one hard-coded answer
- [ ] A routing README exists, with the search terms a future chat would use

## Module 4 · Track A: Explore & Reconcile
*🎯 Goal: see the model (not just the answer) — then turn "defined three ways" into one governed definition*

**Prompt for the learner to run:**
```
Two teams define this metric differently (for example, a stricter vs. broader window, or a prorated vs. full-count denominator). Inspect the data, recommend one governed definition, author it as a .tql surface, and record the decision — and the rejected alternative — in a notes file. Keep the alternative available under a separate, clearly-named metric.
```

> ✅ You'll see: Ana identify the discrepancy, propose one definition, and produce a reviewable change. The rejected alternative stays available — renamed, not erased.

> **When two SOURCES disagree — not just two definitions** — Sometimes the conflict is upstream: a summarized table and the base tables tell different stories about the same question. Don't let Ana pick on convenience — run the question against both, compare the results side by side, and let the SME who owns the data rule which is the source of truth. Then record the ruling — and the caveat about the losing source — in a notes file so it never gets re-litigated, and so Ana routes to the right table next time.

> **Why this matters** — Every organization has a metric defined two ways. Governing it into one definition — with the decision recorded — is the moment the ontology stops being documentation and starts being the source of truth.

**Checkpoint before moving on:**
- [ ] You walked entities → metrics → lineage in the ontology graph
- [ ] A conflicting metric got one governed definition, with the decision recorded in notes/
- [ ] The rejected alternative survives under a clearly-named separate metric

## Module 5 · Track A: Update Without Rebuilding
*🎯 Goal: prove the model is cheap to evolve and still governed*

**Prompt for the learner to run:**
```
Update the ontology: add a new variant of an existing metric and break a metric out by a new dimension. Then handle a schema change (a renamed column). Show me the diff before applying, and open the changes for review.
```

> ✅ You'll see: small, targeted edits and a clean diff — not a rebuild. The schema rename touches schema.tql once; everything downstream follows.

**Prompt for the learner to run:**
```
[Your Module 1 baseline question, exactly as you asked it cold.] Note which definition you used and show the SQL.
```

> ✅ You'll see: the warm answer route through your governed definition — consistent, with the exact audited SQL — instead of an improvised one.

> **Make the ontology learn from usage — not just from events** — Schema renames and new metrics are event -driven updates. The other half is usage-driven : a recurring playbook that mines real activity — repeated questions, manual SQL, failed ontology lookups, mid-thread corrections — and proposes small reviewable patches. Ana drafts; a named owner approves; the learning is routed back into the routing READMEs; unresolved gaps become work items, not buried chat history. Build it in the Automation workshop , and see Ontology Operations Module 4 for the full operating loop.

**Checkpoint before moving on:**
- [ ] You saw the diff before it was applied, and the change went to review
- [ ] A renamed column was handled by one edit in schema.tql
- [ ] Your baseline question's warm answer beat the cold one — and you can say why

## Module 6 · Track B: Build from Documents
*🎯 Goal: fuse messy, mixed-format inputs into one coherent, governed model*

**Prompt for the learner to run:**
```
Read these documents — SOPs, a metrics document, data dictionaries, process-flow diagrams, and call transcripts. Propose an ontology: the core entities, the metrics worth governing, and the dimensions. Explain where each input lands — which became an entity, a metric, a note, or an access rule — then draft the files.
```

> ✅ You'll see: Ana fuse the mixed inputs into one coherent model, narrating where each piece went — entity, metric, note, or access rule.

> **The ontology is more than text and .tql** — Anything that helps a future agent or human understand, generate, or verify work belongs in the repo: entity/lineage diagrams , stylesheets for branded report output, spreadsheet or slide generators for recurring deliverables — with example outputs stored next to the generator and searchable filenames ( finance-revenue-lineage.png , not diagram-final.png ).

**Checkpoint before moving on:**
- [ ] Your input pile had at least 4 input types (docs, metrics, diagrams, transcripts…)
- [ ] Ana narrated where each input landed in the model
- [ ] The drafted files form one model — not one model per document

## Module 7 · Track B: Validate & Govern
*🎯 Goal: accuracy checked (not asserted), and governance expressed in the model itself*

**Prompt for the learner to run:**
```
Use these golden datasets and validation cases to check the metrics. Where a metric is ambiguous, reconcile it to the governed definition and add a golden-query test that pins the expected value.
```

> ✅ You'll see: accuracy checked against known-correct outputs — and a pinned test that will alert on drift.

**Prompt for the learner to run:**
```
Apply these access policies to the ontology: restrict sensitive surfaces to the right roles (fail closed), apply row-level filters by scope, and limit restricted data to authorized roles. Show the access logic in the .tql.
```

> ✅ You'll see: governance expressed in the model — fail-closed access rules, reviewable like any other file. (Module 9 takes this further with full personas.)

**Checkpoint before moving on:**
- [ ] At least one metric has a golden-query test pinning its expected value
- [ ] Sensitive surfaces fail closed, and you can point to the access logic in the .tql

## Module 8 · Track B: The N+1 Document
*🎯 Goal: answer the question every team asks — "we built from N documents; what happens when one more arrives?"*

**Prompt for the learner to run:**
```
Here's one new document. Figure out where it fits in the existing ontology, make a targeted edit to the right metric/entity/notes — not a rebuild — and open a focused change describing exactly what changed and why. Don't touch unrelated files.
```

> ✅ You'll see: the new document slot in via a small, reviewable change. The model evolves continuously; every change is diff-able and auditable.

**Prompt for the learner to run:**
```
Expose these metrics as a stable surface callable from BI or via API/MCP, and describe an agent that watches for new documents or schema changes and proposes an ontology update automatically.
```

> ✅ You'll see: the ontology become a reusable, self-maintaining layer other tools can build on.

**Prompt for the learner to run:**
```
[Your Module 1 baseline question, exactly as you asked it cold.] Note which definition you used and show the SQL.
```

**Checkpoint before moving on:**
- [ ] Document N+1 produced a targeted edit, not a rebuild — and unrelated files were untouched
- [ ] You know how the metrics get consumed downstream (BI, API/MCP) and watched for staleness

## Module 9 · Role-Based Access
*🎯 Goal: serve multiple teams from one governed model — each with its own data scope and response style*

**Prompt for the learner to run:**
```
[A broad question in your domain — e.g. "How are our patients doing on hospital utilization and cost?"]
```

> ✅ You'll see: Program Manager: Ana asks to clarify with a menu of allowed options only, then answers in plain language — no SQL, summary first. Analyst: Ana states her assumptions, proceeds, shows the SQL with a clause-by-clause explanation, and flags inference vs. governed surface.

**Prompt for the learner to run:**
```
[As the restricted persona, ask for something outside that role's scope — a domain they aren't provisioned for.]
```

> ✅ You'll see: Ana declines and redirects — the data exists and the governed surface exists, but this role isn't scoped to it. The access control lives in the context: auditable, version-controlled, reviewable like any other file.

> **The one rule** — Route, don't redefine. An identity folder's README says "start with ../../domains/finance/revenue " — it never redefines revenue locally. One governed definition, many doors into it. (This is the same principle as the Context Stack's "one EDM, many personas.")

**Checkpoint before moving on:**
- [ ] The same question produced two correctly-different answers across personas
- [ ] An off-scope request was declined and redirected — and you can point to the file that did it

