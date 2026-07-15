# MedTech Starter — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "MedTech Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/medtech-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Six Layers
*🎯 Goal: know what's in the box and where everything lives*

### The six layers

> **Standards alignment** — STANDARDS.md maps the model to the conventions it aligns with (FDA device risk class per 21 CFR 860, MDR reportability per 21 CFR 803, complaint handling per 21 CFR 820.198, GMDN nomenclature structurally). The semantic layer (metrics, routing, classification) is fully separated from the physical mapping : every physical table name lives in one file , ontology/schema.tql — re-point it and the metric logic stays put. The starter is authored against a generic catalog / installed-base / service / complaint model (SAP or other ERP, Salesforce CRM, ServiceMax field service, TrackWise/Veeva QMS-shaped) with ANSI/Spark-portable SQL; MIGRATION.md is the 8-step re-point checklist and works the same on Redshift, BigQuery, Snowflake, or Databricks (budget: about a half-day with warehouse access). For a deep technical tour, read DEEP_DIVE.md .

> **Two rules for a long, live session** — 1 · Checkpoint every couple of modules. Long threads have a ceiling. After every module or two, ask Ana: “Save a handoff document summarizing what we've built, what we decided, and what's next — so we can continue in a new thread.” If a thread ever maxes out, you lose nothing. 2 · Pin the scope in every prompt. Name the entity and the source-of-truth tables in each prompt (“…for [entity X], using the [base] tables, not the summary table”) — otherwise Ana may drift to a convenient summary table or query every source at once.

**Checkpoint before moving on:**
- [ ] You can name the six layers and find each one in the repo
- [ ] You know which classification ships in the box vs. needs your own licensed feed (GMDN coded terms, vendor market-share)
- [ ] You know the default model (generic catalog / installed-base / service / complaint, ANSI/Spark-portable) and where re-point lives (schema.tql)

## Module 1 · Define Your North Star

**Prompt for the learner to run:**
```
Help me define the North Star for our ontology before we build anything.
1. Look at the data connected to this thread and summarize in 3-4 lines what we have: the key tables, the grain, and the domains it covers (product catalog, accounts, sales, installed base, field service, post-market complaints).
2. Then ask me 5-7 sharp scoping questions to pin down what we are really doing - who will use this, what decision it changes on Monday morning, whether we are after commercial performance / post-market quality and safety / field service and reliability, what "working" looks like in 30 days, and where our data is messiest. Ask a few at a time, not all at once.
3. From my answers plus the data, recommend the archetype that fits (A commercial performance, B post-market quality and safety, or C field service and reliability) and draft our North Star: one short paragraph on what this ontology is for, plus the 6-8 questions it must answer in 30 days.
4. Save it as north_star.md in the ontology and propose it as a reviewed change, so every later step builds toward it.
```

> ✅ You will see: Ana summarize your data, ask scoping questions, recommend an archetype with a reason, and draft a north_star.md — a paragraph plus the questions that define "done." Review it, push back, ratify.

> **Why a prompt, not a 50-question checklist** — No giant pre-written question bank that nobody maintains. Ana generates the questions that matter for your data and your goal , on the spot — and the output is a sharp use-case definition, not a worksheet.

**Checkpoint before moving on:**
- [ ] You have a written north_star.md (purpose + the 6-8 questions it must answer)
- [ ] You picked an archetype (A / B / C), or Ana recommended one and you agreed
- [ ] It names something real that changes on Monday morning, not "model all of medtech"

## Module 2 · Connect Three Things
*🎯 Goal: ontology repo + warehouse + documents connected — then everything else happens in chat*

### 2.1 · Connect the ontology repo to Ana
This is the key step. In TextQL, add a Git connector and point it at your fork of the starter repo ( TextQLLabs/ontology-starter-kits/tree/main/medtech — no fork yet? Ask your TextQL contact; it takes minutes). Because the ontology is git-backed, Ana now has the entire model — every metric definition, every note, every classification rule — as a reference she reads on demand.

> **No second source of truth** — You don't copy anything into Ana. She reads the repo live; when the repo changes, Ana sees the change.

### 2.2 · Connect your data warehouse
Add the connector for the warehouse holding your product, installed-base, service, and complaint data (Redshift, BigQuery, Snowflake, Databricks, …) — typically sourced from your ERP (SAP) and CRM (Salesforce) for sales + installed base, your field-service system (ServiceMax), and your post-market quality system (TrackWise/Veeva QMS). Read-only access is enough.

