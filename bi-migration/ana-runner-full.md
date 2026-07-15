# Bi Migration — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Bi Migration" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/bi-migration/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · Inventory the Estate

### 0.1 · Export the estate
From your BI tool's admin surface, export three things as files:

### 0.2 · The zombie analysis
Upload the content list and usage stats, then:

**Prompt for the learner to run:**
```
From these files: how many dashboards exist in total, and how many had zero views in the last 90 days? Of the rest, what share of total views does the top 10% of dashboards get? List the top 20 by usage, and separately the zombie list — no views in 90+ days — with owners.
```

> ✅ You'll see: the number most BI estates don't want to know: typically a large majority of dashboards are unopened, with usage concentrated in a small head. This is the political unlock of the whole migration — you are not migrating 400 dashboards; you're migrating the 25 that get used, and the zombie list retires with no migration at all.

### 0.3 · Map the head

**Prompt for the learner to run:**
```
For the top 20 dashboards by usage: group them by the underlying question each answers (e.g., "revenue pacing," "pipeline health," "ops backlog"). Where multiple dashboards answer the same question differently, flag them as a consolidation group.
```

> ✅ You'll see: the estate reframed as a question list — usually far shorter than the dashboard list, with consolidation groups where three teams built three versions of the same view. Questions, not artifacts, are what you'll migrate.

### 0.4 · Extract the semantic inventory
Upload the semantic-layer export:

**Prompt for the learner to run:**
```
From these definitions [LookML models / workbook calculations / measure definitions]: list every metric with its formula in plain English, the dimensions it's computed over, and any filters baked in. Group near-duplicates — metrics with the same name but different formulas, or different names with the same formula.
```

> ✅ You'll see: your semantic layer as a readable catalog — including the near-duplicate groups that will become Module 1's reconciliation work. Finding "active_customers defined three ways" here , in inventory, is far cheaper than finding it in parity testing.

**Checkpoint before moving on:**
- [ ] The zombie share is quantified and the retire-without-migrating list exists, with owners
- [ ] The top-20 head is mapped to its underlying questions, consolidation groups flagged
- [ ] The semantic catalog exists with near-duplicates grouped
- [ ] You can state the migration scope as a question count, not a dashboard count

## Module 1 · Translate the Semantic Layer

### 1.1 · Why the semantic layer migrates first
Dashboards are views; the semantic layer is meaning . Migrating meaning first ensures every rebuilt dashboard (Module 2) and every conversational answer computes from the same governed definitions — instead of re-encoding each dashboard's private math. In TextQL, that meaning lives in the Ontology : your org's semantic layer of metric definitions, entity relationships, and business logic.

### 1.2 · Propose the governed metrics
With Module 0's semantic catalog in the thread:

**Prompt for the learner to run:**
```
From this catalog, propose governed ontology metrics for the [15] metrics that power the top-20 dashboards. For each: the plain-English definition, the source tables and join logic in our warehouse, the dimensions it should support, and the original [LookML/calc/measure] it translates. Where the original embeds tool-specific behavior (a default date filter, a tool-side aggregation quirk), call it out explicitly rather than silently translating it.
```

> ✅ You'll see: a proposed metric set with translation notes. The tool-specific-behavior callouts matter most — a Looker default filter or a Tableau context-filter interaction that silently shaped a number for years needs a decision (keep the behavior? drop it?), not a silent port.

### 1.3 · Reconcile the conflicts
Now the near-duplicate groups from Module 0.4:

**Prompt for the learner to run:**
```
For each conflicting definition group: show the formulas side by side, compute both versions against [last full quarter] so we see the actual numeric gap, and recommend which should become the governed definition. Record the decision rationale and keep the alternative as a clearly-named variant (e.g., active_customers_including_trials) rather than deleting it.
```

> ✅ You'll see: conflicts quantified (the gap is often material — that's why teams argued) and resolved into one governed definition plus named variants. Frame this as the migration's dividend, not its cost: these conflicts existed all along, baked into different dashboards; the migration is simply the first time anyone reconciled them.

### 1.4 · Commit through review
Propose the metric set as ontology patches for review — definitions this load-bearing go through the approval flow, with the metric's business owner (not just the engineer) signing off on each contested decision. The deeper build mechanics — .tql surfaces, folder structure, git sync — live in Build Your Ontology, End to End and Ontology Operations ; this module's job is getting the decisions made and recorded.

**Prompt for the learner to run:**
```
Propose the governed metric set from this thread as ontology patches for review — one patch per metric, each with the definition, source logic, translation notes, and the recorded rationale for any contested decision. Route the contested ones to [the metric's business owner] for review.
```

> ✅ You'll see: the patches created and queued for approval — the decisions recorded where the review flow (and future audits) can find them.

