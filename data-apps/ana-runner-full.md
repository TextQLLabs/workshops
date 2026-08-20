# Build Data Apps with Ana — Ana-Led Runner (FULL · BETA)

> Full-instruction runner. **Data Apps is in beta** — confirm it's enabled first.
> Air-gapped/VPC: upload this file. Token-limited tenants: use the concise `ana-runner.md`.

## Step-0 prompt

```
Hey Ana — facilitate the "Build Data Apps with Ana" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/data-apps/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · Dashboards vs Data Apps
*🎯 Goal: know what a data app is, and when it beats a dashboard*

**Prompt for the learner to run:**
```
What can I build from this workspace beyond charts and dashboards? Explain what a data app is here, what it can and can't do in this beta, and show me one example structure based on my connected data.
```

> ✅ You'll see: Ana describe the app surface available in your workspace and sketch what an app over your data could look like — grounded in your actual tables.

**Checkpoint before moving on:**
- [ ] You can say when a data app beats a dashboard in one sentence
- [ ] Ana confirmed Data Apps is enabled in your workspace

## Module 1 · From Analysis to App
*🎯 Goal: a working data app generated from an analysis you already trust*

**Prompt for the learner to run:**
```
Take the analysis in this thread [or: rebuild my analysis of [metric] by [dimension]] and turn it into a data app: the headline number up top, the trend chart, and the breakdown table — with the layout you'd recommend. Show me a preview before saving it.
```

> ✅ You'll see: a generated app preview — real components on your governed numbers, not a mockup. Iterate conversationally ("move the table below the chart", "add a second metric card").

> **Migrating an existing HTML / Streamlit artifact?** — If you already have an HTML report or Streamlit-style artifact from an earlier thread, hand it to Ana: "convert this into a data app, preserving the layout." That's the fastest path from legacy one-off outputs to a governed, shareable app.

> ⚠️ **Building on a large fact table? Coach static-first from the start** — a naive build re-queries the warehouse on every filter click and times out the first time a viewer touches it (Module 6 has the failure story). Have the learner use this variant instead:

**Prompt for the learner to run (large-table variant):**
```
Take the analysis in this thread and turn it into a STATIC-FIRST data app: pre-aggregate the metrics once at refresh into an in-memory snapshot (base cube + breakdown tables + supporting series) and run all filter/sort/tab/chart interaction in the browser against that snapshot — no warehouse query on routine filtering. De-duplicate every dimension before joining so filtered totals can't double-count. Apply filters instantly (no Apply button); show the snapshot timestamp and fall back to the last good snapshot on refresh failure. Use at most one bounded, debounced live query for un-cacheable distinct counts under a custom filter — never a fan-out of full scans. After building, reconcile the default view and one filtered slice to the dollar against a fresh warehouse query, and tell me which dimensions the breakdowns do and don't slice by. Show me a preview before saving.
```

**Checkpoint before moving on:**
- [ ] An app preview rendered from your real analysis
- [ ] You changed the layout conversationally at least once

## Module 2 · Make It Interactive
*🎯 Goal: viewers drive the app — filters, inputs, and scenario controls*

**Prompt for the learner to run:**
```
Add interactivity: a [region/team] filter, a date-range selector defaulting to the last 90 days, and a what-if input for [assumption, e.g. "utilization change %"] that recalculates the projection live. Wire them so every component honors the selections.
```

> ✅ You'll see: controls appear and drive every component. Change an input — the numbers recompute against governed definitions, not a frozen extract.

**Checkpoint before moving on:**
- [ ] At least two viewer controls drive the app
- [ ] A what-if input recalculates a result live

## Module 3 · Governance & Sharing
*🎯 Goal: the app is shared, and every viewer sees only what their role allows*

**Prompt for the learner to run:**
```
Explain how this app behaves for different viewers: what does a user in the [restricted role] see vs an [analyst role]? Which permissions are checked when a viewer interacts? Then share the app with [team/role] and show me exactly what they'll be able to see and do.
```

> ✅ You'll see: the sharing model spelled out — and if your workspace has restricted roles (e.g. a business-user role scoped to governed views), the same app shows each viewer their permitted slice.

> **Verify, don't assume** — Before sharing broadly, test-view the app as a restricted role (or have a colleague in that role open it) and confirm the boundary holds. Governance you've verified is governance you can defend.

> 📌 **TQL-only connections:** if a connection the app reads is marked TQL-only by an admin, the app's live data sources must be governed ontology query files — inline SQL sources are refused there (row-level security lives in the query files). Build on governed surfaces from the start so a later lockdown never breaks the app; if Ana proposes an inline SQL source against a sensitive connection, redirect her to a governed query surface.

**Checkpoint before moving on:**
- [ ] The app is shared to a real audience
- [ ] You verified what a restricted viewer sees

## Module 4 · Operate & Iterate
*🎯 Goal: the app evolves conversationally and its definitions stay governed*

**Prompt for the learner to run:**
```
Update the app: add [a new metric card], change the default date range to [quarter-to-date], and tell me which governed ontology definitions each component uses. If any component computes something ad hoc, propose adding that definition to the ontology so the app and everyone else share one truth.
```

> ✅ You'll see: a targeted update (not a rebuild) and a components-to-definitions map — plus ontology patches proposed for anything ad hoc.

> **Store the app's look in the ontology** — If your org has a style for generated apps and reports, keep the stylesheet and an example in the ontology with searchable names — every future app starts on-brand.

**Checkpoint before moving on:**
- [ ] You changed the live app conversationally
- [ ] Every component maps to a governed definition (or a patch is proposed)


## Module 5 · Duplicate an existing dashboard — .pbit or .twb → data app
*🎯 Goal: rebuild one real Power BI / Tableau dashboard as a governed data app, reconciled to the source*

Two ways in: **(A) source file** — upload the .pbit (data model + DAX measures + layout) or .twb (Tableau XML: datasource fields, calculated fields, layout); **(B) live BI connector** — Power BI/Tableau connected as a data source; Ana reads the published datasets, measures, and metadata directly (best when the file is locked down or the report keeps changing). **Either way, have the learner attach 2–3 screenshots of the original** — the file/connector gives Ana the logic, the screenshots give her the look; this is the single highest-leverage input.

**Prompt for the learner to run (Path A — file):**
```
Here's a [Power BI .pbit / Tableau .twb] dashboard, plus screenshots of how it looks today. Read its data model, its measures, and its layout — and study the screenshots for the look and feel — don't build yet. Then: (1) list every measure and map each to a governed ontology metric, flagging any that don't map cleanly; (2) propose the equivalent data-app layout (cards, trend, breakdown, filters) matching the screenshots; (3) tell me which source tables/joins each metric needs. Show me the plan before you build.
```

**Prompt for the learner to run (Path B — connected source):**
```
Power BI / Tableau is connected as a data source in this workspace. Read the published dataset and its measures for the [dashboard/report name] directly through the connector — and here are screenshots of how it looks today. Same plan: map each measure to a governed ontology metric (flag any that don't map), propose the matching data-app layout from the screenshots, and tell me which underlying tables/joins each metric needs. Show me the plan before you build.
```

> ✅ You'll see: a measure-by-measure mapping (source measure → ontology metric), a layout proposal mirroring the screenshots, and a flagged list of anything needing a definition. Coach: measures with no clean mapping get flagged, never guessed.

**Prompt for the learner to run (reconcile):**
```
Now build it, then reconcile: reproduce the source dashboard's default (unfiltered) headline numbers exactly, and pick one filtered slice and match it to the dollar against a fresh query on the warehouse. Show me both dashboards' numbers side by side and call out any difference and why.
```

> 📌 **Keep the old tool running** — run the app alongside the Power BI/Tableau original until numbers reconcile and the team trusts it. What usually doesn't come across cleanly: visual-level DAX with no governed equivalent, report-only calculated columns, custom visuals, pixel-exact formatting — rebuild the logic and the intent of the layout, flag what can't map. Estate-wide migration is the Migrate from Your BI Tool workshop.

**Checkpoint before moving on:**
- [ ] Ana got the source (file or connected) AND screenshots of the original
- [ ] The app reproduces the source dashboard's headline numbers to the dollar
- [ ] Every source measure maps to a governed metric (or unmapped ones are flagged)
- [ ] The app runs alongside the original for parity testing

## Module 6 · Make it fast — and troubleshoot slow apps
*🎯 Goal: apps that don't time out, and a prop library for diagnosing one that does*

The failure mode is the **scan storm**: one "Apply filters" click fanning out into 15+ full scans of a multi-million-row fact table, nothing cached (filter combos never repeat). The bottleneck is the scans, not the payload — raising a payload limit doesn't help. The fix is **static-first**: (1) pre-aggregate once at refresh into a small snapshot (base cube + breakdown tables + supporting series); (2) every filter/sort/tab/chart interaction runs in the browser against the snapshot — zero warehouse queries on routine use, instant filters, no Apply button; (3) show the snapshot timestamp and serve the last good snapshot if a refresh fails; (4) one bounded, debounced live query only for genuinely un-cacheable detail (distinct counts under a custom filter) — never a fan-out; (5) tune the client side: update charts in place, one optimized pass over the snapshot (~250 ms target on cached interactions).

**Decision rule:** aggregate fits in a shippable snapshot → ship it, filter in-browser (default) · aggregate too big to ship but ≪ raw fact → **pre-aggregated serving table** in the warehouse, app queries that · genuinely on-demand detail → one bounded live query. **Never:** re-scan the raw fact per interaction, parallelize full scans, or treat a payload limit as the fix.

> ⚠️ **Correctness trap:** a dimension with >1 row per key silently doubles every filtered dollar — and the default view often doesn't hit that join, so it looks fine until someone filters. De-duplicate dimensions at the source; always verify a FILTERED slice.

**Prompt for the learner to run (diagnose):**
```
This data app is slow / times out when I apply a filter. Trace exactly what happens on one filter click: how many warehouse queries fire, against which tables, how many rows each scans, and what (if anything) is cached. Tell me whether the bottleneck is the scans or the payload — don't guess, show the query count.
```

**Prompt for the learner to run (convert to static-first):**
```
Re-architect this app static-first: pre-aggregate the metrics once at refresh into an in-memory snapshot (base cube + breakdown tables + supporting series) and run all filter/sort/tab/chart interaction in the browser against it — no warehouse query on routine filtering. Apply filters instantly (drop the Apply button), show the snapshot timestamp, and fall back to the last good snapshot on refresh failure. Use at most one bounded, debounced live query for un-cacheable distinct counts under a custom filter.
```

**Prompt for the learner to run (check double-counting):**
```
Check every dimension this app joins for more than one row per key. For any that fan out, show me how much it inflates a filtered total, and de-duplicate at the source. Confirm a filtered total matches before and after the fix.
```

**Prompt for the learner to run (reconcile to the dollar):**
```
Reconcile this app: reproduce the default (unfiltered) headline numbers exactly, then pick one filtered slice (e.g. one region + one category + a rolling window) and match it to the dollar against a fresh query on the warehouse. Then tell me which dimensions the breakdown tiles do and don't slice by, so I know the tradeoffs.
```

> 📌 **No silent caps** — breakdown tiles slicing a chosen subset of dimensions and the single bounded query for distinct counts are deliberate tradeoffs: say so in the app's notes.

**Checkpoint before moving on:**
- [ ] No warehouse query fires on an ordinary filter (the learner counted)
- [ ] Every dimension verified to one row per key — a filtered total checked before/after
- [ ] Default view AND one filtered slice reconcile to the dollar
- [ ] The app states its tradeoffs (breakdown dimensions, live-count behavior)
