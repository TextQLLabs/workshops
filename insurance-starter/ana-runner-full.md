# P&C Insurance Starter — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "P&C Insurance Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/insurance-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Six Layers
*🎯 Goal: know what's in the box and where everything lives*

### The six layers

> **Standards alignment** — STANDARDS.md maps the model to the industry standards it aligns with (ACORD, NAIC annual statement, FIBO, ISO statistical plans, SAP/GAAP). The semantic layer (metrics, routing, classification) is fully separated from the physical mapping : every physical table name lives in one file , ontology/schema.tql — re-point it and the metric logic stays put. The starter is authored against a generic policy-admin + claims model with ANSI/Spark-portable SQL; MIGRATION.md is the 8-step re-point checklist and works the same on Redshift, BigQuery, Snowflake, or Databricks (budget: about a half-day with warehouse access). For a deep technical tour, read DEEP_DIVE.md .

> **Two rules for a long, live session** — 1 · Checkpoint every couple of modules. Long threads have a ceiling. After every module or two, ask Ana: “Save a handoff document summarizing what we've built, what we decided, and what's next — so we can continue in a new thread.” If a thread ever maxes out, you lose nothing. 2 · Pin the scope in every prompt. Name the entity and the source-of-truth tables in each prompt (“…for [entity X], using the [base] tables, not the summary table”) — otherwise Ana may drift to a convenient summary table or query every source at once.

**Checkpoint before moving on:**
- [ ] You can name the six layers and find each one in the repo
- [ ] You know which classification ships in the box vs. needs your own licensed feed (ISO/PCS)
- [ ] You know the default model (generic policy-admin + claims, ANSI/Spark-portable) and where re-point lives (schema.tql)

## Module 1 · Define Your North Star

**Prompt for the learner to run:**
```
Help me define the North Star for our ontology before we build anything.
1. Look at the data connected to this thread and summarize in 3-4 lines what we have: the key tables, the grain, and the domains it covers.
2. Then ask me 5-7 sharp scoping questions to pin down what we are really doing - who will use this, what decision it changes on Monday morning, whether we are after loss-ratio/reserving parity / underwriting and pricing / claims operations, what "working" looks like in 30 days, and where our data is messiest. Ask a few at a time, not all at once.
3. From my answers plus the data, recommend the archetype that fits (A loss-ratio/reserving parity, B underwriting/pricing, or C claims operations) and draft our North Star: one short paragraph on what this ontology is for, plus the 6-8 questions it must answer in 30 days.
4. Save it as north_star.md in the ontology and propose it as a reviewed change, so every later step builds toward it.
```

> ✅ You will see: Ana summarize your data, ask scoping questions, recommend an archetype with a reason, and draft a north_star.md — a paragraph plus the questions that define "done." Review it, push back, ratify.

> **Why a prompt, not a 50-question checklist** — No giant pre-written question bank that nobody maintains. Ana generates the questions that matter for your data and your goal , on the spot — and the output is a sharp use-case definition, not a worksheet.

**Checkpoint before moving on:**
- [ ] You have a written north_star.md (purpose + the 6-8 questions it must answer)
- [ ] You picked an archetype (A / B / C), or Ana recommended one and you agreed
- [ ] It names something real that changes on Monday morning, not "model all of P&C insurance"

## Module 2 · Connect Three Things
*🎯 Goal: ontology repo + warehouse + documents connected — then everything else happens in chat*

### 2.1 · Connect the ontology repo to Ana
This is the key step. In TextQL, add a Git connector and point it at your fork of the starter repo ( TextQLLabs/ontology-starter-kits/tree/main/insurance — no fork yet? Ask your TextQL contact; it takes minutes). Because the ontology is git-backed, Ana now has the entire model — every metric definition, every note, every classification rule — as a reference she reads on demand.

> **No second source of truth** — You don't copy anything into Ana. She reads the repo live; when the repo changes, Ana sees the change.

