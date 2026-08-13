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

