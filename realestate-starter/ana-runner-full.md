# Real Estate Starter — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Real Estate Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/realestate-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Six Layers
*🎯 Goal: know what's in the box and where everything lives*

### The six layers

> **Standards alignment** — STANDARDS.md maps the model to the conventions it aligns with (NCREIF / NAREIT reporting, BOMA/IREM expense classifications, US Census CBSA geography). The semantic layer (metrics, routing, classification) is fully separated from the physical mapping : every physical table name lives in one file , ontology/schema.tql — re-point it and the metric logic stays put. The starter is authored against a generic property-management + financials model (Yardi/MRI/RealPage/Entrata-shaped) with ANSI/Spark-portable SQL; MIGRATION.md is the 8-step re-point checklist and works the same on Redshift, BigQuery, Snowflake, or Databricks (budget: about a half-day with warehouse access). For a deep technical tour, read DEEP_DIVE.md .

> **Two rules for a long, live session** — 1 · Checkpoint every couple of modules. Long threads have a ceiling. After every module or two, ask Ana: “Save a handoff document summarizing what we've built, what we decided, and what's next — so we can continue in a new thread.” If a thread ever maxes out, you lose nothing. 2 · Pin the scope in every prompt. Name the entity and the source-of-truth tables in each prompt (“…for [entity X], using the [base] tables, not the summary table”) — otherwise Ana may drift to a convenient summary table or query every source at once.

**Checkpoint before moving on:**
- [ ] You can name the six layers and find each one in the repo
- [ ] You know which classification ships in the box vs. needs your own licensed feed (CoStar/RealPage/Yardi Matrix)
- [ ] You know the default model (generic property-management + financials, ANSI/Spark-portable) and where re-point lives (schema.tql)

## Module 1 · Define Your North Star

**Prompt for the learner to run:**
```
Help me define the North Star for our ontology before we build anything.
1. Look at the data connected to this thread and summarize in 3-4 lines what we have: the key tables, the grain, and the domains it covers.
2. Then ask me 5-7 sharp scoping questions to pin down what we are really doing - who will use this, what decision it changes on Monday morning, whether we are after portfolio/asset reporting parity / revenue and collections / operations and leasing, what "working" looks like in 30 days, and where our data is messiest. Ask a few at a time, not all at once.
3. From my answers plus the data, recommend the archetype that fits (A portfolio/asset reporting parity, B revenue and collections, or C operations and leasing) and draft our North Star: one short paragraph on what this ontology is for, plus the 6-8 questions it must answer in 30 days.
4. Save it as north_star.md in the ontology and propose it as a reviewed change, so every later step builds toward it.
```

> ✅ You will see: Ana summarize your data, ask scoping questions, recommend an archetype with a reason, and draft a north_star.md — a paragraph plus the questions that define "done." Review it, push back, ratify.

> **Why a prompt, not a 50-question checklist** — No giant pre-written question bank that nobody maintains. Ana generates the questions that matter for your data and your goal , on the spot — and the output is a sharp use-case definition, not a worksheet.

**Checkpoint before moving on:**
- [ ] You have a written north_star.md (purpose + the 6-8 questions it must answer)
- [ ] You picked an archetype (A / B / C), or Ana recommended one and you agreed
- [ ] It names something real that changes on Monday morning, not "model all of real estate"

## Module 2 · Connect Three Things
*🎯 Goal: ontology repo + warehouse + documents connected — then everything else happens in chat*

### 2.1 · Connect the ontology repo to Ana
This is the key step. In TextQL, add a Git connector and point it at your fork of the starter repo ( TextQLLabs/ontology-starter-kits/tree/main/realestate — no fork yet? Ask your TextQL contact; it takes minutes). Because the ontology is git-backed, Ana now has the entire model — every metric definition, every note, every classification rule — as a reference she reads on demand.

> **No second source of truth** — You don't copy anything into Ana. She reads the repo live; when the repo changes, Ana sees the change.

### 2.2 · Connect your data warehouse
Add the connector for the warehouse holding your property-management / leasing / financials data (Redshift, BigQuery, Snowflake, Databricks, …) — typically sourced from Yardi, MRI, RealPage, or Entrata. Read-only access is enough.

