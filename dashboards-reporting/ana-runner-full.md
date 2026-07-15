# Dashboards Reporting — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Dashboards Reporting" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/dashboards-reporting/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Output Landscape

### 0.1 · Four ways to ship an answer
Everything you produce with Ana lands in one of four formats. Choosing the right one before you build saves the rework:

**Prompt for the learner to run:**
```
Rule of thumb: if the consumer will ask *follow-up questions of the data*, build a dashboard. If they want *your interpretation*, write a report. If they ask every week, schedule it (playbook for reports — see the Automation workshop; refresh schedule for dashboards — Module 4).
```

### 0.2 · Get ready
Three prerequisites for the hands-on modules:

### 0.3 · Calibrate

**Prompt for the learner to run:**
```
From [your dataset], give me one number worth tracking weekly, one comparison worth showing side by side, and one breakdown worth filtering. For each, say which output format you'd recommend — chart, report, or dashboard — and why.
```

> ✅ You'll see: Ana propose a metric, a comparison, and a dimension — with format recommendations. Keep these three; they become your chart (Module 1), report section (Module 2), and dashboard panels (Module 3).

**Checkpoint before moving on:**
- [ ] You can say when a report beats a dashboard, and vice versa, in one sentence each
- [ ] Dashboards are enabled for your org (or you've asked your admin)
- [ ] You have a familiar dataset picked and the three building blocks from 0.3 saved

## Module 1 · Charts That Communicate

### 1.1 · Baseline chart, then take control

**Prompt for the learner to run:**
```
Show me [your metric from 0.3] by [week/month] for the last 12 months as a chart.
```

> ✅ You'll see: a sensible default. Now redirect it — every property is yours to set:

**Prompt for the learner to run:**
```
Remake that as a [bar/line/area] chart. Sort [descending/by time]. Show only the top [8] categories and group the rest as "Other". Add a target line at [value] and label the periods where we missed it.
```

### 1.2 · The comparison frame
A number means little without its comparison. Make the frame explicit:

**Prompt for the learner to run:**
```
Chart [metric] this quarter vs last quarter side by side, by [dimension]. Make the change visible: add the percentage delta as a label on each pair, and order by the size of the change — biggest mover first.
```

> ✅ You'll see: the chart now answers a question (what moved?) instead of displaying data. This ordering-by-change pattern is the single highest-leverage trick in business charting.

### 1.3 · Distribution, not just averages

**Prompt for the learner to run:**
```
Show the distribution of [value per record — e.g., order size, ticket resolution time] as a histogram. Mark the median and the 90th percentile. Is the average misleading here?
```

> ✅ You'll see: whether your KPI hides a skew — long tails are where averages lie, and a distribution chart is how you catch them before a stakeholder does.

### 1.4 · Small multiples beat busy charts
When one chart gets crowded, split it:

**Prompt for the learner to run:**
```
Instead of one chart with 12 lines, make a grid of small charts — one per [region/product] — each showing [metric] over time with the same y-axis scale, so they're comparable at a glance.
```

> ✅ You'll see: a panel grid. Same-scale small multiples are the honest way to compare many series; mixed scales are how charts mislead.

### 1.5 · Annotate for the absent reader
The chart will travel without you. Bake the story in:

**Prompt for the learner to run:**
```
Annotate that chart for someone with no context: title that states the takeaway (not the metric name), a marker on [the event — launch, price change, outage], and a one-sentence caption summarizing what to conclude.
```

> ✅ You'll see: a self-explanatory chart. Titles that state takeaways ("Churn spike isolated to March cohort") outperform titles that state metrics ("Monthly churn rate").

### 1.6 · Honesty checklist
Before any chart ships, ask Ana to self-audit:

**Prompt for the learner to run:**
```
Review this chart for honesty: does the y-axis start at zero (and if not, is that justified)? Are scales comparable across panels? Is anything excluded that a reader would assume is included? Would a skeptic call this misleading?
```

**Checkpoint before moving on:**
- [ ] You restyled a chart's type, scope, sorting, and annotations through prompts alone
- [ ] You built one comparison chart ordered by size of change
- [ ] You checked a distribution and know whether your average is trustworthy
- [ ] You made one chart that states its takeaway in the title

## Module 2 · Reports: Narrative Outputs

### 2.1 · The report skeleton
Good data reports share one shape. Ask for it explicitly:

**Prompt for the learner to run:**
```
Turn this thread's analysis into a report with this structure: (1) Executive summary — three bullets max, findings not descriptions; (2) The headline number with its comparison; (3) Key charts, each with a one-paragraph takeaway underneath; (4) What we should do — up to three recommendations with owners; (5) Methodology appendix — sources, definitions, filters, time windows.
```

> ✅ You'll see: a structured report. The discipline matters: summaries written as findings ("Enterprise churn doubled; concentrated in March cohort") not descriptions ("This report analyzes churn").

### 2.2 · Formats: PDF and slide deck

**Prompt for the learner to run:**
```
Produce that report as a PDF I can attach to an email. Keep it under [6] pages, charts embedded.
```

**Prompt for the learner to run:**
```
Make a slide version instead: title slide, one slide per key finding (chart + takeaway), one recommendations slide.
```

> ✅ You'll see: downloadable files. Reports are snapshots by design — date-stamp them, and regenerate rather than edit.

### 2.3 · Standalone interactive HTML reports
Between a static PDF and a full dashboard sits a powerful middle ground: a self-contained HTML file with working filters — no TextQL login needed to view it, sendable as an attachment.

**Prompt for the learner to run:**
```
Build this analysis as a standalone interactive HTML report: [metric] by [dimension] with a dropdown filter for [segment] and a date-range toggle. All data embedded in the file, working offline, openable in any browser.
```

> ✅ You'll see: an HTML file that travels anywhere email goes. Use it when your audience can't or won't log in — board members, external partners, exec forwarding chains.

**Prompt for the learner to run:**
```
Know the tradeoff: the data is *frozen at generation time* and embedded in the file. Treat it like a PDF with filters — regenerate to update, and don't embed data the recipient shouldn't keep.
```

### 2.4 · Branding and theming
Reports your org sends repeatedly should look like your org:

### 2.5 · The recurring report decision
If you'll send this same report weekly, don't regenerate by hand. Two paths, covered fully elsewhere — know which fits:

**Checkpoint before moving on:**
- [ ] You produced a report with the five-part skeleton, summary written as findings
- [ ] You exported at least one PDF or deck version
- [ ] You built one standalone interactive HTML report and know its frozen-data tradeoff
- [ ] You know where org-level report theming lives

## Module 3 · Build Your First Dashboard

**Prompt for the learner to run:**
```
Requires Enable Dashboards (Settings → Capabilities; admin, once per org). Dashboards are in Public Preview — interfaces may evolve.
```

### 3.1 · Two ways in

**Prompt for the learner to run:**
```
Build a dashboard for [your domain] with: (1) a KPI row at the top — [your weekly number] with week-over-week delta, plus [2–3 supporting numbers]; (2) [your comparison] as the main chart; (3) [your breakdown] as a table sortable by the main metric; (4) filters for [date range] and [your dimension] that apply to everything.
```

> ✅ You'll see: Ana query your sources, build the visualizations, and assemble a working dashboard — charts, filters, and layout, no BI tool configuration.

### 3.2 · Iterate conversationally
The first version is a draft. Refine it the same way you'd refine any answer:

**Prompt for the learner to run:**
```
Add a tab called "Detail" with the full record-level table, filterable.
Change the main chart to a line chart and add a 4-week moving average.
Make the KPI cards show red/green based on direction vs last period.
Default the date range to the last 90 days.
```

> ✅ You'll see: the dashboard updated in place after each instruction. One change per message beats a paragraph of changes — easier to verify, easier to undo.

### 3.3 · Build from a screenshot
Have a layout you already like — from a slide, an old BI tool, even a sketch?

**Prompt for the learner to run:**
```
Rebuild this dashboard connected to our data: match the layout and chart types, map each panel to the closest equivalent in [your dataset], and tell me which panels you couldn't map.
```

> ✅ You'll see: a working version of the reference layout on your data — including an honest list of unmappable panels. This is the fastest migration path off a legacy dashboard.

### 3.4 · Design for the consumer
Three rules that separate dashboards people use from dashboards people ignore:

**Prompt for the learner to run:**
```
Review this dashboard against three rules: is the most important number top-left? Does every filter map to a question a real viewer would ask? Would a first-time viewer pass the five-second test? Suggest specific changes.
```

### 3.5 · Check the numbers before anyone else sees them
A wrong dashboard is worse than no dashboard:

**Prompt for the learner to run:**
```
Before I publish: recompute the headline KPI directly with a fresh query, independent of the dashboard's data source, and confirm they match. Then list every filter and confirm each actually changes the data shown.
```

> ✅ You'll see: an independent verification pass — the dashboard equivalent of the smoke test from the Connect Your Data workshop.

**Checkpoint before moving on:**
- [ ] You built a dashboard with a KPI row, main chart, sortable table, and working filters
- [ ] You iterated at least three refinements conversationally
- [ ] You ran the design self-audit and the five-second test
- [ ] The headline KPI survived independent recomputation, and every filter demonstrably works

## Module 4 · Publish, Share & Operate

### 4.1 · Publish
Click-path: in the dashboard view → Publish dropdown → Publish Now

### 4.2 · Decide visibility deliberately
Visibility is controlled by the creator: personal dashboards are visible only to you; team dashboards are visible to members with appropriate access. Underneath, dashboard access follows your org's RBAC (dashboard read / write permissions — Admin workshop, Module 2) and your data layer's rules: if the underlying connector has RLS policies, they apply to dashboard queries automatically .

**Prompt for the learner to run:**
```
Two viewers can legitimately see different numbers on the same dashboard if row-level security scopes them differently. That's a feature — but tell your viewers, or you'll get "the dashboard is wrong" tickets that are actually RLS working.
```

### 4.3 · Schedule refreshes
A published dashboard re-executes its queries only when refreshed:

### 4.4 · Edit with versions, roll back without fear
To change a published dashboard:

**Prompt for the learner to run:**
```
Make one small edit to this dashboard: append "(edited)" to the main chart's title.
```

### 4.5 · The owner's habits
A published dashboard is a product with users. The maintenance contract:

**Prompt for the learner to run:**
```
List the dashboards I own, when each was last refreshed and last meaningfully viewed if available, and flag candidates for archiving.
```

> **Store the look, not just the chart** — Anything you'll want again belongs in the ontology as a first-class artifact: the stylesheet that keeps generated reports on-brand, the workbook or slide generator behind a recurring deliverable, an example output stored next to its generator, and a one-line README saying how to regenerate it. Use searchable filenames — finance-revenue-lineage.png , not diagram-final.png — so the next thread finds them by search.

**Checkpoint before moving on:**
- [ ] Your dashboard is published and visible in the right tab (personal vs team — deliberately chosen)
- [ ] A refresh schedule is set that matches the underlying data cadence, and the cadence is stated on the dashboard
- [ ] You made an edit, published a new version, and successfully rolled back via History
- [ ] The dashboard description names an owner, purpose, cadence, and source

## Module 5 · Data Sources & Performance

### 5.1 · What a data source is
Dashboards don't query ad hoc — they're powered by named, reusable data sources that Ana creates and wires up when she builds for you. A data source encapsulates how data is fetched , so the same query or file can feed multiple dashboards without duplication.

**Prompt for the learner to run:**
```
When a governed definition exists, prefer Ontology SQL for headline KPIs. A dashboard that computes "active users" its own way is how two dashboards end up disagreeing in the same meeting — exactly what the ontology exists to prevent.
```

### 5.2 · See what's under yours

**Prompt for the learner to run:**
```
For the dashboard I just built: list its data sources — name, type, and which panels each one feeds. Is anything fetched twice that could be shared?
```

> ✅ You'll see: the wiring diagram. Multiple panels sharing one source is good design; near-duplicate sources doing almost-identical queries are consolidation candidates.

### 5.3 · Push work into the query
The single biggest performance lever: aggregate in SQL, not after . A panel showing monthly totals by region needs maybe 200 aggregated rows — not 2 million raw rows aggregated at render time.

**Prompt for the learner to run:**
```
Review my dashboard's data sources for performance: for each, does the query return pre-aggregated data at the granularity the panel displays, or raw rows? Rewrite any raw-row sources to push the aggregation into SQL — without changing what any panel shows. Confirm the numbers match before and after.
```

> ✅ You'll see: rewritten sources and a before/after check. Two cautions that the rewrite must respect:

### 5.4 · Granularity and the filter contract
For each filter, decide where it's applied:

**Prompt for the learner to run:**
```
The date-range filter should re-query the warehouse on change rather than loading all history up front. The region filter can stay in-memory.
```

### 5.5 · Reuse across dashboards
Because sources are named and centrally managed, your second dashboard should reuse the first one's sources where they overlap:

**Prompt for the learner to run:**
```
I'm building a second dashboard for [adjacent audience]. Which existing data sources can it reuse as-is, which need a variant, and which are genuinely new?
```

> ✅ You'll see: a reuse plan. Teams that treat data sources as a shared library get consistency for free — when the definition of a source improves, every dashboard on it improves together.

**Checkpoint before moving on:**
- [ ] You can name the four data source types and when each applies
- [ ] You listed your dashboard's sources and consolidated any near-duplicates
- [ ] Your sources return pre-aggregated data at panel granularity — with no truncation and filters preserved
- [ ] Headline KPIs ride on Ontology SQL where a governed definition exists

## Module 6 · Troubleshooting Clinic

### 6.1 · "The data is stale"
The most common complaint, with a fixed diagnosis order:

**Prompt for the learner to run:**
```
Query the underlying table directly: what is the max [timestamp column]? Does it match what the dashboard's last-refresh implies it should be?
```

### 6.2 · "The dashboard is blank / broken"

**Prompt for the learner to run:**
```
This dashboard broke after a schema change. Check each data source's query against the current schema, list what no longer resolves, and propose the minimal fix.
```

### 6.3 · "Filters don't work"
Three distinct causes that present identically:

### 6.4 · "Two dashboards disagree"
The trust-killer. The diagnosis is always definitional:

**Prompt for the learner to run:**
```
Dashboard A says [X] for [metric]; dashboard B says [Y] for the same period. Compare their data sources: different tables? different filters (test data, refunds, internal users)? different time-zone handling? different metric definitions? Give me the exact divergence.
```

> ✅ You'll see: the precise fork in logic. The fix is Module 5.1's advice in reverse: migrate both onto one shared source — ideally Ontology SQL — so the disagreement becomes impossible, not just patched. (This is also your data team's cue to govern that metric: ontology workshop.)

### 6.5 · "It's slow"
In order of likely impact: raw-row sources aggregating at render (Module 5.3) → too many panels on one page (split into tabs) → filters re-querying when in-memory would do, or vice versa (Module 5.4) → warehouse-side slowness (queue, warmup — Connect Your Data workshop).

### 6.6 · When it's not the dashboard
Escalate with evidence, not vibes. Capture: dashboard name and version, what you expected vs saw, last-refresh time, whether manual refresh reproduces it, and whether the underlying source queries run correctly in a fresh chat. That package turns a "dashboards are broken" ticket into a five-minute fix — and admins can see org-wide dashboard health in Observability (Admin workshop, Module 5).

### 🎓 You're done
You can direct charts that carry a message, ship reports people read, build and publish dashboards people trust, design the data source layer underneath for speed and reuse, and fix the failure modes yourself.

**Checkpoint before moving on:**
- [ ] You can run the four-step staleness diagnosis in order
- [ ] You know rollback-first for broken edits, and the three filter failure modes
- [ ] You can resolve a two-dashboards-disagree dispute to its exact definitional fork
- [ ] You know what evidence to attach when escalating