**Checkpoint before moving on:**
- [ ] The top metrics are proposed as governed definitions with source logic and translation notes
- [ ] Every tool-specific behavior got an explicit keep/drop decision
- [ ] Each conflict group has a quantified gap, a governed winner, a named variant, and a recorded rationale
- [ ] Patches are in review with business owners on the contested ones

## Module 2 · Rebuild the Flagship

### 2.1 · Pick the flagship
From Module 0's usage ranking, take the #1 dashboard — the one whose users will notice everything. Rebuilding the hardest audience's view first is deliberate: if parity convinces them , the rest of the migration is downhill.

### 2.2 · Rebuild from the artifact
Attach a screenshot (or PDF export) of the original:

**Prompt for the learner to run:**
```
Rebuild this dashboard connected to our warehouse: match the layout, panels, and chart types; compute every metric through the governed ontology definitions we created (not ad-hoc SQL); add the same filters. List any panel you couldn't map to our data or definitions rather than improvising it.
```

> ✅ You'll see: a working rebuild with an honest unmappable list (that list feeds Module 4). The governed-definitions requirement is the point — the rebuilt dashboard inherits the reconciled semantic layer, so it can never drift from the official numbers the way the original's private calcs could. (Dashboard craft — publishing, refresh schedules, versioning — lives in the Dashboards & Reporting workshop.)

### 2.3 · Side-by-side discipline
Open the original and the rebuild together, same date ranges, same filter selections:

**Prompt for the learner to run:**
```
For each panel in the rebuild, I'm comparing against the original with identical filters: [date range, segment]. Recompute each panel's headline figure and show it with its definition, so I can tick them off one by one.
```

> ✅ You'll see: panel-by-panel figures for systematic comparison. Don't eyeball charts — compare numbers , panel by panel, and log every discrepancy with its filter state. The discrepancy log is Module 3's input, and the discipline here is what makes the parity claim credible later.

### 2.4 · Resist improving it (yet)
The rebuild will tempt you to fix the original's sins — the cluttered fourth row, the filter nobody uses. Don't, yet. Parity first, improvements after sign-off: a rebuild that's different can't be verified . Park improvements in a list; ship them as v2 the week after cutover, when they read as momentum instead of discrepancy.

**Checkpoint before moving on:**
- [ ] The flagship is rebuilt on governed definitions, with an honest unmappable list
- [ ] Every panel compared numerically against the original at identical filter states
- [ ] All discrepancies logged with filter context for Module 3
- [ ] The improvements list is parked, not shipped

## Module 3 · Parity Testing

### 3.1 · Pin the golden values
From the old tool, capture the numbers people actually trust — the flagship's headline figures for closed periods (a closed quarter's value never moves; "this month" never stops moving):

**Prompt for the learner to run:**
```
Create a golden parity table: for each of these [12] metric/period pairs from the old tool [paste them: metric, filters, period, value], compute the same figure through our governed definitions and record old value, new value, and the difference. Closed periods only.
```

> ✅ You'll see: the parity table — your migration's evidence exhibit. Store it in the ontology (a migration/parity/ note) so the proof is versioned alongside the definitions it validates.

### 3.2 · Triage the mismatches
Every mismatch has exactly one of three causes. Work them in this order:

**Prompt for the learner to run:**
```
For each mismatch in the parity table: decompose the difference — does it close if we align time-window conventions? Does it match one of our recorded definition variants? If neither, compare the row populations between the two computations and show me what's in one but not the other.
```

> ✅ You'll see: every gap attributed to its cause. The population comparison (what rows are in one total but not the other) is the workhorse move — gaps stop being mysterious when you can see the actual rows.

### 3.3 · When the old number is the wrong one
Some mismatches will resolve uncomfortably: the old tool's trusted number was computed on a stale extract, a silently-broken filter, or a definition nobody would defend today. This happens in most migrations. The protocol:

### 3.4 · Keep the suite running
The parity table is also your post-cutover drift alarm: schedule it as a recurring check (the golden-suite playbook pattern — built out fully in the Data Quality & Validation Deep-Dive , with the ongoing operating discipline in Ontology Operations ). Parity isn't a one-time gate; it's a standing guarantee.

**Prompt for the learner to run:**
```
Create a playbook "Parity Suite" that re-runs the golden parity table [weekly] and posts the results to [#migration-channel], flagging any metric whose old-vs-new difference exceeds [0.5%]. Closed periods only, same as the original table.
```

> ✅ You'll see: the parity suite scheduled — drift now announces itself instead of waiting to be discovered.

**Checkpoint before moving on:**
- [ ] The golden parity table exists, closed periods only, stored in the ontology
- [ ] Every mismatch is attributed: time-window, definition, or data — none left "unexplained"
- [ ] Any wrong-old-number cases are documented and routed to business owners before cutover
- [ ] The parity suite is scheduled to keep running after cutover