> **Use your governed, contracted warehouse** — Post-market complaint and MDR data are FDA-regulated quality records (21 CFR 820 / 803) — auditable, with reportability timing that matters. Connect the enterprise warehouse that's already in scope for your quality-system, data-residency, and audit obligations — see ontology/notes/governance-quality.md .

### 2.3 · (Optional) Bring in your documents
Your real-world context — the commercial reporting workbook, the complaint-handling / MDR decision SOP, dbt models, the spreadsheet where someone defined "attach rate" — often lives in messy files. Upload them in chat, connect Google Drive, or connect SharePoint/OneDrive. Ana reads them alongside the ontology as corpus, not migration , and can fold what she learns into the model.

### 2.4 · Say hello

**Prompt for the learner to run:**
```
Read the ontology repo and give me a tour: what entities, metrics, and classification dimensions are defined, and what governed questions am I able to ask once my warehouse is validated?
```

> ✅ You'll see: Ana describe the model from the repo itself — proof the Git connector works and the ontology is being read as ground truth.

**Checkpoint before moving on:**
- [ ] Ana described the starter's entities and metrics from the connected repo
- [ ] Your warehouse connector is attached, read-only, and in your contracted region
- [ ] You know which of your documents you'll bring in (or that you're skipping this)

## Module 3 · Validate Against Your Schema
*🎯 Goal: before trusting numbers, prove the ontology's assumptions match your actual tables — and settle the grain — without writing SQL*

### 3.1 · The dry run — required

**Prompt for the learner to run:**
```
Look at the ontology repo, then inspect my warehouse. Run validation/dry-run-prompt.md against my schema: pull the information schema for my product, account, sales_txn, device, service_order, and complaint tables and tell me where the ontology's expected table and column names don't match what I actually have — including whether product carries category ('capital'/'consumable'/'service') and device_class, whether device carries status ('active'/'decommissioned') and install_date, whether service_order carries is_corrective and first_visit_resolved, and whether complaint carries is_mdr and is_serious flags. Propose the exact changes to ontology/schema.tql.
```

> ✅ You'll see: Ana discover your schema, diff it against the ontology, and hand you a precise list of fixes — table backings, column names, and the all-important category, device-status, service-flag, and complaint-flag questions. (The ready-made version lives in validation/dry-run-prompt.md .)

### 3.2 · Run the validator — required
The dry run is discovery; validation/validate_tql.py is the mechanical gate. It verifies every governed surface against your warehouse: each logical name resolves, each referenced column exists, each query compiles. Ana runs it for you — no terminal needed:

**Prompt for the learner to run:**
```
Run validation/validate_tql.py from the ontology repo in your sandbox — static checks first, then the SQL check against my warehouse. Report every failure with the file it's in and the fix it needs, then apply the fixes (column renames in the affected .tql, table renames in schema.tql only) and re-run until clean.
```

> ✅ You'll see: the typo'd-backing, wrong-alias, and missing-column bug classes caught here — instead of surfacing as wrong numbers in front of stakeholders.

> **Prefer the terminal?** — The same gate runs locally: python3 validation/validate_tql.py (static — no warehouse needed) · --check-sql (paste the output into Ana: rows = missing columns) · --dsn "<dsn>" --explain (live column check + compile test).

### 3.3 · Apply the fixes as a PR — required

**Prompt for the learner to run:**
```
Make those changes and open a pull request.
```

> ✅ You'll see: Ana edit the files and open a reviewable PR in your repo. Every physical table name lives in one place ( ontology/schema.tql ) — re-point it and the metric logic stays put. The join keys the surfaces rely on are product_id , account_id , and device_id .

> **Why this step matters** — Every medtech company's warehouse differs from the reference shape somewhere — a renamed column, a missing table, a different grain. Finding those before you trust a number is the difference between a defensible attach rate and a debugging session in front of the CCO.

### 3.4 · Decide your grain & verify your joins — required
Medtech data fans out across several grains — product (SKU) vs. device (installed unit) vs. service order vs. complaint — and joining across them carelessly multiplies counts. Three decisions dominate this module, and all are prompts: which fact answers which question, whether device is an active-vs-decommissioned snapshot counted as of the reporting date (the denominator behind attach, complaint, and availability rates), and how the per-1k-device rates use the active installed base — not the order line — as the denominator.

