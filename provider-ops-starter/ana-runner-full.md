# Provider Ops Starter — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Provider Ops Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/provider-ops-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Six Layers
*🎯 Goal: know what's in the box and where everything lives*

### The six layers

> **Standards alignment** — STANDARDS.md maps the model to the conventions it aligns with (CMS MS-DRG & GMLOS, NUBC/UB-04 dispositions, NPI, US Census geography). The semantic layer (metrics, routing, classification) is fully separated from the physical mapping : every physical table name lives in one file , ontology/schema.tql — re-point it and the metric logic stays put. The starter is authored against a generic encounter / census / OR / revenue-cycle model (Epic Clarity/Caboodle, Oracle Health/Cerner, MEDITECH-shaped) with ANSI/Spark-portable SQL; MIGRATION.md is the 8-step re-point checklist and works the same on Redshift, BigQuery, Snowflake, or Databricks (budget: about a half-day with warehouse access). For a deep technical tour, read DEEP_DIVE.md .

> **Two rules for a long, live session** — 1 · Checkpoint every couple of modules. Long threads have a ceiling. After every module or two, ask Ana: “Save a handoff document summarizing what we've built, what we decided, and what's next — so we can continue in a new thread.” If a thread ever maxes out, you lose nothing. 2 · Pin the scope in every prompt. Name the entity and the source-of-truth tables in each prompt (“…for [entity X], using the [base] tables, not the summary table”) — otherwise Ana may drift to a convenient summary table or query every source at once.

**Checkpoint before moving on:**
- [ ] You can name the six layers and find each one in the repo
- [ ] You know which classification ships in the box vs. needs your own licensed feed (full coded groupers — CPT/SNOMED)
- [ ] You know the default model (generic encounter / census / OR / rev-cycle, ANSI/Spark-portable) and where re-point lives (schema.tql)

## Module 1 · Define Your North Star

**Prompt for the learner to run:**
```
Help me define the North Star for our ontology before we build anything.
1. Look at the data connected to this thread and summarize in 3-4 lines what we have: the key tables, the grain, and the domains it covers (encounters, census, OR, ambulatory, revenue cycle).
2. Then ask me 5-7 sharp scoping questions to pin down what we are really doing - who will use this, what decision it changes on Monday morning, whether we are after capacity and patient flow / surgical and acuity performance / revenue cycle and access, what "working" looks like in 30 days, and where our data is messiest. Ask a few at a time, not all at once.
3. From my answers plus the data, recommend the archetype that fits (A capacity and patient flow, B surgical and acuity performance, or C revenue cycle and access) and draft our North Star: one short paragraph on what this ontology is for, plus the 6-8 questions it must answer in 30 days.
4. Save it as north_star.md in the ontology and propose it as a reviewed change, so every later step builds toward it.
```

> ✅ You will see: Ana summarize your data, ask scoping questions, recommend an archetype with a reason, and draft a north_star.md — a paragraph plus the questions that define "done." Review it, push back, ratify.

> **Why a prompt, not a 50-question checklist** — No giant pre-written question bank that nobody maintains. Ana generates the questions that matter for your data and your goal , on the spot — and the output is a sharp use-case definition, not a worksheet.

**Checkpoint before moving on:**
- [ ] You have a written north_star.md (purpose + the 6-8 questions it must answer)
- [ ] You picked an archetype (A / B / C), or Ana recommended one and you agreed
- [ ] It names something real that changes on Monday morning, not "model all of hospital operations"

## Module 2 · Connect Three Things
*🎯 Goal: ontology repo + warehouse + documents connected — then everything else happens in chat*

### 2.1 · Connect the ontology repo to Ana
This is the key step. In TextQL, add a Git connector and point it at your fork of the starter repo ( TextQLLabs/ontology-starter-kits/tree/main/provider-ops — no fork yet? Ask your TextQL contact; it takes minutes). Because the ontology is git-backed, Ana now has the entire model — every metric definition, every note, every classification rule — as a reference she reads on demand.

> **No second source of truth** — You don't copy anything into Ana. She reads the repo live; when the repo changes, Ana sees the change.

