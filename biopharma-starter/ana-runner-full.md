# Biopharma Starter — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Biopharma Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/biopharma-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Six Layers
*🎯 Goal: know what's in the box and where everything lives*

> **Standards alignment** — STANDARDS.md maps the model to the industry standards it aligns with (NDC, RxNorm, ATC, IDMP, OMOP); SOURCES.md cites every source. The semantic layer (metrics, routing, classification) is fully separated from the physical mapping : every physical table name lives in one file , ontology/schema.tql — re-point it and the metric logic stays put. The starter is authored against a generic commercial-analytics model with ANSI/Spark-portable SQL; MIGRATION.md is the re-point checklist and works the same on Redshift, BigQuery, Snowflake, or Databricks (budget: about a half-day with warehouse access). For a deep technical tour, read DEEP_DIVE.md .

> **In scope, by design — and what isn't** — This is biopharma commercial (Rx, sales, field, access, share). RWE (patient outcomes, adherence, line-of-therapy) is a deliberate extension that reuses the sibling healthcare-claims starter's spine and HIPAA governance rather than duplicating it. Clinical-trials / R&D (CDISC), genomics, and medtech telemetry are out of scope — separate future siblings. Same six-layer framework, different spine. See NORTH_STAR.md and notes/glossary.md .

> **Two rules for a long, live session** — 1 · Checkpoint every couple of modules. Long threads have a ceiling. After every module or two, ask Ana: “Save a handoff document summarizing what we've built, what we decided, and what's next — so we can continue in a new thread.” If a thread ever maxes out, you lose nothing. 2 · Pin the scope in every prompt. Name the entity and the source-of-truth tables in each prompt (“…for [entity X], using the [base] tables, not the summary table”) — otherwise Ana may drift to a convenient summary table or query every source at once.

**Checkpoint before moving on:**
- [ ] You can name the six layers and find each one in the repo
- [ ] You know what ships in the box (public classification) vs. comes from your licensed Rx feed
- [ ] You know this kit is commercial-only — RWE reuses the healthcare sibling; R&D/CDISC is out of scope

## Module 1 · Define Your North Star

**Prompt for the learner to run:**
```
Help me define the North Star for our ontology before we build anything.
1. Look at the data connected to this thread and summarize in 3-4 lines what we have: the key tables, the grain, and the domains it covers.
2. Then ask me 5-7 sharp scoping questions to pin down what we are really doing - who will use this, what decision it changes on Monday morning, whether we are after brand-performance parity / market access / field-force effectiveness, what "working" looks like in 30 days, and where our data is messiest. Ask a few at a time, not all at once.
3. From my answers plus the data, recommend the archetype that fits (A brand-performance parity, B market access/payer, or C field-force effectiveness) and draft our North Star: one short paragraph on what this ontology is for, plus the 6-8 questions it must answer in 30 days.
4. Save it as north_star.md in the ontology and propose it as a reviewed change, so every later step builds toward it.
```

> ✅ You will see: Ana summarize your data, ask scoping questions, recommend an archetype with a reason, and draft a north_star.md — a paragraph plus the questions that define "done." Review it, push back, ratify.

> **Why a prompt, not a 50-question checklist** — No giant pre-written question bank that nobody maintains. Ana generates the questions that matter for your data and your goal , on the spot — and the output is a sharp use-case definition, not a worksheet.

**Checkpoint before moving on:**
- [ ] You have a written north_star.md (purpose + the 6-8 questions it must answer)
- [ ] You picked an archetype (A / B / C), or Ana recommended one and you agreed
- [ ] It names something real that changes on Monday morning, not "model all of biopharma commercial"

## Module 2 · Connect Three Things
*🎯 Goal: ontology repo + warehouse + documents connected — then everything else happens in chat*

> **No second source of truth** — You don't copy anything into Ana. She reads the repo live; when the repo changes, Ana sees the change.