**Prompt for the learner to run:**
```
Read ontology/notes/grain.md, then inspect my tables. Confirm the grain of each: is product one row per SKU with category ('capital'/'consumable'/'service') and device_class? Is device one row per installed unit with status ('active'/'decommissioned') and install_date, so the active installed base is COUNT(*) WHERE status='active' AND install_date <= reporting date — NOT a row per order? Is sales_txn one row per order line, service_order one row per service event, and complaint one row per regulated complaint record? Tell me where counting decommissioned devices in the rate denominator, or treating a sales line as an installed unit, would distort the numbers — and record the decision in databases/[ourschema]/README.md (copy databases/device_core/ as the template).
```

> ✅ You'll see: the grain decision made and written down before anything gets edited — it shapes every count downstream. The traps to confirm: the per-1k-device rates (complaint, serious-event) and attach are active-installed-base questions over a device snapshot (never count decommissioned units, never count sales lines as devices); revenue is a sales-line question; and availability counts open corrective service orders against that same active base.

**Prompt for the learner to run:**
```
Verify every join the ontology relies on against my warehouse: does the key (product_id / account_id / device_id) exist on both sides, what's the overlap rate, and is the grain 1:1 or 1:N? Then confirm the bases: does sales_txn carry revenue, units and channel? Does device carry status and install_date so the active snapshot resolves? Do service_order.is_corrective and first_visit_resolved, and complaint.is_mdr and is_serious, exist as booleans? Record each verdict in databases/[ourschema]/README.md and flag any join or basis we shouldn't trust.
```

> ✅ You'll see: a join-by-join verdict list plus a basis check — because attach rate needs category='consumable' on the product, the per-1k rates need a resolvable active-device snapshot, first-time-fix needs first_visit_resolved , and MDR rate needs is_mdr ; a missing column or a mislabeled flag changes the number.

**Prompt for the learner to run:**
```
Find the dataset-specific literals the surfaces hard-code — the category enums ('capital'/'consumable'/'service'), the device_class values ('I'/'II'/'III'), the device status values ('active'/'decommissioned'), the channel spellings ('direct'/'distributor'), and the service_type values ('repair'/'pm'/'install') — check each against what's actually in my warehouse, and propose the corrections in the same PR. Also confirm the closure/resolution-day DATEDIFF dialect for my warehouse (Spark 2-arg DATEDIFF(end,start) vs. Redshift DATEDIFF(day,start,end) vs. BigQuery DATE_DIFF(end,start,DAY)).
```

> ✅ You'll see: the enumerations the validator can't know — category = 'consumable' , device_class = 'III' , status = 'active' , the channel spellings — corrected from your real data instead of assumed, plus the closure/resolution DATEDIFF dialect pinned for your engine.

> **The full checklist** — This module covers the core; MIGRATION.md is the complete 8-step re-point (discover → grain → schema.tql → identity → validator → literals → governance → glossary → goldens). Half a day with warehouse access; most of it is verification, not editing.

> **No device-status snapshot, or no install_date?** — If your warehouse can't resolve an active installed base as of the reporting date — no status flag, or no install_date — then every per-1k-device rate (complaint, serious-event) and attach loses its denominator and the numbers are meaningless. Flag it in the dry run; notes/installed-base.md has the as-of-date derivation. This is the difference between a true complaint rate and one divided by every unit ever shipped.

**Checkpoint before moving on:**
- [ ] Ana produced a concrete mismatch list (or confirmed a clean match)
- [ ] The product / device-snapshot / service-order / complaint grain — and active-vs-decommissioned, plus per-1k-device denominators — is decided and written down
- [ ] The fixes landed as a PR you can review — not silent edits — and you merged it (or know who reviews it)

## Module 4 · The Classification Layer
*🎯 Goal: rollup-powered questions — product family → category/modality, device class → FDA risk, complaint type → grouping — with zero writes to your warehouse*

### 4.1 · Prove the rollups work

**Prompt for the learner to run:**
```
Using the ontology's classification layer, roll my product_family values up to modality (Imaging / Monitoring / Infusion / Surgical / In-vitro diagnostics / …), map my device_class values to FDA risk class (I→low, II→moderate, III→high) using the 21 CFR 860 seed, and group my complaint_type values into the malfunction / injury / use-error grouping, then show me complaint rate and serious-event rate by modality and risk class. Explain how you joined the seed CSVs without writing to the warehouse.
```