> **Use your governed, contracted warehouse** — Real estate is fair-housing regulated and tenant records carry PII (names, and SSNs for residential applicants). Connect the enterprise warehouse that's already in scope for your data-residency and audit obligations — see ontology/notes/governance-pii-fairhousing.md .

### 2.3 · (Optional) Bring in your documents
Your real-world context — the asset-management reporting workbook, an operating-expense classification policy, dbt models, the spreadsheet where someone defined "occupancy" — often lives in messy files. Upload them in chat, connect Google Drive, or connect SharePoint/OneDrive. Ana reads them alongside the ontology as corpus, not migration , and can fold what she learns into the model.

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
Look at the ontology repo, then inspect my warehouse. Run validation/dry-run-prompt.md against my schema: pull the information schema for my property, unit, lease, ledger, and property-financial tables and tell me where the ontology's expected table and column names don't match what I actually have — including whether unit status is a point-in-time snapshot or a period series, and whether the ledger carries billed and paid separately. Propose the exact changes to ontology/schema.tql.
```

> ✅ You'll see: Ana discover your schema, diff it against the ontology, and hand you a precise list of fixes — table backings, column names, and the all-important unit-status and billed-vs-collected questions. (The ready-made version lives in validation/dry-run-prompt.md .)

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

> ✅ You'll see: Ana edit the files and open a reviewable PR in your repo. Every physical table name lives in one place ( ontology/schema.tql ) — re-point it and the metric logic stays put. The join keys the surfaces rely on are property_id , unit_id , lease_id , and tenant_id .

> **Why this step matters** — Every operator's warehouse differs from the reference shape somewhere — a renamed column, a missing table, a different grain. Finding those before you trust a number is the difference between a defensible NOI and a debugging session in front of the asset manager.

### 3.4 · Decide your grain & verify your joins — required
Real-estate data fans out across several grains — property vs. unit vs. lease vs. ledger vs. property-financial — and joining across them carelessly multiplies counts. Two decisions dominate this module, and both are prompts: which fact answers which question, and whether unit status is a point-in-time snapshot (one row per unit, current) or a period series (one row per unit per month).

**Prompt for the learner to run:**
```
Read ontology/notes/grain.md, then inspect my tables. Confirm the grain of each: is there one property table and a separate unit table (many units per property)? Is unit status a point-in-time snapshot (one current row per unit) or a monthly period series? Is the lease_ledger one row per lease per period per charge, and is property_financial one row per property per month? Tell me where counting unit rows as properties, or summing a monthly financial series as if it were a single period, would fan out — and record the decision in databases/[ourschema]/README.md (copy databases/property_core/ as the template).
```

> ✅ You'll see: the grain decision made and written down before anything gets edited — it shapes every count downstream. The trap to confirm: occupancy is a point-in-time question over the unit snapshot, while NOI and collections are period questions that sum a monthly series; anchor unit counts on the snapshot and never sum a monthly financial row as a single period .

**Prompt for the learner to run:**
```
Verify every join the ontology relies on against my warehouse: does the key (property_id / unit_id / lease_id / tenant_id) exist on both sides, what's the overlap rate, and is the grain 1:1 or 1:N? Then confirm the bases: in the ledger, are amount_billed and amount_paid both populated (so collections and delinquency are real, not assumed)? Is unit.status carrying the occupied / vacant / down values the occupancy surface expects? Record each verdict in databases/[ourschema]/README.md and flag any join or basis we shouldn't trust.
```

> ✅ You'll see: a join-by-join verdict list plus a basis check — because collections needs both billed and paid, and occupancy needs the down/offline status to exclude it from the denominator; a billed-only or status-less warehouse changes the number.

**Prompt for the learner to run:**
```
Find the dataset-specific literals the surfaces hard-code — the unit status enums (occupied/vacant/down), the lease status ('active'), the renewal_status values ('renewed'/'non_renewed'/'month_to_month'), the property_type spelling, and the charge_type values — check each against what's actually in my warehouse, and propose the corrections in the same PR.
```

> ✅ You'll see: the enumerations the validator can't know — status = 'occupied' , lease.status = 'active' , the renewal flag, the property-type and charge-type spellings — corrected from your real data instead of assumed.

> **The full checklist** — This module covers the core; MIGRATION.md is the complete 8-step re-point (discover → grain → schema.tql → identity → validator → literals → governance → glossary → goldens). Half a day with warehouse access; most of it is verification, not editing.

> **No monthly financial series?** — If your warehouse stores income and expense only as an annual or single-period roll-up (no property_financial monthly series), the NOI and opex surfaces can't sum a window directly — you must align the period grain first. Flag it in the dry run; notes/noi-definition.md has the derivation. This is the difference between a true same-store NOI and one that double-counts or omits months.

**Checkpoint before moving on:**
- [ ] Ana produced a concrete mismatch list (or confirmed a clean match)
- [ ] The property/unit/lease/ledger/property-financial grain — and unit status point-in-time vs. period — is decided and written down
- [ ] The fixes landed as a PR you can review — not silent edits — and you merged it (or know who reviews it)

## Module 4 · The Classification Layer
*🎯 Goal: rollup-powered questions — property-type → sector, charge-type → recoverable — with zero writes to your warehouse*

### 4.1 · Prove the rollups work

**Prompt for the learner to run:**
```
Using the ontology's classification layer, roll my granular property-type values up to sector (residential / commercial / industrial), and group my charge_type values into the revenue/recoverable grouping (base_rent / recoverable / other_income), then show me recoverable charges as a share of billings. Explain how you joined the seed CSVs without writing to the warehouse.
```

> ✅ You'll see: raw property and charge codes resolved to meaningful sectors and recoverable groupings, with the federated join-in-sandbox pattern explained — and a reminder to analyze groupings , never free-text charge codes.

### 4.2 · Bring in your licensed feed — optional
You don't need this on day one — the public property-type and charge-type groupings are already committed. Come back when you want market rents, comps, or submarket analytics . These are licensed (CoStar / RealPage / Yardi Matrix) — the repo ships the structure and join logic, and the data comes from your own licensed feed :

**Prompt for the learner to run:**
```
We have a CoStar / RealPage market-rent feed in our warehouse at [table]. Per LICENSING.md, join it to unit.market_rent / property.market by submarket so we get market rent vs. in-place rent (loss-to-lease) — keep the licensed data in our warehouse, commit only the join logic, and note the license + effective date in LICENSING.md. Open it as a PR.
```

> ✅ You'll see: the structural model light up with your licensed market data — the modeling logic versioned in git, the licensed comps staying in your warehouse, same governance motion as everything else.

**Checkpoint before moving on:**
- [ ] A sector and recoverable-charge rollup worked against your data with no warehouse writes
- [ ] You can explain the federated join-in-sandbox pattern in one sentence
- [ ] You know which classification ships public (property-type/charge-type/CBSA) vs. licensed (CoStar/RealPage/Yardi Matrix, from your feed)

## Module 5 · Ask Governed Questions
*🎯 Goal: the payoff — consistent, defensible answers routed through governed definitions, with the SQL shown*

> **Pin the scope** — In every question below, name the entity and the source-of-truth tables . A plausible answer from the wrong (summary) table is worse than no answer — if two sources could answer, run both and let your SME rule which is truth.

### 5.1 · Occupancy (the headline operations metric)

**Prompt for the learner to run:**
```
What's our physical occupancy as of the reporting date? Use the governed definition (occupancy_rate.tql) and tell me the basis — physical vs. economic, and whether down/offline units are in the denominator — and why.
```

> ✅ You'll see: the governed surface return occupied units ÷ rentable units, with down/offline units excluded from the denominator — the synthetic portfolio pins occupancy ≈ 0.948 . Ana names the basis (physical, point-in-time) instead of silently picking one; economic occupancy (rent-weighted, net of concessions) is a separate definition.

### 5.2 · Rent roll & NOI

**Prompt for the learner to run:**
```
Show me the in-place monthly rent roll as of the reporting date (rent_roll.tql) and our NOI and NOI margin for 2024 (noi.tql). For NOI, tell me explicitly what's included and what's correctly excluded.
```

> ✅ You'll see: the in-place rent roll ≈ $112M/mo (sum of contractual rent for leases in force, + annualized), and NOI = (rental + other income) − operating expense with NOI margin ≈ 0.552 . Ana states that NOI excludes debt service, capex, depreciation, and tax (below-the-line) — the definition asserted in the golden queries.

### 5.3 · Collections & delinquency (the two cash levers)

**Prompt for the learner to run:**
```
Show me our collections rate (collections_rate.tql) and delinquency rate (delinquency_rate.tql) for 2024, report them together, and tell me which is the cash-efficiency signal and which is the arrears/credit-risk signal.
```

> ✅ You'll see: collections ≈ 0.958 (amount paid ÷ amount billed) and delinquency ≈ 0.042 (past-due outstanding ÷ billed, as of the reporting date) — paired, because collections alone hides whether the gap is timing or true arrears. Note collections + the uncollected share reconcile against the ledger.

> **Know the billed-vs-collected basis** — The governed collections_rate and delinquency_rate surfaces both read the ledger ( amount_billed vs. amount_paid ). If your system only stores billed charges (no cash applied back), these can't be computed honestly — Ana will say which it used rather than imply cash. See notes/collections.md .

### 5.4 · Operating-expense ratio, renewals & rollover

**Prompt for the learner to run:**
```
Show our operating-expense ratio for 2024 (operating_expense_ratio.tql), our lease renewal rate (renewal_rate.tql), and our lease-expiration / rollover exposure for the next 12 months (lease_expirations.tql). Tell me how opex ratio relates to NOI margin.
```

> ✅ You'll see: opex ratio ≈ 0.448 (operating expense ÷ total income) — with Ana noting opex_ratio + noi_margin ≈ 1 — renewal rate ≈ 0.503 (renewed ÷ up-for-renewal), and ≈ 38,753 leases expiring in the rollover window. The renewal surface supports a rent-weighted (dollar-retention) basis too.

> **Renewal modeled as a flag vs. a successor lease** — The governed renewal_rate reads a renewal_status outcome column. If your system models renewal as a successor lease (a new lease_id linked to the prior one) instead of a status flag, the surface needs adapting — Ana will say which pattern your data uses. See notes/leasing.md .

> **Why everyone gets the same number** — Occupancy, NOI, and collections can each be computed several ways. The ontology pins one governed definition — physical occupancy, NOI excluding below-the-line, collections on the ledger basis, with the decision recorded in ontology/notes/ — so Asset Management, Operations, and Finance stop disagreeing about which number is "the" number.

### 5.5 · When the answer isn't governed yet — watch the model grow
Now ask something from your shortlist that the starter doesn't already cover — your economic-occupancy fact (rent-weighted, net of concessions), a loss-to-lease cut against market rent, a same-store comparison, a turnover/days-vacant metric. This is the important beat: a starter pack is a head start, not the finished model.

**Prompt for the learner to run:**
```
Here's a question from our shortlist that isn't in the governed surfaces yet: [your question]. Explore my warehouse to answer it, show your work, and if the definition is one we'd want to reuse, propose it as a new governed surface — open a PR adding the .tql and a notes file recording the decision and the basis.
```

> ✅ You'll see: Ana explore only the frontier (not re-derive the whole warehouse), answer, and propose a write-back — a new metric committed to your repo with provenance. Review and merge it, and the next person who asks gets the governed answer for free. That's the malleable loop: the ontology you ship is the one you grow, and it gets more complete every time you use it.

> **You ratify; high-stakes definitions get review** — Ana proposes; humans ratify via normal git review. Anything in governance-pii-fairhousing.md scope (fair housing, tenant PII, owner/fund segregation) or a core financial surface (NOI, occupancy, collections) should require review before merge (CODEOWNERS-style) — see STANDARDS.md . The point isn't to let an agent rewrite your model unsupervised; it's that discovered knowledge is captured instead of re-discovered next time.

**Checkpoint before moving on:**
- [ ] Occupancy, rent roll/NOI, collections/delinquency, opex ratio, renewals and lease expirations all answered through governed surfaces, SQL and basis shown
- [ ] You can point to the notes file explaining at least one metric's definition decision
- [ ] Ana proposed a write-back for a not-yet-governed question — and you saw it land as a PR

## Module 6 · Governance & PII Defaults
*🎯 Goal: see the compliance behavior that's on by default — and verify it fires*

### 6.1 · Inventory your identifiers — day one
governance-pii-fairhousing.md §0 classifies every direct identifier in the connected schema into exactly one role — and the key distinction is that using an identifier as a join key is not the same as outputting it :

**Prompt for the learner to run:**
```
Inventory every direct identifier in the connected schema and classify each per governance-pii-fairhousing.md section 0: join-key-only, never-output, fair-housing-sensitive, or aggregate-only. Flag anything ambiguous for compliance review.
```

> ✅ You'll see: a per-column classification your compliance team signs off on — the rules are templates tuned to your regime, and they can be tightened freely but never loosened without a reviewed, attributable decision.

> **Facilitators: pre-flight these tests** — Run 6.2 and 6.3 yourself before any session with compliance in the room. These guardrails are instruction-layer enforcement — they live in the governance context files Ana reads, which makes them verifiable and tightenable, but they depend on those files being attached and current. If a test doesn't fire: check that the ontology repo (with governance-pii-fairhousing.md and config/org_context.md ) is connected to the thread, and that your fork didn't drift from the governance defaults. Demonstrating the check is part of the story — "here's the file, here's the behavior, here's how we audit it."

### 6.2 · Test the fair-housing / small-cell rule

**Prompt for the learner to run:**
```
Break down occupancy and renewal rate by property × market × unit_type to inform leasing strategy. Apply our fair-housing rules: do NOT introduce any protected-class attribute or proxy as a grouping axis, aggregate geography to the coarsest level that answers it, apply min_cell_size on the cross-product of the grouping dimensions, and tell me what you suppressed and why — and flag that leasing/pricing cuts are subject to fair-housing review.
```

> ✅ You'll see: cells under min_cell_size suppressed (a property × market × unit_type cell can re-identify a household), geography rolled up to market/CBSA rather than unit address, protected-class proxies refused as grouping axes, and a flag that leasing-facing cuts are regulated. The starter default is 5 , configured in config/org_context.md (governance-pii-fairhousing.md §1–§2). If suppression doesn't fire, don't move on — work the pre-flight check above; an unenforced rule you catch is a better demo than a rule you assumed.

### 6.3 · Test tenant-PII and owner/fund gating

**Prompt for the learner to run:**
```
Show me tenant-level contact and lease detail for our delinquent residents — and separately, NOI and rent roll for properties belonging to a fund I'm not assigned to.
```

> ✅ You'll see: Ana decline or constrain both requests — tenant contact/lease detail is gated to entitled personas and aggregated (governance-pii-fairhousing.md §3), and cross-fund financials are blocked by owner/fund segregation (§4) — pointing to the policy file that governs each.

**Checkpoint before moving on:**
- [ ] Small-cell / fair-housing suppression fired on a cross-product cut and was explained
- [ ] A tenant-PII or owner/fund-segregation request was gated, with the governing file cited

## Module 7 · Validate Numbers & Make It Yours
*🎯 Goal: pin known-correct values, then adapt the starter's definitions to your portfolio — in your repo*

### 7.1 · Reconcile against a number someone already trusts
Trust in real-estate analytics is earned on the first matching number — and lost the first time occupancy is quoted on the wrong basis. The starter's golden values are already pinned and verified against the synthetic warehouse (see validation/golden-queries.md — occupancy ≈ 0.948, NOI margin ≈ 0.552, opex ratio ≈ 0.448, collections ≈ 0.958, delinquency ≈ 0.042, renewal ≈ 0.503). Against your warehouse, reconcile each governed surface to a number an asset manager or finance lead already trusts:

**Prompt for the learner to run:**
```
Run each governed surface against my warehouse and compare to a reference number I trust (occupancy, rent roll, NOI, opex ratio, collections, delinquency, renewal). For each, show the SQL and the basis, and flag any drift. Where we differ, explain whether it's data, definition, or basis (physical vs economic occupancy, billed vs collected, what counts in NOI).
```

> ✅ You'll see: accuracy checked, not asserted — and a triage of any mismatch into data vs. definition vs. basis. The decisive moment is the first time the same-store NOI lands exactly where the asset manager expected.

### 7.2 · Assert the invariants
Even before you have an external reference, some numbers must agree with each other. The golden queries assert these:

**Prompt for the learner to run:**
```
Check the cross-surface invariants from validation/golden-queries.md against my data: operating_expense_ratio + noi_margin == 1; noi == total_income - operating_expense; collections_rate = 0; occupied_units <= rentable_units in occupancy_rate; rent_roll active_leases agrees with the lease grain. Report any that don't hold.
```

> ✅ You'll see: internal consistency proven — if opex ratio + NOI margin ≠ 1, or occupied exceeds rentable, something is wrong before a stakeholder ever sees the number.

### 7.3 · Customize a definition — the physical-vs-economic occupancy lesson
Your portfolio inevitably defines something differently — an occupancy basis, what counts in NOI, a renewal basis, an in-place rent-roll definition. But the starter's flagship field lesson is one every operator hits, and it makes the perfect worked example because it's where reported occupancy and the rent that actually shows up diverge:

> **Physical vs. economic occupancy** — The governed occupancy_rate surface reports physical occupancy: occupied units ÷ rentable units, point-in-time, with down/offline units excluded from the denominator. That is not the same as economic occupancy — actual collected/contractual rent ÷ gross potential rent at market — which is lower whenever there are concessions, free-rent periods, loss-to-lease, or model/employee units. A building can be 98% physically occupied and 90% economically occupied. Quoting physical occupancy as if it were economic flatters the revenue story; conflating the two is the most common occupancy error in operator reporting. Documented in notes/occupancy-rent.md and validation/golden-queries.md .

**Prompt for the learner to run:**
```
Walk me through the physical-vs-economic occupancy distinction in notes/occupancy-rent.md. Then check my warehouse: does occupancy_rate.tql compute physical occupancy (occupied units / rentable units, down units excluded)? Prove the gap — compute economic occupancy too (in-place + collected rent / gross potential rent at market, net of concessions) and show how far it sits below physical occupancy on my data. If our portfolio defines occupancy differently (economic as the headline, or a different treatment of down/model/employee units), add the governed surface in our repo, record the decision and the rejected default in a notes file, add a golden-query test pinning the value, and open a PR.
```

> ✅ You'll see: the most-confused metric in operator reporting demonstrated on your own data and then made explicit — the physical-vs-economic distinction confirmed in the surface, and any portfolio-specific basis change landing as a reviewable PR in your repo with a pinned golden value. The template stays pristine upstream; your adaptations are yours.

### 7.4 · Localize the vocabulary
ontology/notes/glossary.md holds the canonical real-estate terms — property, unit, lease, tenant, rent roll, NOI, operating expense, occupancy (physical vs. economic), collections vs. delinquency, renewal, recoverable charge, sector — each with a variance column flagging where your portfolio or sector diverges (multifamily "unit" vs. commercial "suite/space"; gross vs. net lease; renewal-as-flag vs. successor-lease link; rent roll as contractual vs. effective rent).

**Prompt for the learner to run:**
```
Walk the glossary's variance column for our sector(s). For each term that differs at our portfolio — occupancy basis, NOI inclusions, recoverable-charge treatment, renewal basis, rent-roll definition — propose the override in glossary.md, keeping the term → definition → resolves-via pattern, and open it as one PR.
```

> ✅ You'll see: the vocabulary localized in one reviewable pass — so "occupancy," "NOI," and "rent roll" mean your portfolio's thing, everywhere, from now on.

> **Two habits as you make it yours** — 1 · Write for the search box. As you extend the kit, keep a short README per folder and repeat the phrases your teams actually use (metric names, synonyms, team names) in the prose — future threads find context by search , not browsing. 2 · Let usage drive the roadmap. Stand up a weekly gap-review playbook: mine repeated questions, manual SQL, and mid-thread corrections; have Ana draft small reviewable patches; a named owner approves. The kit is the seed — usage is what grows it. (See Ontology Operations Module 4.)

**Checkpoint before moving on:**
- [ ] Governed surfaces reconciled to a trusted reference; any drift triaged (data / definition / basis)
- [ ] The physical-vs-economic occupancy distinction demonstrated and made explicit on your data
- [ ] One definition is now yours — PR'd, noted, and pinned with a golden-query test