> **Licensed-data restrictions first** — Licensed Rx data (IQVIA / Symphony) carries vendor use-restrictions, and prescriber-level data is competitively sensitive. Connect read-only and honor the vendor contract — see ontology/notes/governance-hcp-pii.md and LICENSING.md .

**Prompt for the learner to run:**
```
Read the ontology repo and give me a tour: what entities, metrics, and classification are defined, and what governed questions am I able to ask once my warehouse is validated?
```

> ✅ You'll see: Ana describe the model from the repo itself — proof the Git connector works and the ontology is being read as ground truth.

**Checkpoint before moving on:**
- [ ] Ana described the starter's entities and metrics from the connected repo
- [ ] Your warehouse connector is attached, read-only, and vendor-restriction-aware
- [ ] You know which of your documents you'll bring in (or that you're skipping this)

## Module 3 · Validate Against Your Schema
*🎯 Goal: before trusting numbers, prove the ontology's assumptions match your actual tables — without writing SQL*

**Prompt for the learner to run:**
```
Look at the ontology repo, then inspect my warehouse. Pull the information schema for my commercial tables (product, hcp, rx_fact, sales_fact, call_activity, formulary, …) and tell me where the ontology's expected table and column names don't match what I actually have. Propose the exact changes to ontology/schema.tql.
```

> ✅ You'll see: Ana discover your schema, diff it against the ontology, and hand you a precise list of fixes — table backings, column names. (A ready-made version of this check lives in validation/dry-run-prompt.md .)

**Prompt for the learner to run:**
```
Run validation/validate_tql.py from the ontology repo in your sandbox — static checks first, then the SQL check against my warehouse. Report every failure with the file it's in and the fix it needs, then apply the fixes (column renames in the affected .tql, table renames in schema.tql only) and re-run until clean.
```

> ✅ You'll see: the typo'd-backing, wrong-alias, and missing-column bug classes caught here — instead of surfacing as wrong numbers in front of stakeholders.

> **Prefer the terminal?** — The same gate runs locally: python3 validation/validate_tql.py (static — no warehouse needed) · --check-sql (paste the output into Ana: rows = missing columns) · --dsn "<dsn>" --explain (live column check + compile test).

**Prompt for the learner to run:**
```
Make those changes and open a pull request.
```

> ✅ You'll see: Ana edit the files and open a reviewable PR in your repo. Every physical table name lives in one place ( ontology/schema.tql ) — re-point it and the metric logic stays put. The join keys are product_id / hcp_id / territory_id ; re-point the backings and the surfaces follow.

> **Why this step matters** — Every customer's warehouse differs from the reference shape somewhere — a renamed column, a missing table, a different grain. Finding those before you trust a number is the difference between a defensible metric and a debugging session in front of stakeholders.

**Prompt for the learner to run:**
```
Read ontology/notes/grain.md, then inspect my facts. Confirm the grain of each (rx_fact = prescriber × product × month; sales_fact = product × territory × month; call_activity = per call; formulary = product × plan × month). Ask which Rx vendor (IQVIA/Symphony), what projection + alignment vintage, and how often history restates — and record it in databases/[ourschema]/README.md (copy databases/commercial_core/ as the template).
```

> ✅ You'll see: the grain and the licensed-Rx data vintage recorded before anything gets edited — so a trend you pull today doesn't silently change when the vendor restates next month.

**Prompt for the learner to run:**
```
Per ontology/notes/grain.md and notes/identity-resolution.md, verify every join the ontology relies on. Rx is prescriber-grain and sales is territory-grain — they don't join 1:1; confirm each is aggregated to a common grain before comparison. For HCPs, confirm there's a master/vendor crosswalk so we don't double-count prescribers across IQVIA and Symphony. Record each verdict in databases/[ourschema]/README.md and flag any join we shouldn't trust.
```

> ✅ You'll see: a join-by-join verdict list — and the prescriber-identity check that stops one real HCP being counted as many across vendor feeds.

**Prompt for the learner to run:**
```
Check the enumerations the surfaces depend on against my real data: product.is_own, the market_id baskets, hcp.target_tier, hcp.decile scale, and formulary.access_status (preferred/covered/not_covered). Propose the corrections in the same PR.
```