> ✅ You'll see: raw product families, device classes, and complaint types resolved to meaningful modalities, FDA risk classes, and complaint groupings, with the federated join-in-sandbox pattern explained — and a reminder to analyze groupings , never free-text codes.

### 4.2 · Bring in your licensed feed — optional
You don't need this on day one — the public category/modality, FDA-risk-class, and complaint-type groupings are already committed. Come back when you want coded device nomenclature beyond the public groupings. These are licensed (e.g. GMDN device terms, IQVIA/vendor market-share) — the repo ships the structure and join logic, and the data comes from your own licensed feed :

**Prompt for the learner to run:**
```
We have a licensed nomenclature feed (e.g. GMDN device terms, or an IQVIA market-share dataset) in our warehouse at [table]. Per LICENSING.md, join it to the product/device by code so we get the grouped device concept (or market context) — keep the licensed data in our warehouse, commit only the join logic, and note the license + effective date in LICENSING.md. Open it as a PR.
```

> ✅ You'll see: the structural model light up with your licensed nomenclature — the modeling logic versioned in git, the licensed content staying in your warehouse, same governance motion as everything else.

**Checkpoint before moving on:**
- [ ] A modality, FDA-risk-class, and complaint-type-grouping rollup worked against your data with no warehouse writes
- [ ] You can explain the federated join-in-sandbox pattern in one sentence
- [ ] You know which classification ships public (category/modality, FDA risk class, complaint grouping) vs. licensed (GMDN, vendor market-share, from your feed)

## Module 5 · Ask Governed Questions
*🎯 Goal: the payoff — consistent, defensible answers routed through governed definitions, with the SQL shown*

> **Pin the scope** — In every question below, name the entity and the source-of-truth tables . A plausible answer from the wrong (summary) table is worse than no answer — if two sources could answer, run both and let your SME rule which is truth.

### 5.1 · Product revenue & installed base (the commercial pair)

**Prompt for the learner to run:**
```
What's our product revenue for 2024 (product_revenue.tql), and how many active devices are in the installed base as of the reporting date (installed_base.tql)? Tell me the basis — which categories are in revenue (capital / consumable / service), and that installed base counts active units as-of the reporting date, not every device ever shipped.
```

> ✅ You'll see: the governed surface return revenue ≈ $7.65B over an active installed base of 42,498 devices (window 2024, reporting date 2024-12-31). Ana names the basis (revenue across all categories over the order-date window; installed base = active units as-of the reporting date) instead of silently picking one.

### 5.2 · Consumable attach rate (the pull-through headline)

**Prompt for the learner to run:**
```
What's our consumable attach rate for 2024 (attach_rate.tql)? Tell me the basis — consumable revenue ÷ active installed devices — and confirm you're dividing by the active device snapshot, not by sales lines or every unit ever shipped.
```

> ✅ You'll see: the governed surface return consumable revenue ÷ active installed devices — the synthetic portfolio pins attach ≈ $78.3k per active device. Ana names the basis (the razor/razor-blade pull-through KPI, active installed base in the denominator) instead of conflating it with raw consumable revenue.

### 5.3 · Complaint rate & MDR share (the post-market quality pair)

**Prompt for the learner to run:**
```
Show me our post-market complaint rate for 2024 (complaint_rate.tql) — complaints per 1,000 active installed devices — and our MDR-reportable share (mdr_rate.tql). For complaint rate, confirm the per-1k-device denominator; for MDR rate, tell me the 21 CFR 803 reportability basis.
```

> ✅ You'll see: complaint rate ≈ 941 per 1,000 active devices and an MDR-reportable share ≈ 0.0998 (per 21 CFR 803). Ana notes complaint rate is normalized on the active installed base (not a raw count) and that MDR reportability is a regulated determination, not an assumed flag.

> **Know the MDR reportability basis** — The governed mdr_rate surface reports the share of received complaints flagged is_mdr — MDR-reportable per 21 CFR 803 . Reportability (and its 30-day timing) is a regulated determination made in your quality system, not a count you can assume. A rising MDR rate is an auditable safety/escalation signal. If your stakeholder means a different reportability scope, that's a different cut — Ana will say which one your question maps to. See notes/governance-quality.md .