### 2.2 · Connect your data warehouse
Add the connector for the warehouse holding your policy-admin / claims / actuarial data (Redshift, BigQuery, Snowflake, Databricks, …). Read-only access is enough.

> **Use your governed, contracted warehouse** — Insurance is state-regulated (NAIC model laws / state DOIs) and life/health claims carry medical PII. Connect the enterprise warehouse that's already in scope for your data-residency and audit obligations — see ontology/notes/governance-pii.md .

### 2.3 · (Optional) Bring in your documents
Your real-world context — the actuarial reserving workbook, a rating manual, dbt models, the spreadsheet where someone defined "in force" — often lives in messy files. Upload them in chat, connect Google Drive, or connect SharePoint/OneDrive. Ana reads them alongside the ontology as corpus, not migration , and can fold what she learns into the model.

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
Look at the ontology repo, then inspect my warehouse. Run validation/dry-run-prompt.md against my schema: pull the information schema for my policy, premium, and claims tables and tell me where the ontology's expected table and column names don't match what I actually have — including whether premium is stored earned (a monthly series) or only written at inception. Propose the exact changes to ontology/schema.tql.
```

> ✅ You'll see: Ana discover your schema, diff it against the ontology, and hand you a precise list of fixes — table backings, column names, and the all-important earned-vs-written premium question. (The ready-made version lives in validation/dry-run-prompt.md .)

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

> ✅ You'll see: Ana edit the files and open a reviewable PR in your repo. Every physical table name lives in one place ( ontology/schema.tql ) — re-point it and the metric logic stays put. The join keys the surfaces rely on are policy_id , policyholder_id , and claim_id .

> **Why this step matters** — Every carrier's warehouse differs from the reference shape somewhere — a renamed column, a missing table, a different grain. Finding those before you trust a number is the difference between a defensible loss ratio and a debugging session in front of the actuary.

### 3.4 · Decide your grain & verify your joins — required
Insurance data fans out across several grains — policy vs. coverage vs. claim vs. transaction — and joining across them carelessly multiplies counts. Settling this is the single most important decision in this module, and it's a prompt:

**Prompt for the learner to run:**
```
Read ontology/notes/grain.md, then inspect my tables. Confirm the grain of each: is there one policy table and a separate coverage table (many coverages per policy)? Is premium stored as a monthly earned series or only as written-at-inception? Does the claim table carry one row per claim, with claim_transaction as the payment/reserve ledger? Tell me where counting coverage rows as policies, or claim_transaction rows as claims, would fan out — and record the decision in databases/[ourschema]/README.md (copy databases/policy_core/ as the template).
```

> ✅ You'll see: the grain decision made and written down before anything gets edited — it shapes every count downstream. The trap to confirm: a policy fans out into many coverage lines, and a claim into many transactions; anchor policy counts on policy and claim counts on distinct claim_id .

**Prompt for the learner to run:**
```
Verify every join the ontology relies on against my warehouse: does the key (policy_id / policyholder_id / claim_id) exist on both sides, what's the overlap rate, and is the grain 1:1 or 1:N? Then confirm the bases: is "incurred" available as paid_loss + case_reserve, and is loss_date (accident date) populated separately from report_date? Record each verdict in databases/[ourschema]/README.md and flag any join or basis we shouldn't trust.
```

> ✅ You'll see: a join-by-join verdict list plus a basis check — because loss ratio needs incurred losses and accident-year dating, and a paid-only or report-date warehouse changes the number.

**Prompt for the learner to run:**
```
Find the dataset-specific literals the surfaces hard-code — the policy/claim status enums, the renewal_status values, line_of_business spelling, and the cause_of_loss values — check each against what's actually in my warehouse, and propose the corrections in the same PR.
```

> ✅ You'll see: the enumerations the validator can't know — status = 'open' , the renewal flag, the LOB and cause-of-loss spellings — corrected from your real data instead of assumed.

> **The full checklist** — This module covers the core; MIGRATION.md is the complete 8-step re-point (discover → grain → schema.tql → identity → validator → literals → governance → glossary → goldens). Half a day with warehouse access; most of it is verification, not editing.

> **Written-only premium?** — If your warehouse stores premium only as written-at-inception (no monthly earned series), the ratios can't sum earned directly — you must earn it pro-rata over effective_date → expiration_date first. Flag it in the dry run; notes/premium-definition.md has the derivation. This is the difference between a correct loss ratio and one that flatters a growing book.

**Checkpoint before moving on:**
- [ ] Ana produced a concrete mismatch list (or confirmed a clean match)
- [ ] The policy/coverage/claim/transaction grain is decided and written down
- [ ] The fixes landed as a PR you can review — not silent edits — and you merged it (or know who reviews it)

## Module 4 · The Classification Layer
*🎯 Goal: rollup-powered questions — NAIC line of business, peril groups — with zero writes to your warehouse*

### 4.1 · Prove the rollups work

**Prompt for the learner to run:**
```
Using the ontology's classification layer, roll my granular line-of-business values up to the NAIC major line, and group my top 10 most frequent cause-of-loss codes into peril groups (with the catastrophe flag). Explain how you joined the seed CSVs without writing to the warehouse.
```

> ✅ You'll see: raw product and cause-of-loss codes resolved to meaningful NAIC lines and peril groups, with the federated join-in-sandbox pattern explained — and a reminder to analyze peril groups , never free-text cause-of-loss.

### 4.2 · Bring in your licensed feed — optional
You don't need this on day one — the public NAIC and peril groupings are already committed. Come back when you want the granular ISO peril / cause-of-loss codes , PCS catastrophe codes , or ISO rating territories . These are licensed (Verisk/ISO) — the repo ships the structure and join logic, and the data comes from your carrier's licensed feed :

**Prompt for the learner to run:**
```
We have an ISO cause-of-loss / PCS catastrophe feed in our warehouse at [table]. Per LICENSING.md, join it to claim.cause_of_loss by code so we get the granular peril and per-claim cat tag — keep the licensed data in our warehouse, commit only the join logic, and note the license + effective date in LICENSING.md. Open it as a PR.
```

> ✅ You'll see: the structural model light up with your licensed data — the modeling logic versioned in git, the licensed codes staying in your warehouse, same governance motion as everything else.

**Checkpoint before moving on:**
- [ ] A NAIC-line and peril-group rollup worked against your data with no warehouse writes
- [ ] You can explain the federated join-in-sandbox pattern in one sentence
- [ ] You know which classification ships public (NAIC/peril) vs. licensed (ISO/PCS, from your feed)

## Module 5 · Ask Governed Questions
*🎯 Goal: the payoff — consistent, defensible answers routed through governed definitions, with the SQL shown*

> **Pin the scope** — In every question below, name the entity and the source-of-truth tables . A plausible answer from the wrong (summary) table is worse than no answer — if two sources could answer, run both and let your SME rule which is truth.

### 5.1 · Loss ratio (the headline metric)

**Prompt for the learner to run:**
```
What's our loss ratio for accident year 2024? Use the governed definition (loss_ratio.tql) and tell me the basis — incurred or paid, earned or written premium, accident or calendar year — and why.
```

> ✅ You'll see: the governed surface return incurred losses (paid + case reserve) over earned premium, accident-year — the synthetic book pins $94,860,400 / $173,458,746 = 0.5469 . Ana names the basis instead of silently picking one; the paid basis runs separately and comes in lower (it excludes case reserves).

### 5.2 · Combined ratio

**Prompt for the learner to run:**
```
Decompose our combined ratio for 2024 into loss ratio + expense ratio, and tell me whether we're underwriting at a profit. Use combined_ratio.tql.
```

> ✅ You'll see: the combined ratio as loss + expense — synthetic 0.5469 + 0.2500 = 0.7969 , comfortably under 100% (an underwriting profit). The invariant combined == loss + expense is asserted in the golden queries.

### 5.3 · Frequency and severity (the two levers)

**Prompt for the learner to run:**
```
Show me claim frequency (claims per 1,000 policies) and average claim severity (incurred per claim) for 2024. Use claim_frequency.tql and claim_severity.tql, report them together, and tell me which lever is driving the loss ratio.
```

> ✅ You'll see: frequency 521.17 / 1,000 (13,000 claims / 24,944 PIF) and severity $7,296.95 ($94,860,400 / 13,000) — paired, because the loss ratio alone hides which lever moved. Note frequency × severity ≈ pure premium (loss cost).

> **Know the exposure basis** — The governed claim_frequency surface uses policies-in-force as the denominator. The actuarially correct base is earned exposure (car-years, house-years, payroll) — PIF is an approximation. If your warehouse carries an earned-exposure fact, re-point the denominator; Ana will say which it used rather than imply it's earned exposure. See notes/frequency-severity.md .

### 5.4 · Reserve position

**Prompt for the learner to run:**
```
Show our reserve position for the open book — case reserves, paid-to-date, incurred, and the reserve ratio. Use reserve_position.tql and tell me explicitly whether this is case-incurred or ultimate.
```

> ✅ You'll see: the open book at case $32,340,903 · paid $12,936,361 · incurred $45,277,264 · reserve ratio 0.7143 — with Ana stating plainly that this is case-incurred, not ultimate .

> **Where's IBNR?** — The governed surface computes case-incurred (paid + case reserve) from the claim snapshot. IBNR is not in raw claims — it's a reserving-study output — so case-incurred understates ultimate for recent accident periods. Ana will say so rather than fabricate IBNR; the decision record in notes/reserve-definition.md documents the scope. If you have an actuarial ultimate/IBNR table, add it as a backing.

> **Why everyone gets the same number** — Loss ratio, combined ratio, and reserves can each be computed several ways. The ontology pins one governed definition — incurred / earned / accident-year, with the decision recorded in ontology/notes/ — so Actuarial, Underwriting, and Finance stop disagreeing about which number is "the" number.

### 5.5 · When the answer isn't governed yet — watch the model grow
Now ask something from your shortlist that the starter doesn't already cover — your earned-exposure fact, a reinsurance treaty structure, a loss-development triangle, the actuarial ultimate/IBNR table. This is the important beat: a starter pack is a head start, not the finished model.

**Prompt for the learner to run:**
```
Here's a question from our shortlist that isn't in the governed surfaces yet: [your question]. Explore my warehouse to answer it, show your work, and if the definition is one we'd want to reuse, propose it as a new governed surface — open a PR adding the .tql and a notes file recording the decision and the basis.
```

> ✅ You'll see: Ana explore only the frontier (not re-derive the whole warehouse), answer, and propose a write-back — a new metric committed to your repo with provenance. Review and merge it, and the next person who asks gets the governed answer for free. That's the malleable loop: the ontology you ship is the one you grow, and it gets more complete every time you use it.

> **You ratify; high-stakes definitions get review** — Ana proposes; humans ratify via normal git review. Anything in governance-pii.md scope (fair pricing, reserve MNPI, medical-claim sensitivity) or a core ratio surface should require review before merge (CODEOWNERS-style) — see STANDARDS.md . The point isn't to let an agent rewrite your model unsupervised; it's that discovered knowledge is captured instead of re-discovered next time.

**Checkpoint before moving on:**
- [ ] Loss ratio, combined ratio, frequency/severity, and reserves all answered through governed surfaces, SQL and basis shown
- [ ] You can point to the notes file explaining at least one metric's definition decision
- [ ] Ana proposed a write-back for a not-yet-governed question — and you saw it land as a PR

## Module 6 · Governance & PII Defaults
*🎯 Goal: see the compliance behavior that's on by default — and verify it fires*

### 6.1 · Inventory your identifiers — day one
governance-pii.md §0 classifies every direct identifier in the connected schema into exactly one role — and the key distinction is that using an identifier as a join key is not the same as outputting it :

**Prompt for the learner to run:**
```
Inventory every direct identifier in the connected schema and classify each per governance-pii.md section 0: join-key-only, never-output, sensitive, or aggregate-only. Flag anything ambiguous for compliance review.
```

> ✅ You'll see: a per-column classification your compliance team signs off on — the rules are templates tuned to your regime, and they can be tightened freely but never loosened without a reviewed, attributable decision.

> **Facilitators: pre-flight these tests** — Run 5.2 and 5.3 yourself before any session with compliance in the room. These guardrails are instruction-layer enforcement — they live in the governance context files Ana reads, which makes them verifiable and tightenable, but they depend on those files being attached and current. If a test doesn't fire: check that the ontology repo (with governance-pii.md and config/org_context.md ) is connected to the thread, and that your fork didn't drift from the governance defaults. Demonstrating the check is part of the story — "here's the file, here's the behavior, here's how we audit it."

### 6.2 · Test the fair-pricing / small-cell rule

**Prompt for the learner to run:**
```
Break down loss ratio by line of business × state × peril to inform rate adequacy. Apply our fair-pricing rules: aggregate geography to the coarsest level that answers it, apply min_cell_size on the cross-product of the grouping dimensions, and tell me what you suppressed and why — and flag that this is a rate/underwriting cut subject to filed-rate and state DOI rules.
```

> ✅ You'll see: cells under min_cell_size suppressed (a LOB × state × peril cell can re-identify a claimant), geography rolled up to territory/region rather than address, and a flag that pricing-facing cuts are regulated. The starter default is 5 , configured in config/org_context.md (governance-pii.md §1–§2). If suppression doesn't fire, don't move on — work the pre-flight check above; an unenforced rule you catch is a better demo than a rule you assumed.

### 6.3 · Test sensitive-claim and reserve gating

**Prompt for the learner to run:**
```
Show me claimant-level injury/diagnosis detail for our workers-comp claims — and separately, large-loss and reserve-adequacy detail by individual claim.
```

> ✅ You'll see: Ana decline or constrain both requests — claimant medical detail is gated to entitled personas and aggregated (governance-pii.md §3), and reserve/large-loss data is treated as potential MNPI for a public carrier (§4) — pointing to the policy file that governs each.

**Checkpoint before moving on:**
- [ ] Small-cell / fair-pricing suppression fired on a cross-product cut and was explained
- [ ] A sensitive-claim or reserve-MNPI request was gated, with the governing file cited

## Module 7 · Validate Numbers & Make It Yours
*🎯 Goal: pin known-correct values, then adapt the starter's definitions to your carrier — in your repo*

### 7.1 · Reconcile against a number someone already trusts
Trust in insurance analytics is earned on the first matching number — and lost the first time a loss ratio is quoted on the wrong basis. The starter's golden values are already pinned and verified against the synthetic warehouse (see validation/golden-queries.md — loss ratio 0.5469, combined 0.7969, reserve ratio 0.7143, retention 0.5004). Against your warehouse, reconcile each governed surface to a number an actuary or finance lead already trusts:

**Prompt for the learner to run:**
```
Run each governed surface against my warehouse and compare to a reference number I trust (loss ratio, combined ratio, reserve position, retention). For each, show the SQL and the basis, and flag any drift. Where we differ, explain whether it's data, definition, or basis (paid vs incurred, written vs earned, accident vs calendar year).
```

> ✅ You'll see: accuracy checked, not asserted — and a triage of any mismatch into data vs. definition vs. basis. The decisive moment is the first time the accident-year loss ratio lands exactly where the actuary expected.

### 7.2 · Assert the invariants
Even before you have an external reference, some numbers must agree with each other. The golden queries assert these:

**Prompt for the learner to run:**
```
Check the cross-surface invariants from validation/golden-queries.md against my data: combined_ratio == loss_ratio + expense_ratio; incurred >= paid; reserve_position.incurred == paid_to_date + case_reserves; PIF agrees between claim_frequency and policies_in_force; loss_ratio and retention land in a sane range. Report any that don't hold.
```

> ✅ You'll see: internal consistency proven — if combined ≠ loss + expense, or PIF disagrees between two surfaces, something is wrong before a stakeholder ever sees the number.

### 7.3 · Customize a definition — the written-vs-earned premium lesson
Your carrier inevitably defines something differently — a loss-ratio basis (net vs. gross, with or without LAE), a retention basis, an in-force definition. But the starter's flagship field lesson is one every carrier hits, and it makes the perfect worked example because it's a real bug that was found and fixed in this very repo:

> **The ~12× premium-inflation bug** — The reference model pre-earns premium into a monthly premium_earned fact — one row per policy per month. Written premium is booked once at inception for the whole term, so it repeats across every one of those monthly rows. An early version of earned_premium did SUM(written_premium) off that monthly series — which inflated written premium ~12× (roughly one duplicate per month of term). The fix: report earned premium (sum the monthly earned series — that is correct, it recognizes over time) and pull written premium from the policy grain , never the earned series. Documented in notes/premium-definition.md and validation/golden-queries.md → Issue found & fixed .

**Prompt for the learner to run:**
```
Walk me through the written-vs-earned premium distinction in notes/premium-definition.md. Then check my warehouse: is written_premium stored on the monthly premium_earned series? If so, prove the trap — show that SUM(written_premium) off the monthly fact inflates ~12x vs. written premium counted once at the policy grain. Confirm earned_premium.tql sums the EARNED series for ratios and sources WRITTEN from the policy grain. If our carrier defines the loss-ratio basis differently (net of reinsurance, w/ or w/o LAE), update the governed surface in our repo, record the decision and the rejected default in a notes file, add a golden-query test pinning the value, and open a PR.
```

> ✅ You'll see: the most expensive ratio bug in P&C demonstrated on your own data and then guarded against — the earned-vs-written discipline confirmed in the surface, and any carrier-specific basis change landing as a reviewable PR in your repo with a pinned golden value. The template stays pristine upstream; your adaptations are yours.

### 7.4 · Localize the vocabulary
ontology/notes/glossary.md holds the canonical insurance terms — policy, policyholder, coverage, written vs. earned premium, loss/combined ratio, incurred/case/IBNR/ultimate, frequency/severity, retention, PIF, line of business, peril — each with a variance column flagging where your carrier or line of business diverges (life "policy ≈ contract on a life" vs. P&C term policy; gross vs. net of reinsurance; renewal-as-flag vs. successor-policy link; PIF as bound vs. issued vs. paid).

**Prompt for the learner to run:**
```
Walk the glossary's variance column for our line(s) of business. For each term that differs at our carrier — earned-premium earning pattern, loss-ratio basis, reserve definitions, retention basis, in-force definition — propose the override in glossary.md, keeping the term → definition → resolves-via pattern, and open it as one PR.
```

> ✅ You'll see: the vocabulary localized in one reviewable pass — so "earned premium," "incurred," and "in force" mean your carrier's thing, everywhere, from now on.

> **Two habits as you make it yours** — 1 · Write for the search box. As you extend the kit, keep a short README per folder and repeat the phrases your teams actually use (metric names, synonyms, team names) in the prose — future threads find context by search , not browsing. 2 · Let usage drive the roadmap. Stand up a weekly gap-review playbook: mine repeated questions, manual SQL, and mid-thread corrections; have Ana draft small reviewable patches; a named owner approves. The kit is the seed — usage is what grows it. (See Ontology Operations Module 4.)

**Checkpoint before moving on:**
- [ ] Governed surfaces reconciled to a trusted reference; any drift triaged (data / definition / basis)
- [ ] The written-vs-earned trap demonstrated and guarded against on your data
- [ ] One definition is now yours — PR'd, noted, and pinned with a golden-query test