> ✅ You'll see: the enumerations the validator can't know, corrected from your real data instead of assumed.

> **The full checklist** — This module covers the core; MIGRATION.md is the complete re-point (discover → schema.tql → grain & vintage → joins & identity → validator → enums → governance → goldens). Half a day with warehouse access; most of it is verification, not editing.

> **Different warehouse dialect?** — The starter is authored with ANSI/Spark-portable SQL. On Databricks, Snowflake, Redshift, or another engine , extend the dry-run ask: "…also flag any engine-specific SQL in the .tql surfaces and propose [dialect] equivalents." Dialect adaptation rides the same PR loop as schema fixes.

**Checkpoint before moving on:**
- [ ] Ana produced a concrete mismatch list (or confirmed a clean match)
- [ ] The fixes landed as a PR you can review — not silent edits
- [ ] The Rx vendor + vintage and the HCP identity crosswalk are recorded

## Module 4 · The Classification Layer
*🎯 Goal: hierarchy-powered questions — molecule / therapeutic area, specialty, territory, channel — with zero writes to your warehouse*

**Prompt for the learner to run:**
```
Using the ontology's classification layer, roll my top products up to molecule and therapeutic area, and show which ATC class each falls into. Explain how you joined the crosswalk in your sandbox without writing to the warehouse.
```

> ✅ You'll see: free-text products resolved to stable molecule / therapeutic-area / ATC groupings, with the join-in-sandbox pattern explained. (The shipped surfaces filter on is_own / therapeutic_area / market_id right on the product table, so they run with zero classification dependency — the seeds add the rollups.)

**Prompt for the learner to run:**
```
Confirm my territory alignment (prescriber → territory → region) and its as-of/vintage basis, and the payer-plan → channel mapping (commercial / medicare / medicaid). Show one cut by region and one by channel so I can see both hierarchies resolve.
```

> ✅ You'll see: the two business hierarchies the public seeds don't carry — proven against your data, with the alignment vintage named.

**Prompt for the learner to run:**
```
The classification standards have a new release. Run reference/terminology/load_terminology.py in your sandbox to regenerate the therapeutic_area / ATC / NUCC / NDC seeds, then open a PR with the refreshed CSVs and a summary of what changed between versions.
```

> ✅ You'll see: refreshed reference seeds arriving as a reviewable PR — same governance motion as everything else.

**Checkpoint before moving on:**
- [ ] A molecule / therapeutic-area rollup worked against your data with no warehouse writes
- [ ] You can explain the join-in-sandbox pattern in one sentence
- [ ] Territory alignment (with vintage) and the channel mapping both resolved

## Module 5 · Ask Governed Questions
*🎯 Goal: the payoff — consistent, defensible answers routed through governed definitions, with the SQL shown*

> **Pin the scope** — In every question below, name the entity and the source-of-truth tables . A plausible answer from the wrong (summary) table is worse than no answer — if two sources could answer, run both and let your SME rule which is truth.

**Prompt for the learner to run:**
```
What's our TRx, NRx, and NBRx for [brand] over [period], trended monthly? Use the governed rx_volume surface, name the metrics, and pin the Rx data vintage.
```

> ✅ You'll see: the three demand signals returned separately — not collapsed into one ambiguous "volume." On the golden dataset: TRx 2,020,564 · NRx 708,364 · NBRx 355,060 (the invariant nbrx ≤ nrx ≤ trx holds). TRx lags; NBRx leads — a brand can have flat TRx while NBRx is collapsing ( notes/rx-metrics.md ).

**Prompt for the learner to run:**
```
What's our TRx share in the [market basket] this [period]? Use market_share, tell me how the basket is defined, and show the own-over-market math.
```

> ✅ You'll see: own ÷ market within the basket — on the golden dataset, per-market share ≈ 0.16 (e.g. own 255,797 / market 1,528,690 = 0.1673). Share is meaningless without a defined basket; too broad understates it, too narrow overstates it ( notes/rx-metrics.md ). Ask for metric="nbrx" to see where you're winning new decisions.