### 5.4 · Serious events & complaint closure (the safety + timeliness pair)

**Prompt for the learner to run:**
```
Show our serious-event rate for 2024 (serious_event_rate.tql) — serious complaints per 1,000 active devices — and our complaint closure cycle time (complaint_closure_days.tql): complaints received, how many remain open, and average days to close. Confirm closure time is an auditable 21 CFR 820.198 measure.
```

> ✅ You'll see: serious-event rate ≈ 48.5 per 1,000 active devices (injury / death / serious deterioration) and average complaint closure ≈ 30.5 days — with Ana flagging that complaint-handling timeliness is auditable under 21 CFR 820.198 and that serious events normalize on the same active installed base.

### 5.5 · Service resolution & device availability (the field-service + reliability pair)

**Prompt for the learner to run:**
```
Show me our field-service resolution for 2024 (service_resolution.tql) — service orders, how many remain open, average days to close, and the first-time-fix rate — and our installed-base availability (device_availability.tql). Tell me which is the efficiency signal and which is the fleet-uptime signal.
```

> ✅ You'll see: average service resolution ≈ 20.5 days with a first-time-fix rate ≈ 0.6976 , and device availability ≈ 0.8483 (1 − devices with an open corrective order ÷ active installed base). Ana names the basis for each — resolution & first-time-fix on orders opened in the window, availability as a point-in-time fleet-uptime proxy — and pairs them as the two reliability levers.

> **Why everyone gets the same number** — Revenue, installed base, and complaint rate can each be computed several ways. The ontology pins one governed definition — revenue over the order-date window, installed base as the active as-of-reporting-date snapshot, complaint and serious-event rates per 1,000 active devices, with the decision recorded in ontology/notes/ — so Commercial, Field Service, and Quality stop disagreeing about which number is "the" number.

### 5.6 · When the answer isn't governed yet — watch the model grow
Now ask something from your shortlist that the starter doesn't already cover — warranty-vs-billable service mix, attach by modality, an installed-base-aging cut, a region-level complaint-rate trend, distributor-vs-direct revenue split. This is the important beat: a starter pack is a head start, not the finished model.

**Prompt for the learner to run:**
```
Here's a question from our shortlist that isn't in the governed surfaces yet: [your question]. Explore my warehouse to answer it, show your work, and if the definition is one we'd want to reuse, propose it as a new governed surface — open a PR adding the .tql and a notes file recording the decision and the basis.
```

> ✅ You'll see: Ana explore only the frontier (not re-derive the whole warehouse), answer, and propose a write-back — a new metric committed to your repo with provenance. Review and merge it, and the next person who asks gets the governed answer for free. That's the malleable loop: the ontology you ship is the one you grow, and it gets more complete every time you use it.

> **You ratify; high-stakes definitions get review** — Ana proposes; humans ratify via normal git review. Anything in governance-quality.md scope (FDA quality records, MDR reportability, account confidentiality, small-cell) or a core regulated surface (complaint rate, MDR rate, serious-event rate, closure time) should require review before merge (CODEOWNERS-style) — see STANDARDS.md . The point isn't to let an agent rewrite your model unsupervised; it's that discovered knowledge is captured instead of re-discovered next time.

**Checkpoint before moving on:**
- [ ] Revenue/installed base, attach rate, complaint/MDR rate, serious events/closure, and service resolution/availability all answered through governed surfaces, SQL and basis shown
- [ ] You can point to the notes file explaining at least one metric's definition decision
- [ ] Ana proposed a write-back for a not-yet-governed question — and you saw it land as a PR

## Module 6 · Governance & Quality-Record Defaults
*🎯 Goal: see the FDA quality-record & confidentiality behavior that's on by default — and verify it fires*

### 6.1 · Inventory your sensitive fields — day one
governance-quality.md §0 classifies the sensitive fields in the connected schema into a role — and the key distinction is that using an identifier as a join key is not the same as outputting it :

**Prompt for the learner to run:**
```
Inventory every sensitive field in the connected schema and classify each per governance-quality.md section 0: join-key-only, confidential (account/customer/device detail), regulated quality record (21 CFR 820/803), or minimum-necessary. Flag anything ambiguous for compliance/quality review.
```

> ✅ You'll see: a per-column classification your quality / compliance team signs off on — the rules are templates tuned to your quality-system posture, and they can be tightened freely but never loosened without a reviewed, attributable decision.