## Module 4 · What Doesn't Migrate

### 4.1 · The honest table

### 4.2 · What replaces dashboards entirely
The deeper reframe: a dashboard is a pre-answered question — built because asking used to be expensive. When asking costs nothing, several dashboard genres stop earning their maintenance:

### 4.3 · Classify your own head

**Prompt for the learner to run:**
```
Take our top-20 dashboard list with its question mapping. Classify each: (a) rebuild as a live dashboard — genuinely monitored regularly; (b) replace with a scheduled playbook — it's really a recurring report; (c) replace with a feed agent — it's really a watch-for-changes; (d) retire to on-demand asking — it was an in-case-someone-asks. Recommend with one line of reasoning each.
```

> ✅ You'll see: your migration plan compressed — usually far fewer rebuilds than the dashboard count implied, which is also your effort estimate and your license-bill story.

**Checkpoint before moving on:**
- [ ] The honest table is shared with stakeholders before they find the gaps themselves
- [ ] Your unmappable list from Module 2 is classified: equivalent exists / keep in old tool / genuinely gone
- [ ] The top-20 are classified into rebuild / playbook / agent / on-demand
- [ ] You can explain "migrate questions, not artifacts" with your own numbers

## Module 5 · Coexistence & Cutover

### 5.1 · Coexistence is a phase, not a failure
The realistic migration shape: a months-long overlap where the old tool keeps serving what it serves well (the pixel-perfect holdouts from Module 4, the long tail not yet classified) while TextQL takes the head. Make the phase deliberate — with an owner, entry criteria (parity proven, Module 3), and exit criteria (usage shifted, Module 6) — rather than an indefinite drift.

### 5.2 · Connect the old tool as a data source
During coexistence, connect Tableau or Power BI to TextQL as a connector (setup in Connect Your Data , Module 4 — including the collection/workspace sync step people miss). This does two jobs:

**Prompt for the learner to run:**
```
Using the [Tableau/Power BI] connector: what dashboards and views can you see? For [a not-yet-migrated dashboard], summarize what it shows and what question it answers.
```

> ✅ You'll see: the old estate visible from inside the new front door — coexistence as a feature rather than a compromise.

### 5.3 · Usage-based deprecation
Retire on evidence, not on opinion:

**Prompt for the learner to run:**
```
From the latest usage export: which migrated-and-rebuilt dashboards still show old-tool usage above [20 views/week]? Those are my stuck users — list the dashboards and their heaviest remaining users so I can find out what the rebuild is missing.
```

> ✅ You'll see: the stuck-user list — your best source of truth about what the rebuilds lack. Talk to them before forcing the cutover; they know something the parity table doesn't.

### 5.4 · The cutover decision
Per dashboard, cut over when: parity proven (Module 3) and old-tool usage collapsed (5.3) and the owner signs off. Per estate , the license-renewal date is usually the forcing function — work back from it: the head rebuilt and verified two months before renewal, coexistence holdouts (pixel-perfect reports) explicitly budgeted, everything else retired.

**Checkpoint before moving on:**
- [ ] Coexistence has an owner, entry criteria, and exit criteria — it's a phase, not a drift
- [ ] The old BI tool is connected as a data source and answering questions about unmigrated dashboards
- [ ] The zombie list is archived with a grace period; deprecation is usage-triggered, not opinion-triggered
- [ ] The stuck-user list exists and someone is talking to them

## Module 6 · Measure Adoption

### 6.1 · Define the adoption signals
A migration succeeds when behavior moves. The measurable signals:

### 6.2 · The adoption playbook

**Prompt for the learner to run:**
```
Create a playbook "Migration Adoption Weekly" that runs Mondays at 8am: (1) TextQL-side — threads created, distinct and repeat users, shares, rebuilt-dashboard views, each vs the prior week; (2) old-tool side from the latest usage export — total views and views on migrated dashboards, trending; (3) the crossover chart — old-tool views vs TextQL activity on one timeline; (4) flags — any migrated dashboard whose old-tool usage ISN'T declining, and any week where new-tool activity drops. Deliver to [#migration-channel].
```

> ✅ You'll see: the migration's scoreboard, weekly, where the team works. The crossover chart is the artifact for leadership; the not-declining flag is the working signal — it feeds Module 5's stuck-user conversation.

### 6.3 · Read it honestly
Two failure smells to watch for in the weekly numbers:

**Checkpoint before moving on:**
- [ ] The adoption playbook runs weekly with both sides of the shift plus the crossover chart
- [ ] The not-declining flag feeds an actual conversation with stuck users
- [ ] A plateau has a planned response (enablement) distinct from the stuck-dashboard response (fix or reclassify)
- [ ] Leadership sees the crossover chart without asking for it