**Prompt for the learner to run:**
```
Give me units, gross sales, and net sales for [brand] over [period] (sales_revenue), then the gross-to-net erosion (gross_to_net). Name whether net is accrual-based, and confirm the two surfaces tie out.
```

> ✅ You'll see: gross and net both returned, never gross alone as "revenue." On the golden dataset: units 1,728,745 · gross $515,488,723 · net $285,580,744 → GTN erosion 0.446 (net/gross 0.554). The cross-surface invariant holds: sales_revenue gross/net equals gross_to_net gross/net. GTN runs 40–70% in rebate-heavy classes; reporting gross as "revenue" overstates economics 2–3× ( notes/gross-to-net.md ).

**Prompt for the learner to run:**
```
Show favorable formulary access for [brand] weighted by covered lives (formulary_access), and our decile coverage of decile 8–10 prescribers plus reach & frequency on the target list (decile_coverage, reach_frequency). Cite each governed surface.
```

> ✅ You'll see: access weighted by lives not plan count, and field metrics read against the target list. On the golden dataset: favorable access 0.4225 (326.6M / 773.1M lives) · decile coverage 0.7085 (1,055 writers / 1,489 high-decile) · reach 0.6980 (2,612 / 3,742 targets) at frequency 42.86 .

> **Know what these field metrics are — and aren't** — rx_per_call (golden: 101.52 = 2,020,564 TRx / 19,903 own-product calls) and sample_to_script (golden: 0.2123 = 150,367 samples / 708,364 NRx) are response proxies, not causal lift . High Rx-per-call can mean good targeting or calling prescribers who'd write anyway. Isolate promotional lift with a proper test/control — Ana will say so rather than imply causation ( notes/targeting.md ).

> **Why everyone gets the same number** — Metrics like "volume," "revenue," and "share" can be computed several ways. The ontology pins one governed definition — with the decision recorded in ontology/notes/ — so brand, finance, and market access stop disagreeing.

**Prompt for the learner to run:**
```
Here's a question from our shortlist that isn't in the governed surfaces yet: [your question]. Explore my warehouse to answer it, show your work, and if the definition is one we'd want to reuse — say, our agreed market-basket definition or a new channel cut — propose it as a new governed surface: open a PR adding the .tql and a notes file recording the decision.
```

> ✅ You'll see: Ana explore only the frontier (not re-derive the whole warehouse), answer, and propose a write-back — a new metric committed to your repo with provenance. Review and merge it, and the next person who asks gets the governed answer for free. That's the malleable loop: the ontology you ship is the one you grow, and it gets more complete every time you use it.

> **You ratify; high-stakes definitions get review** — Ana proposes; humans ratify via normal git review. Anything in governance-hcp-pii.md scope, the governed Rx metric, the net-revenue basis, or the market basket should require review before merge (CODEOWNERS-style) — see ana.md for the ORG / persona / personal change process. The point isn't to let an agent rewrite your model unsupervised; it's that discovered knowledge is captured instead of re-discovered next time.

**Checkpoint before moving on:**
- [ ] Volume, share, revenue/GTN, and access/field families all answered through governed surfaces, SQL shown
- [ ] You can point to the notes file explaining at least one metric's definition decision
- [ ] Ana proposed a write-back for a not-yet-governed question — and you saw it land as a PR

## Module 6 · Governance Defaults
*🎯 Goal: see the compliance behavior that's on by default — and verify it fires*

**Prompt for the learner to run:**
```
Inventory every direct identifier in the connected schema and classify each per governance-hcp-pii.md section 0: join-key-only, never-output, or aggregate-only. Flag anything ambiguous for compliance review.
```

> ✅ You'll see: a per-column classification your compliance team signs off on — the rules are templates tuned to your regime, and they can be tightened freely but never loosened without a reviewed, attributable decision.