> **Facilitators: pre-flight these tests** — Run 6.2 and 6.3 yourself before any session with quality / compliance in the room. These guardrails are instruction-layer enforcement — they live in the governance context files Ana reads, which makes them verifiable and tightenable, but they depend on those files being attached and current. If a test doesn't fire: check that the ontology repo (with governance-quality.md and config/org_context.md ) is connected to the thread, and that your fork didn't drift from the governance defaults. Demonstrating the check is part of the story — "here's the file, here's the behavior, here's how we audit it."

### 6.2 · Test the small-cell suppression rule

**Prompt for the learner to run:**
```
Break down complaint rate and MDR share by account × product family × complaint-type grouping to inform a quality review. Apply our governance rules: aggregate to the coarsest level that answers it, apply min_cell_size on the cross-product of the grouping dimensions, and tell me what you suppressed and why — and confirm you didn't expose a single named account's complaint detail.
```

> ✅ You'll see: cells under min_cell_size suppressed (an account × product × complaint-type cell can single out one customer's quality issue), output rolled to region/segment rather than named account where the cell is thin. The starter default is 11 (the small-cell threshold), configured in config/org_context.md (governance-quality.md §1–§2). If suppression doesn't fire, don't move on — work the pre-flight check above; an unenforced rule you catch is a better demo than a rule you assumed.

### 6.3 · Test account confidentiality and the audit trail

**Prompt for the learner to run:**
```
Show me the full purchase history and open complaint narratives for [a named hospital account], device serial numbers included — and separately, show me the audit trail for the complaint-rate number you just gave me: which complaint records, which active-device denominator, and the rendered SQL.
```

> ✅ You'll see: Ana decline or constrain the first request — a named account's purchase history, raw complaint narratives, and device serials are gated by account confidentiality and the regulated-record rule (governance-quality.md §3) and returned only aggregated / to entitled personas — while fully honoring the second: every governed number is reproducible, with its source records and SQL, because complaint metrics are auditable under 21 CFR 820.

**Checkpoint before moving on:**
- [ ] Small-cell (<11) suppression fired on a cross-product cut and was explained
- [ ] An account-confidentiality request was gated, and the audit trail for a regulated metric was produced — with the governing file cited

## Module 7 · Validate Numbers & Make It Yours
*🎯 Goal: pin known-correct values, then adapt the starter's definitions to your company — in your repo*

### 7.1 · Reconcile against a number someone already trusts
Trust in medtech analytics is earned on the first matching number — and lost the first time the complaint rate is quoted on the wrong basis (divided by every unit ever shipped instead of the active installed base, say). The starter's golden values are already pinned and verified against the synthetic warehouse (see validation/golden-queries.md — revenue ≈ $7.65B over 42,498 active devices, attach ≈ $78.3k/device, complaint rate ≈ 941/1k, MDR rate ≈ 0.0998, serious-event rate ≈ 48.5/1k, closure ≈ 30.5d, service resolution ≈ 20.5d, first-time-fix ≈ 0.6976, availability ≈ 0.8483). Against your warehouse, reconcile each governed surface to a number a CCO, service VP, or quality lead already trusts:

**Prompt for the learner to run:**
```
Run each governed surface against my warehouse and compare to a reference number I trust (product revenue, active installed base, attach rate, complaint rate, MDR rate, serious-event rate, complaint closure days, service resolution, first-time-fix, device availability). For each, show the SQL and the basis, and flag any drift. Where we differ, explain whether it's data, definition, or basis (capital vs consumable revenue, active vs decommissioned devices, per-1k denominator, MDR reportability scope).
```

> ✅ You'll see: accuracy checked, not asserted — and a triage of any mismatch into data vs. definition vs. basis. The decisive moment is the first time the complaint rate or attach lands exactly where the quality or commercial leader expected.

### 7.2 · Assert the invariants
Even before you have an external reference, some numbers must agree with each other. The golden queries assert these:

**Prompt for the learner to run:**
```
Check the cross-surface invariants from validation/golden-queries.md against my data: mdr_reportable <= complaints and serious_events <= complaints in the complaint surfaces; 0 <= mdr_rate <= 1, 0 <= first_time_fix_rate <= 1, 0 <= availability <= 1; devices_with_open_corrective <= active_devices in device_availability; open_orders <= service_orders in service_resolution; attach_rate == consumable_revenue / active_devices; the active_devices denominator is identical across complaint_rate, serious_event_rate, attach_rate, and installed_base. Report any that don't hold.
```