### 2.2 · Connect your data warehouse
Add the connector for the warehouse holding your encounter / census / OR / revenue-cycle data (Redshift, BigQuery, Snowflake, Databricks, …) — typically sourced from Epic (Clarity/Caboodle), Oracle Health/Cerner, or MEDITECH, plus the ADT / scheduling / OR / revenue-cycle feeds. Read-only access is enough.

> **Use your governed, contracted warehouse** — Provider operations data is protected health information (PHI) under HIPAA — patient identifiers, MRNs, dates of birth. Connect the enterprise warehouse that's already in scope for your HIPAA, data-residency, and audit obligations — see ontology/notes/governance-phi.md .

### 2.3 · (Optional) Bring in your documents
Your real-world context — the operations reporting workbook, the LOS or readmission definition memo, dbt models, the spreadsheet where someone defined "occupancy" — often lives in messy files. Upload them in chat, connect Google Drive, or connect SharePoint/OneDrive. Ana reads them alongside the ontology as corpus, not migration , and can fold what she learns into the model.

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
Look at the ontology repo, then inspect my warehouse. Run validation/dry-run-prompt.md against my schema: pull the information schema for my encounter, patient, bed_census, or_case, appointment, and charge tables and tell me where the ontology's expected table and column names don't match what I actually have — including whether the encounter spine carries encounter_type (inpatient/ed/outpatient/observation), whether los_days is precomputed or must be derived, whether bed_census is a midnight snapshot, and whether the charge line carries denial_flag. Propose the exact changes to ontology/schema.tql.
```

> ✅ You'll see: Ana discover your schema, diff it against the ontology, and hand you a precise list of fixes — table backings, column names, and the all-important encounter-type, LOS-derivation, and census-grain questions. (The ready-made version lives in validation/dry-run-prompt.md .)

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

> ✅ You'll see: Ana edit the files and open a reviewable PR in your repo. Every physical table name lives in one place ( ontology/schema.tql ) — re-point it and the metric logic stays put. The join keys the surfaces rely on are patient_id , encounter_id , provider_id , facility_id , and department_id .

> **Why this step matters** — Every health system's warehouse differs from the reference shape somewhere — a renamed column, a missing table, a different grain. Finding those before you trust a number is the difference between a defensible ALOS and a debugging session in front of the COO.

### 3.4 · Decide your grain & verify your joins — required
Provider-ops data fans out across several grains — encounter vs. bed-census snapshot vs. charge line vs. OR case — and joining across them carelessly multiplies counts. Three decisions dominate this module, and all are prompts: which fact answers which question, whether bed_census is a point-in-time midnight snapshot summed to bed-days (not a row per patient), and how the encounter spine separates observation from inpatient (ED LOS, separately, needs admit/discharge timestamps because it's sub-day).

**Prompt for the learner to run:**
```
Read ontology/notes/grain.md, then inspect my tables. Confirm the grain of each: is encounter one row per visit/stay, with encounter_type separating inpatient / ed / outpatient / observation? Is bed_census a midnight snapshot (one row per facility per census_date) that I sum to bed-days — NOT a row per patient? Is or_case one row per surgical case and charge one row per rev-cycle line? Tell me where counting census rows as patients, or treating an observation stay as an inpatient discharge, would distort the numbers — and record the decision in databases/[ourschema]/README.md (copy databases/encounter_core/ as the template).
```

> ✅ You'll see: the grain decision made and written down before anything gets edited — it shapes every count downstream. The traps to confirm: bed occupancy is a bed-day question over the census snapshot (never count census rows as patients); ALOS and CMI are inpatient-discharge questions over the encounter spine (decide whether observation counts); and ED LOS is sub-day , so it's a timestamp difference, not los_days .

**Prompt for the learner to run:**
```
Verify every join the ontology relies on against my warehouse: does the key (patient_id / encounter_id / provider_id / facility_id / department_id) exist on both sides, what's the overlap rate, and is the grain 1:1 or 1:N? Then confirm the bases: does encounter carry los_days (precomputed) or must I derive it from admit_ts/discharge_ts? Are admit_ts and discharge_ts populated as timestamps so ED LOS can be computed sub-day? Does charge carry denial_flag, billed_amount and paid_amount? Record each verdict in databases/[ourschema]/README.md and flag any join or basis we shouldn't trust.
```

> ✅ You'll see: a join-by-join verdict list plus a basis check — because ED LOS needs real admit/discharge timestamps (sub-day), denial rate needs denial_flag on the charge line, and CMI needs drg_weight ; a missing column or a date-only timestamp changes the number.

**Prompt for the learner to run:**
```
Find the dataset-specific literals the surfaces hard-code — the encounter_type enums (inpatient/ed/outpatient/observation), the discharge_disposition values (home/snf/expired/ama/transfer/lwbs), the appointment status values ('no_show'/'cancelled'/'arrived'/'bumped'), and the payer_type spellings — check each against what's actually in my warehouse, and propose the corrections in the same PR. Also confirm the ED-LOS timestamp-difference dialect for my warehouse (Spark unix_timestamp vs. Redshift DATEDIFF vs. BigQuery TIMESTAMP_DIFF).
```

> ✅ You'll see: the enumerations the validator can't know — encounter_type = 'inpatient' , discharge_disposition = 'lwbs' , status = 'no_show' , the payer-type spellings — corrected from your real data instead of assumed, plus the ED-LOS timestamp dialect pinned for your engine.

> **The full checklist** — This module covers the core; MIGRATION.md is the complete 8-step re-point (discover → grain → schema.tql → identity → validator → literals → governance → glossary → goldens). Half a day with warehouse access; most of it is verification, not editing.

> **No precomputed LOS, or date-only timestamps?** — If your warehouse stores admit/discharge only as dates (no time component), inpatient ALOS can still be derived day-wise, but ED LOS — which is measured in hours — cannot ; you need real timestamps. Flag it in the dry run; notes/ed-throughput.md has the derivation and the per-engine timestamp-difference dialect. This is the difference between a true ED LOS and one that silently floors to zero.

**Checkpoint before moving on:**
- [ ] Ana produced a concrete mismatch list (or confirmed a clean match)
- [ ] The encounter / census-snapshot / charge-line / OR-case grain — and observation-vs-inpatient, plus ED-LOS-needs-timestamps — is decided and written down
- [ ] The fixes landed as a PR you can review — not silent edits — and you merged it (or know who reviews it)

## Module 4 · The Classification Layer
*🎯 Goal: rollup-powered questions — DRG → MDC/weight, specialty → service line, disposition → grouping — with zero writes to your warehouse*

### 4.1 · Prove the rollups work

**Prompt for the learner to run:**
```
Using the ontology's classification layer, roll my granular specialty/department values up to service line (Cardiovascular / Orthopedics / Neurosciences / …), group my discharge_disposition values into the UB-04 grouping (Home / Post-acute / Transfer / Mortality / …), and join my drg_code values to the CMS MS-DRG seed for MDC and relative weight, then show me discharges and average DRG weight by service line. Explain how you joined the seed CSVs without writing to the warehouse.
```

> ✅ You'll see: raw specialty, disposition, and DRG codes resolved to meaningful service lines, disposition groupings, and MDC/weight, with the federated join-in-sandbox pattern explained — and a reminder to analyze groupings , never free-text codes.

### 4.2 · Bring in your licensed feed — optional
You don't need this on day one — the public DRG, service-line, and disposition groupings are already committed (and the ms_drg seed is a small illustrative slice; hydrate the full ~760-DRG current-fiscal-year CMS table before benchmarking CMI). Come back when you want full coded groupers beyond public DRG/disposition. These are licensed (e.g. CPT, SNOMED) — the repo ships the structure and join logic, and the data comes from your own licensed feed :

**Prompt for the learner to run:**
```
We have a licensed coded-grouper feed (e.g. CPT / SNOMED) in our warehouse at [table]. Per LICENSING.md, join it to the encounter/charge by code so we get the grouped clinical concept — keep the licensed data in our warehouse, commit only the join logic, and note the license + effective date (and CMS DRG fiscal year) in LICENSING.md. Open it as a PR.
```

> ✅ You'll see: the structural model light up with your licensed clinical data — the modeling logic versioned in git, the licensed content staying in your warehouse, same governance motion as everything else.

**Checkpoint before moving on:**
- [ ] A service-line, disposition-grouping, and DRG → MDC/weight rollup worked against your data with no warehouse writes
- [ ] You can explain the federated join-in-sandbox pattern in one sentence
- [ ] You know which classification ships public (DRG/service-line/disposition/geography) vs. licensed (full coded groupers, from your feed) — and that the bundled DRG seed is illustrative, not the full CMS table

## Module 5 · Ask Governed Questions
*🎯 Goal: the payoff — consistent, defensible answers routed through governed definitions, with the SQL shown*

> **Pin the scope** — In every question below, name the entity and the source-of-truth tables . A plausible answer from the wrong (summary) table is worse than no answer — if two sources could answer, run both and let your SME rule which is truth.

### 5.1 · Length of stay & discharge volume (the throughput pair)

**Prompt for the learner to run:**
```
What's our average inpatient length of stay for 2024 (length_of_stay.tql), and how many inpatient discharges does that cover (discharge_volume.tql)? Tell me the basis — encounter type, whether observation is included, and whether deaths/transfers are in the denominator — and why.
```

> ✅ You'll see: the governed surface return ALOS ≈ 5.48 days over 78,572 inpatient discharges (window 2024). Ana names the basis (inpatient discharges only, observation excluded, discharged-in-window) instead of silently picking one.

### 5.2 · Bed occupancy (the capacity headline)

**Prompt for the learner to run:**
```
What's our inpatient bed occupancy for 2024 (bed_occupancy.tql)? Tell me the basis — occupied bed-days ÷ available bed-days from the midnight census — and confirm you're summing the snapshot to bed-days, not counting census rows as patients.
```

> ✅ You'll see: the governed surface return occupied bed-days ÷ available bed-days from the census — the synthetic portfolio pins occupancy ≈ 0.8356 . Ana names the basis (midnight census, bed-day weighted) instead of conflating it with a flow-weighted average.

### 5.3 · ED throughput & OR utilization (the flow + surgical-capacity pair)

**Prompt for the learner to run:**
```
Show me ED throughput for 2024 (ed_throughput.tql) — ED encounters, average ED length of stay in hours, and the LWBS rate — and OR prime-time utilization (or_utilization.tql). For ED LOS, confirm it's a sub-day timestamp difference; for OR utilization, tell me the block-minutes basis.
```

> ✅ You'll see: ED LOS ≈ 5.49 hours with an LWBS rate ≈ 4.09% (left without being seen), and OR utilization ≈ 0.7748 (actual in-room minutes ÷ allocated prime-time block minutes). Ana notes ED LOS is computed as a timestamp difference (sub-day), not from los_days .

> **Know the ED-LOS measurement basis** — The governed ed_throughput surface computes ED LOS as a timestamp difference ( admit_ts → discharge_ts , in hours) because it's sub-day — and the dialect differs by engine (Spark unix_timestamp vs. Redshift DATEDIFF(second,…) vs. BigQuery TIMESTAMP_DIFF ). If your warehouse only stores dates, ED LOS can't be computed honestly — Ana will say which it used. See notes/ed-throughput.md .

### 5.4 · Case mix index & readmission (the acuity + quality pair)

**Prompt for the learner to run:**
```
Show our case mix index for 2024 (case_mix_index.tql) and our 30-day all-cause readmission rate (readmission_rate.tql). Tell me how CMI normalizes volume for acuity, and confirm the readmission definition — operational all-cause within 30 days, deaths excluded from the index — vs. the CMS risk-adjusted measure.
```

> ✅ You'll see: CMI ≈ 1.7427 (average MS-DRG relative weight across inpatient discharges) and a 30-day all-cause readmission rate ≈ 0.0316 (3.16%) — with Ana flagging that this is the operational definition (index excludes deaths), not the payer/CMS risk-adjusted measure that lives in the healthcare-payer sibling.

> **Operational vs. CMS readmission** — The governed readmission_rate is the operational all-cause 30-day count (index discharges followed by another inpatient admission for the same patient within 30 days, deaths excluded from the index). It is not the CMS risk-adjusted, condition-specific, planned-readmission-excluded measure used for payment. If your stakeholder means the CMS number, that's a different surface — Ana will say which one your question maps to. See notes/readmission-operational.md .

### 5.5 · Denials & no-shows (the revenue-cycle + access pair)

**Prompt for the learner to run:**
```
Show me our revenue-cycle denial rate for 2024 (denial_rate.tql) and our appointment no-show rate (appointment_no_show_rate.tql), report them together, and tell me which is the clean-claim / cash signal and which is the access / clinic-capacity signal.
```

> ✅ You'll see: denial rate ≈ 0.0903 (denied billed dollars ÷ total billed) and no-show rate ≈ 0.1211 (no-show appointments ÷ scheduled). Ana names the basis for each — denials on billed dollars over the post-date window, no-shows on scheduled appointments over the slot window — and pairs them as the two leakage levers.

> **Why everyone gets the same number** — LOS, occupancy, and readmission can each be computed several ways. The ontology pins one governed definition — inpatient ALOS excluding observation, occupancy on the midnight-census bed-day basis, operational all-cause 30-day readmission, with the decision recorded in ontology/notes/ — so Capacity, Perioperative, Quality, and Revenue Cycle stop disagreeing about which number is "the" number.

### 5.6 · When the answer isn't governed yet — watch the model grow
Now ask something from your shortlist that the starter doesn't already cover — observed-to-expected LOS against the DRG GMLOS, a discharge-before-noon rate, ED boarding hours, surgical first-case on-time starts, a payer-mix cut of denials. This is the important beat: a starter pack is a head start, not the finished model.

**Prompt for the learner to run:**
```
Here's a question from our shortlist that isn't in the governed surfaces yet: [your question]. Explore my warehouse to answer it, show your work, and if the definition is one we'd want to reuse, propose it as a new governed surface — open a PR adding the .tql and a notes file recording the decision and the basis.
```

> ✅ You'll see: Ana explore only the frontier (not re-derive the whole warehouse), answer, and propose a write-back — a new metric committed to your repo with provenance. Review and merge it, and the next person who asks gets the governed answer for free. That's the malleable loop: the ontology you ship is the one you grow, and it gets more complete every time you use it.

> **You ratify; high-stakes definitions get review** — Ana proposes; humans ratify via normal git review. Anything in governance-phi.md scope (PHI, minimum-necessary, small-cell) or a core operational surface (LOS, occupancy, readmission, CMI) should require review before merge (CODEOWNERS-style) — see STANDARDS.md . The point isn't to let an agent rewrite your model unsupervised; it's that discovered knowledge is captured instead of re-discovered next time.

**Checkpoint before moving on:**
- [ ] LOS/discharge volume, bed occupancy, ED throughput/OR utilization, CMI/readmission, and denials/no-shows all answered through governed surfaces, SQL and basis shown
- [ ] You can point to the notes file explaining at least one metric's definition decision
- [ ] Ana proposed a write-back for a not-yet-governed question — and you saw it land as a PR

## Module 6 · Governance & PHI Defaults
*🎯 Goal: see the HIPAA / compliance behavior that's on by default — and verify it fires*

### 6.1 · Inventory your identifiers — day one
governance-phi.md §0 classifies every direct identifier in the connected schema into exactly one role — and the key distinction is that using an identifier as a join key is not the same as outputting it :

**Prompt for the learner to run:**
```
Inventory every direct identifier in the connected schema and classify each per governance-phi.md section 0: join-key-only, never-output, HIPAA-sensitive (age/ZIP3/date), or minimum-necessary. Flag anything ambiguous for compliance review.
```

> ✅ You'll see: a per-column classification your compliance / privacy team signs off on — the rules are templates tuned to your HIPAA posture, and they can be tightened freely but never loosened without a reviewed, attributable decision.

> **Facilitators: pre-flight these tests** — Run 6.2 and 6.3 yourself before any session with compliance / privacy in the room. These guardrails are instruction-layer enforcement — they live in the governance context files Ana reads, which makes them verifiable and tightenable, but they depend on those files being attached and current. If a test doesn't fire: check that the ontology repo (with governance-phi.md and config/org_context.md ) is connected to the thread, and that your fork didn't drift from the governance defaults. Demonstrating the check is part of the story — "here's the file, here's the behavior, here's how we audit it."

### 6.2 · Test the small-cell suppression rule

**Prompt for the learner to run:**
```
Break down readmission rate and ALOS by facility × service line × age band to inform a quality review. Apply our HIPAA rules: aggregate dates and geography to the coarsest level that answers it, apply min_cell_size on the cross-product of the grouping dimensions, and tell me what you suppressed and why — and confirm you emitted age bands, never raw DOB.
```

> ✅ You'll see: cells under min_cell_size suppressed (a facility × service line × age-band cell can re-identify a patient), geography rolled to ZIP3/region rather than full address, age emitted as a band rather than DOB. The starter default is 11 (the HIPAA-aligned small-cell threshold), configured in config/org_context.md (governance-phi.md §1–§2). If suppression doesn't fire, don't move on — work the pre-flight check above; an unenforced rule you catch is a better demo than a rule you assumed.

### 6.3 · Test patient-PHI and facility/region gating

**Prompt for the learner to run:**
```
Show me patient-level contact and encounter detail for our 30-day readmitted patients — names, MRNs, and dates of birth — and separately, ALOS and occupancy for facilities in a region I'm not assigned to.
```

> ✅ You'll see: Ana decline or constrain both requests — patient name/MRN/DOB and encounter detail is gated by HIPAA minimum-necessary and aggregated (governance-phi.md §3), and cross-region facility detail is blocked by facility/region segregation (§4) — pointing to the policy file that governs each.

**Checkpoint before moving on:**
- [ ] Small-cell (<11) suppression fired on a cross-product cut and was explained
- [ ] A patient-PHI or facility/region-segregation request was gated, with the governing file cited

## Module 7 · Validate Numbers & Make It Yours
*🎯 Goal: pin known-correct values, then adapt the starter's definitions to your health system — in your repo*

### 7.1 · Reconcile against a number someone already trusts
Trust in provider-ops analytics is earned on the first matching number — and lost the first time ALOS is quoted on the wrong basis (observation folded into inpatient, say). The starter's golden values are already pinned and verified against the synthetic warehouse (see validation/golden-queries.md — ALOS ≈ 5.48d over 78,572 inpatient discharges, bed occupancy ≈ 0.8356, ED LOS ≈ 5.49h, LWBS ≈ 4.09%, OR utilization ≈ 0.7748, CMI ≈ 1.7427, 30-day readmission ≈ 0.0316, denial rate ≈ 0.0903, no-show rate ≈ 0.1211). Against your warehouse, reconcile each governed surface to a number a COO or service-line leader already trusts:

**Prompt for the learner to run:**
```
Run each governed surface against my warehouse and compare to a reference number I trust (ALOS, discharge volume, bed occupancy, ED LOS, LWBS, OR utilization, CMI, readmission, denial rate, no-show rate). For each, show the SQL and the basis, and flag any drift. Where we differ, explain whether it's data, definition, or basis (observation vs inpatient, midnight census vs flow, operational vs CMS readmission, billed vs paid).
```

> ✅ You'll see: accuracy checked, not asserted — and a triage of any mismatch into data vs. definition vs. basis. The decisive moment is the first time ALOS or occupancy lands exactly where the operations leader expected.

### 7.2 · Assert the invariants
Even before you have an external reference, some numbers must agree with each other. The golden queries assert these:

**Prompt for the learner to run:**
```
Check the cross-surface invariants from validation/golden-queries.md against my data: occupied_bed_days <= available_bed_days in bed_occupancy; 0 <= bed_occupancy <= 1; readmission_rate, denial_rate, lwbs_rate and no_show_rate all between 0 and 1; readmission index discharges exclude disposition='expired'; total_patient_days == avg_los_days * discharges in length_of_stay; case_mix_index averages only inpatient discharges with a non-null drg_weight. Report any that don't hold.
```

> ✅ You'll see: internal consistency proven — if occupied bed-days exceed available, or a rate falls outside [0,1], something is wrong before a stakeholder ever sees the number.

### 7.3 · Customize a definition — the operational-vs-CMS readmission lesson
Your health system inevitably defines something differently — an LOS boundary, what counts as a discharge, a readmission window. But the starter's flagship field lesson is one every operator hits, and it makes the perfect worked example because it's where the number on the operations dashboard and the number CMS pays on diverge:

> **Operational vs. CMS risk-adjusted readmission** — The governed readmission_rate surface reports the operational all-cause 30-day rate: index inpatient discharges (deaths excluded) followed by another inpatient admission for the same patient within 30 days. That is not the same as the CMS risk-adjusted readmission measure — which is condition-specific (HF, AMI, pneumonia, …), risk-adjusted for patient comorbidity, and excludes planned readmissions , and which drives the Hospital Readmissions Reduction Program penalty. A unit can look fine on the operational all-cause number and still carry a CMS penalty on a condition cohort. Quoting the operational rate as if it were the CMS measure misreads the penalty risk; conflating the two is the most common readmission error in operator reporting. Documented in notes/readmission-operational.md and validation/golden-queries.md . (The same shape applies to the observation-vs-inpatient ALOS boundary — folding observation stays into inpatient ALOS flatters throughput.)

**Prompt for the learner to run:**
```
Walk me through the operational-vs-CMS readmission distinction in notes/readmission-operational.md. Then check my warehouse: does readmission_rate.tql compute the operational all-cause 30-day rate (index excludes deaths, any-cause inpatient readmit within 30 days)? Prove the gap — if I have a condition flag and a planned-admission flag, approximate a CMS-style condition-specific, planned-excluded rate and show how far it sits from the operational number on my data. If our health system reports readmission differently (a different window, a successor-encounter model, or the CMS measure as headline), add the governed surface in our repo, record the decision and the rejected default in a notes file, add a golden-query test pinning the value, and open a PR.
```

> ✅ You'll see: the most-confused metric in operator reporting demonstrated on your own data and then made explicit — the operational-vs-CMS distinction confirmed in the surface, and any health-system-specific basis change landing as a reviewable PR in your repo with a pinned golden value. The template stays pristine upstream; your adaptations are yours.

### 7.4 · Localize the vocabulary
ontology/notes/glossary.md holds the canonical provider-ops terms — encounter, inpatient vs. observation, discharge, length of stay, bed occupancy, ED LOS, LWBS, OR utilization (block basis), case mix index, readmission (operational vs. CMS), denial, no-show, service line — each with a variance column flagging where your health system diverges (observation-vs-inpatient boundary; staffed vs. licensed beds; ED LOS arrival-to-departure vs. arrival-to-disposition; readmission-as-count vs. CMS measure).

**Prompt for the learner to run:**
```
Walk the glossary's variance column for our service line(s). For each term that differs at our health system — observation-vs-inpatient boundary, occupancy bed basis, ED LOS endpoints, readmission window, what counts as a discharge — propose the override in glossary.md, keeping the term → definition → resolves-via pattern, and open it as one PR.
```

> ✅ You'll see: the vocabulary localized in one reviewable pass — so "length of stay," "occupancy," and "readmission" mean your health system's thing, everywhere, from now on.

> **Two habits as you make it yours** — 1 · Write for the search box. As you extend the kit, keep a short README per folder and repeat the phrases your teams actually use (metric names, synonyms, team names) in the prose — future threads find context by search , not browsing. 2 · Let usage drive the roadmap. Stand up a weekly gap-review playbook: mine repeated questions, manual SQL, and mid-thread corrections; have Ana draft small reviewable patches; a named owner approves. The kit is the seed — usage is what grows it. (See Ontology Operations Module 4.)

**Checkpoint before moving on:**
- [ ] Governed surfaces reconciled to a trusted reference; any drift triaged (data / definition / basis)
- [ ] The operational-vs-CMS readmission distinction (or observation-vs-inpatient ALOS boundary) demonstrated and made explicit on your data
- [ ] One definition is now yours — PR'd, noted, and pinned with a golden-query test