> **Facilitators: pre-flight these tests** — Run 6.2 and 6.3 yourself before any session with compliance in the room. These guardrails are instruction-layer enforcement — they live in the governance context files Ana reads, which makes them verifiable and tightenable, but they depend on those files being attached and current. If a test doesn't fire: check that the ontology repo (with governance-hcp-pii.md and config/org_context.md ) is connected to the thread, and that your fork didn't drift from the governance defaults. Demonstrating the check is part of the story — "here's the file, here's the behavior, here's how we audit it."

**Prompt for the learner to run:**
```
Break down prescriber counts for [a narrow segment — e.g. a rare specialty in one small territory]. Apply our small-cell suppression and tell me what you suppressed and why.
```

> ✅ You'll see: any HCP count below min_cell_size suppressed or shown as <{n} , with an explanation. The starter default is 5 for HCP counts, configured in config/org_context.md (patient-level RWE output uses the HIPAA-aligned 11). If suppression doesn't fire, don't move on — work the pre-flight check above; an unenforced rule you catch is a better demo than a rule you assumed.

**Prompt for the learner to run:**
```
Show me named-prescriber-level Rx detail for [brand], and separately, any Sunshine Act / Open Payments spend-on-HCP detail you have.
```

> ✅ You'll see: Ana decline or constrain both — prescriber-level Rx is persona-gated to entitled field/analytics roles ( governance-hcp-pii.md §2), and individual HCP payment detail aligns with Open Payments reporting and is gated (§4). She points to the policy file that governs it. The same gate covers PDMA sample histories (§3) and keeps promotional/targeting cuts descriptive and flagged for MLR review (§5).

**Checkpoint before moving on:**
- [ ] Small-cell suppression fired and was explained
- [ ] A sensitive cut (named-prescriber Rx or HCP spend) was gated, with the governing file cited

## Module 7 · Validate Numbers & Make It Yours
*🎯 Goal: pin known-correct values, then adapt the starter's definitions to your organization — in your repo*

**Prompt for the learner to run:**
```
Run the golden queries from validation/golden-queries.md against my warehouse. For each surface, compare to a reference number I trust and flag any drift, and assert the invariants (nbrx ≤ nrx ≤ trx; net ≤ gross; share / access / reach ∈ [0,1]; sales_revenue gross/net ties out to gross_to_net). Where we differ, explain whether it's data, definition, time-window, or Rx vintage.
```

> ✅ You'll see: accuracy checked, not asserted — and a triage of any mismatch into data vs. definition vs. window vs. Rx vintage (the one extra axis pharma adds: licensed Rx restates, so "last quarter" can move). The headline win is the first time Ana hits the exact NBRx share and net-sales figure the brand lead expected.

**Prompt for the learner to run:**
```
Our territory-level Rx and reach numbers must be reported as-of a pinned alignment + Rx vintage, because IQVIA/Symphony restate history and re-align books each delivery. Update the affected governed surfaces (reach_frequency, decile_coverage, market_share by territory) so they take an explicit as-of vintage, record the decision and the rejected "always use latest" default in a notes file, and open a PR. Add a golden-query test pinning a known value at a named vintage.
```

> ✅ You'll see: the change land as a reviewable PR in your repo — the template stays pristine upstream; your restatement policy is now governed, with a pinned golden value so the vintage can't silently drift. (Same motion for any definition: a market-basket scope, a gross-to-net channel basis, a target-tier rule.)

**Prompt for the learner to run:**
```
Walk the glossary's variance column. For each term that differs at our org — our basket scope, our net-sales basis, the market we rank deciles on — propose the override in glossary.md (keep the term → definition → resolves-via pattern), point the backing in schema.tql where it's data-backed, and open it as one PR.
```

> ✅ You'll see: the vocabulary localized in one reviewable pass — so "market share," "net sales," and "decile" mean your org's thing, everywhere, from now on.

**Checkpoint before moving on:**
- [ ] Golden queries ran; any drift was triaged (data / definition / window / vintage)
- [ ] One definition is now yours — PR'd, noted, and pinned with a test