> ✅ You'll see: internal consistency proven — if MDR-reportable exceeds total complaints, or a rate falls outside [0,1], or the active-device denominator drifts between surfaces, something is wrong before a stakeholder ever sees the number.

### 7.3 · Customize a definition — the MDR-reportability field lesson
Your company inevitably defines something differently — a revenue category boundary, what counts as the active installed base, an MDR-reportability scope. But the starter's flagship field lesson is one every medtech quality team hits, and it makes the perfect worked example because it's where a quality-system judgment and a dashboard number diverge:

> **MDR reportability — definition & timing** — The governed mdr_rate surface reports the share of received complaints flagged is_mdr — MDR-reportable per 21 CFR 803 . But "reportable" is a regulated determination , not a field you can assume: it turns on whether the event may have caused or contributed to a death or serious injury, or a malfunction that would be likely to do so on recurrence — and it carries a 30-day reporting clock (5 days for events requiring remedial action to prevent unreasonable risk). A complaint can be logged but not yet adjudicated; counting is_mdr before adjudication, or off the wrong date (received vs. became-aware vs. decision), misstates both the rate and the reporting-timeliness picture. Conflating an unadjudicated complaint flag with a true MDR determination is the most common reportability error in medtech operator reporting. Documented in notes/governance-quality.md and validation/golden-queries.md . (The same shape applies to the active-installed-base denominator — dividing complaint rate by every unit ever shipped instead of the active as-of-date snapshot flatters quality.)

**Prompt for the learner to run:**
```
Walk me through the MDR-reportability definition and timing in notes/governance-quality.md. Then check my warehouse: does mdr_rate.tql compute the share of received complaints flagged is_mdr per 21 CFR 803? Prove the gap — if I have an adjudication-status field and a became-aware date, approximate a "decided-MDR within the 30-day clock" rate and show how far it sits from the raw is_mdr share on my data. If our company determines or dates MDR reportability differently (a different awareness date, an adjudication gate, or a 5-day-vs-30-day split), add the governed surface in our repo, record the decision and the rejected default in a notes file, add a golden-query test pinning the value, and open a PR.
```

> ✅ You'll see: the most-confused metric in medtech operator reporting demonstrated on your own data and then made explicit — the reportability definition and timing confirmed in the surface, and any company-specific basis change landing as a reviewable PR in your repo with a pinned golden value. The template stays pristine upstream; your adaptations are yours.

### 7.4 · Localize the vocabulary
ontology/notes/glossary.md holds the canonical medtech terms — product, capital vs. consumable vs. service, installed base, active vs. decommissioned device, attach rate, complaint, MDR-reportable, serious event, complaint closure, service resolution, first-time-fix, device availability, device class, modality — each with a variance column flagging where your company diverges (revenue-category boundary; active-installed-base as-of rule; MDR reportability scope/timing; corrective-vs-PM service classification).

**Prompt for the learner to run:**
```
Walk the glossary's variance column for our portfolio. For each term that differs at our company — capital-vs-consumable boundary, active-installed-base as-of rule, MDR reportability scope/timing, what counts as a corrective service event, the attach denominator — propose the override in glossary.md, keeping the term → definition → resolves-via pattern, and open it as one PR.
```

> ✅ You'll see: the vocabulary localized in one reviewable pass — so "attach rate," "installed base," and "MDR-reportable" mean your company's thing, everywhere, from now on.

> **Two habits as you make it yours** — 1 · Write for the search box. As you extend the kit, keep a short README per folder and repeat the phrases your teams actually use (metric names, synonyms, team names) in the prose — future threads find context by search , not browsing. 2 · Let usage drive the roadmap. Stand up a weekly gap-review playbook: mine repeated questions, manual SQL, and mid-thread corrections; have Ana draft small reviewable patches; a named owner approves. The kit is the seed — usage is what grows it. (See Ontology Operations Module 4.)

**Checkpoint before moving on:**
- [ ] Governed surfaces reconciled to a trusted reference; any drift triaged (data / definition / basis)
- [ ] The MDR-reportability definition/timing distinction (or active-installed-base denominator) demonstrated and made explicit on your data
- [ ] One definition is now yours — PR'd, noted, and pinned with a golden-query test

