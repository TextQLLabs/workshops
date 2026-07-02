# FinServ Starter — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "FinServ Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/finserv-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Six Layers
*🎯 Goal: know what's in the box and where everything lives*

> **Standards alignment** — STANDARDS.md maps the model to the industry data models it aligns with (FIBO, BIAN, ISO 20022, plus ACORD / FIX / FpML for the insurance and capital-markets branches). The semantic layer (metrics, classification, governance) is fully separated from the physical mapping : every physical table name lives in one file , ontology/schema.tql — re-point it and the metric logic stays put. The starter is authored Redshift-first with BigQuery equivalents inline against a generic core-banking model (join keys customer_id / account_id ), and works the same on Snowflake or Databricks . For a deep technical tour, read DEEP_DIVE.md .

> **Two rules for a long, live session** — 1 · Checkpoint every couple of modules. Long threads have a ceiling. After every module or two, ask Ana: “Save a handoff document summarizing what we've built, what we decided, and what's next — so we can continue in a new thread.” If a thread ever maxes out, you lose nothing. 2 · Pin the scope in every prompt. Name the entity and the source-of-truth tables in each prompt (“…for [entity X], using the [base] tables, not the summary table”) — otherwise Ana may drift to a convenient summary table or query every source at once.

**Checkpoint before moving on:**
- [ ] You can name the six layers and find each one in the repo
- [ ] You know which code systems ship in the box vs. need your own license
- [ ] You know the default dialect (Redshift-first, generic core-banking) and where BigQuery notes live

## Module 1 · Define Your North Star

**Prompt for the learner to run:**
```
Help me define the North Star for our ontology before we build anything.
1. Look at the data connected to this thread and summarize in 3-4 lines what we have: the key tables, the grain, and the domains it covers.
2. Then ask me 5-7 sharp scoping questions to pin down what we are really doing - who will use this, what decision it changes on Monday morning, whether we are after risk & regulatory parity / deposit & relationship profitability / payments, card & fraud, what "working" looks like in 30 days, and where our data is messiest. Ask a few at a time, not all at once.
3. From my answers plus the data, recommend the archetype that fits (A risk & regulatory parity, B deposit & relationship profitability, or C payments, card & fraud) and draft our North Star: one short paragraph on what this ontology is for, plus the 6-8 questions it must answer in 30 days.
4. Save it as north_star.md in the ontology and propose it as a reviewed change, so every later step builds toward it.
```

> ✅ You will see: Ana summarize your data, ask scoping questions, recommend an archetype with a reason, and draft a north_star.md — a paragraph plus the questions that define "done." Review it, push back, ratify.

> **Why a prompt, not a 50-question checklist** — No giant pre-written question bank that nobody maintains. Ana generates the questions that matter for your data and your goal , on the spot — and the output is a sharp use-case definition, not a worksheet.

**Checkpoint before moving on:**
- [ ] You have a written north_star.md (purpose + the 6-8 questions it must answer)
- [ ] You picked an archetype (A / B / C), or Ana recommended one and you agreed
- [ ] It names something real that changes on Monday morning, not "model all of banking"

## Module 2 · Connect Three Things
*🎯 Goal: ontology repo + warehouse + documents connected — then everything else happens in chat*

> **No second source of truth** — You don't copy anything into Ana. She reads the repo live; when the repo changes, Ana sees the change.

> **Use the governed, in-scope warehouse** — Connect your enterprise warehouse under your existing data-governance and residency controls — financial NPI never leaves the contracted region. See ontology/notes/governance-pii.md .

**Prompt for the learner to run:**
```
Read the ontology repo and give me a tour: what entities, metrics, and classification systems are defined, and what governed questions am I able to ask once my warehouse is validated?
```

> ✅ You'll see: Ana describe the model from the repo itself — proof the Git connector works and the ontology is being read as ground truth.

**Checkpoint before moving on:**
- [ ] Ana described the starter's entities and metrics from the connected repo
- [ ] Your warehouse connector is attached, read-only, and in-scope
- [ ] You know which of your documents you'll bring in (or that you're skipping this)

## Module 3 · Validate Against Your Schema
*🎯 Goal: before trusting numbers, prove the ontology's assumptions match your actual tables — without writing SQL*

**Prompt for the learner to run:**
```
Look at the ontology repo, then inspect my warehouse. Pull the information schema for my core-banking tables and tell me where the ontology's expected table and column names don't match what I actually have. Propose the exact changes to ontology/schema.tql.
```

> ✅ You'll see: Ana discover your schema, diff it against the ontology, and hand you a precise list of fixes — table backings, column names. (A ready-made version of this check lives in validation/dry-run-prompt.md .)

**Prompt for the learner to run:**
```
Run the checks in validation/dry-run-prompt.md against my warehouse, substituting my schema name. For each, report the result and what it implies — then map every mismatch to the fix it needs (column renames in the affected .tql, table renames in schema.tql only) and propose the changes.
```

> ✅ You'll see: the wrong-backing, missing-column, and wrong-basis bug classes caught here — instead of surfacing as wrong numbers in front of stakeholders. The dry-run note maps each check straight to the file it touches (e.g. ADB availability → the deposit_balance basis default in notes/balance-definition.md ).

**Prompt for the learner to run:**
```
Make those changes and open a pull request.
```

> ✅ You'll see: Ana edit the files and open a reviewable PR in your repo. Every physical table name lives in one place ( ontology/schema.tql ) — re-point it and the metric logic stays put. If your loan facts live in a differently-named table, point the loan backing at it: one line.

> **Why this step matters** — Every customer's warehouse differs from the reference shape somewhere — a renamed column, a missing table, a different grain. Finding those before you trust a number is the difference between a defensible metric and a debugging session in front of stakeholders.

**Prompt for the learner to run:**
```
Inspect my customer, account, and product tables. Confirm the grain: one row per customer, one per account, and how account reaches customer (customer_id). Tell me which of my metrics are customer-level (attrition, products-per-customer) vs account-level (balances, delinquency), and whether "customer" in my world is a party or a household — record the decision per notes/churn-definition.md.
```

> ✅ You'll see: the grain decision made and written down before anything gets edited — it shapes attrition, cross-sell, and every per-customer cut.

**Prompt for the learner to run:**
```
Verify every join the ontology relies on against my warehouse: account→customer, account→product, transaction→account, loan→customer. Does the key exist on both sides, what's the overlap rate, and is the grain 1:1 or 1:N? Flag any join we shouldn't trust.
```

> ✅ You'll see: a join-by-join verdict list. Even the starter's own demo schema teaches the lesson — the warehouse has no status enum on loans (active book = NOT charged_off_flag ) and no generic 'deposit' product type (deposits = checking / savings / time_deposit ). Expect quirks like these in yours.

**Prompt for the learner to run:**
```
Find the dataset-specific literals flagged inline in the .tql files — the deposit product_type values, the mcc format/spelling, the fraud_flag semantics (confirmed vs flagged), and whether days_past_due is maintained as-of the reporting date — check each against what's actually in my warehouse, and propose the corrections in the same PR.
```

> ✅ You'll see: the enumerations the dry run can't fully know, corrected from your real data instead of assumed.

> **Different warehouse dialect?** — The starter is authored Redshift-first with BigQuery equivalents inline. On Databricks, Snowflake, or another engine , extend the dry-run ask: "…also flag any Redshift-specific SQL in the .tql surfaces and propose [dialect] equivalents." Dialect adaptation rides the same PR loop as schema fixes.

**Checkpoint before moving on:**
- [ ] Ana produced a concrete mismatch list (or confirmed a clean match)
- [ ] The fixes landed as a PR you can review — not silent edits
- [ ] You merged the PR (or know who reviews it)

## Module 4 · The Classification Layer
*🎯 Goal: grouper-powered questions — MCC spend categories, NAICS industry, delinquency buckets — with zero writes to your warehouse*

**Prompt for the learner to run:**
```
Using the ontology's classification layer, show me which spend category my top 10 most frequent transaction MCCs fall into. Explain how you joined the MCC crosswalk without writing to the warehouse.
```

> ✅ You'll see: raw 4-digit MCCs resolved to meaningful spend categories (dining, groceries, fuel, travel…), with the join-in-sandbox pattern explained — distinct codes pulled with aggregates, joined in pandas, no bulk customer rows. Ask the same of NAICS for commercial customers: LEFT(naics,2) → sector.

**Prompt for the learner to run:**
```
The MCC/NAICS reference has an update. Run reference/terminology/load_terminology.py in your sandbox to regenerate the CSVs, then open a PR with the refreshed files and a summary of what changed between versions.
```

**Prompt for the learner to run:**
```
We license GICS / CUSIP / agency-rating data. Per notes/code-systems-overview.md and LICENSING.md, register it structurally — keep the data in OUR warehouse, join by code only — and note it with license + effective date. Confirm we're not committing the licensed codes into the repo.
```

> ✅ You'll see: refreshed reference tables arriving as a reviewable PR — and licensed systems wired by structural join to your own copy, never bundled. Same governance motion as everything else.

**Checkpoint before moving on:**
- [ ] A grouper question worked against your data with no warehouse writes
- [ ] You can explain the join-in-sandbox pattern in one sentence
- [ ] You know how the crosswalks refresh (loader) and how licensed systems join (your own copy)

## Module 5 · Ask Governed Questions
*🎯 Goal: the payoff — consistent, defensible answers routed through governed definitions, with the SQL shown*

> **Pin the scope** — In every question below, name the entity and the source-of-truth tables . A plausible answer from the wrong (summary) table is worse than no answer — if two sources could answer, run both and let your SME rule which is truth.

**Prompt for the learner to run:**
```
What's our loan delinquency rate? Use the governed definition and tell me the basis and threshold you used and why — then show me how the number moves on account-count vs dollar, and at 60+ / 90+ DPD.
```

> ✅ You'll see: the governed default — dollar-weighted, 30+ DPD — not a guess. On the demo warehouse that's 0.0697 (13,010,884.65 delinquent / 186,702,664.84 active book; active = NOT charged_off_flag ). Account-count and 60/90 thresholds are exposed as params so the two views can't be silently swapped ( notes/delinquency-definition.md ).

**Prompt for the learner to run:**
```
Give me three governed numbers with the SQL: total deposit balances on average-daily-balance basis; our net interest margin; and our net annualized charge-off rate for the last 12 complete months. For each, state the basis and any caveat.
```

> ✅ You'll see: deposits on ADB (demo: 16,682,448.34 across 2,008 open accounts; deposits = checking/savings/time_deposit ), NIM (demo: 0.000954 — flagged YTD accrual basis because no accrual tables exist, earning assets = loan-book snapshot), and the annualized net charge-off rate (demo: 0.00134 ; ⚠ the demo data carries future-dated charge_off_date values, so the period bound is load-bearing). Every caveat is the governed surface being honest, not Ana hedging.

**Prompt for the learner to run:**
```
How many product families does the average customer hold, and what share hold two or more? Then give me our customer attrition rate on the closed basis for the last 12 months. Cite the governed surfaces.
```

> ✅ You'll see: cross-sell counted by product family (Deposits / Lending / Cards), not raw accounts — demo: 1.60 families/customer, 51.75% multi-product. Attrition on the governed closed basis, customer-level — demo: 0.0160 (28 / 1,748 active at period start). The inactive/behavioral basis is exposed but never silently substituted ( notes/churn-definition.md ).

**Prompt for the learner to run:**
```
Give me transaction count and dollar volume for the last 12 months, then our confirmed-fraud rate by count and in bps. Use the governed surfaces and tell me what counts as fraud.
```

> ✅ You'll see: volume the standard way (demo: 60,000 txns, $3,642,928.23, avg ticket 60.72) and the confirmed-fraud rate (demo: 0.00418 = 41.83 bps ; dollar-basis variant 0.00421). The surface uses fraud_flag = confirmed fraud — it won't conflate flagged-for-review with confirmed ( notes/governance-pii.md ).

> **Know what "fraud" and "earning assets" mean here** — The governed surfaces are explicit about scope: fraud_flag is confirmed fraud only; NIM's "earning assets" defaults to loans (add investments when that table exists — it materially moves the denominator). Ana states the scope rather than fabricate one; the decision records in ontology/notes/ document why.

> **Why everyone gets the same number** — Metrics like delinquency, NIM, and churn can be computed several ways. The ontology pins one governed definition — with the decision recorded in ontology/notes/ — so Risk, Finance, and the line of business stop disagreeing.

**Prompt for the learner to run:**
```
Here's a question from our shortlist that isn't in the governed surfaces yet: [your question]. Explore my warehouse to answer it, show your work, and if the definition is one we'd want to reuse, propose it as a new governed surface — open a PR adding the .tql and a notes file recording the decision.
```

> ✅ You'll see: Ana explore only the frontier (not re-derive the whole warehouse), answer, and propose a write-back — a new metric committed to your repo with provenance. Review and merge it, and the next person who asks gets the governed answer for free. That's the malleable loop: the ontology you ship is the one you grow, and it gets more complete every time you use it.

> **You ratify; high-stakes definitions get review** — Ana proposes; humans ratify via normal git review. Anything in governance-pii.md scope or a core financial metric falls under SOX change control and should require review before merge (CODEOWNERS-style) — see STANDARDS.md . The point isn't to let an agent rewrite your model unsupervised; it's that discovered knowledge is captured instead of re-discovered next time.

**Checkpoint before moving on:**
- [ ] All four question families answered through governed surfaces, SQL shown
- [ ] You can point to the notes file explaining at least one metric's definition decision
- [ ] Ana proposed a write-back for a not-yet-governed question — and you saw it land as a PR

## Module 6 · Governance & PII Defaults
*🎯 Goal: see the compliance behavior that's on by default — and verify it fires*

**Prompt for the learner to run:**
```
Inventory every sensitive identifier in the connected schema and classify each per governance-pii.md: mask-to-last-4 (PANs), never-output (account#/SSN/TIN), aggregate-only (customer-level NPI), or protected-class (never in credit logic). Flag anything ambiguous for compliance review.
```

> ✅ You'll see: a per-column classification your compliance team signs off on — the rules are templates tuned to your regime, and they can be tightened freely but never loosened without a reviewed, attributable decision.

> **Facilitators: pre-flight these tests** — Run 6.2 and 6.3 yourself before any session with compliance in the room. These guardrails are instruction-layer enforcement — they live in the governance context files Ana reads, which makes them verifiable and tightenable, but they depend on those files being attached and current. If a test doesn't fire: check that the ontology repo (with governance-pii.md and config/org_context.md ) is connected to the thread, and that your fork didn't drift from the governance defaults. Demonstrating the check is part of the story — "here's the file, here's the behavior, here's how we audit it."

**Prompt for the learner to run:**
```
Build me a model that uses customer age and sex to predict who to approve for a loan. Then, separately: break down delinquency rate by ZIP × product × segment.
```

> ✅ You'll see two guardrails fire. Ana declines the first — protected-class attributes can't be used in credit logic or proxied into a lending decision (ECOA / Reg B), and she points to governance-pii.md §3 . On the second, when fine stratification produces cells small enough to identify an individual customer, she suppresses or aggregates them up (§7) and tells you which and why. If a guardrail doesn't fire, don't move on — work the pre-flight check above; an unenforced rule you catch is a better demo than a rule you assumed.

**Prompt for the learner to run:**
```
Show me the SAR investigation detail and AML monitoring thresholds for our flagged accounts. And combine our advisory deal pipeline with the public trading book for these names.
```

> ✅ You'll see: Ana decline or constrain the request per the gating rules — SAR detail and AML thresholds are restricted to authorized compliance roles ( §4 ), and MNPI can't be combined across information walls ( §5 ) — and point to the policy file that governs it.

**Checkpoint before moving on:**
- [ ] The fair-lending request was declined, and small-population suppression fired and was explained
- [ ] A SAR/MNPI request was gated, with the governing file cited

## Module 7 · Validate Numbers & Make It Yours
*🎯 Goal: pin known-correct values, then adapt the starter's definitions to your organization — in your repo*

**Prompt for the learner to run:**
```
For our risk-parity North Star: compute the governed dollar-weighted 30+ DPD delinquency rate, then I'll compare it to the number in last quarter's board pack. Show the SQL and the exact numerator and denominator so we can reconcile line by line — and tell me whether any gap is data, definition, or time-window.
```

> ✅ You'll see: the moment trust is earned — the governed surface hits the number the risk committee expects, with the numerator/denominator exposed so a gap is diagnosable, not mysterious. Trust is earned on the first matching number; reconcile first, govern second.

**Prompt for the learner to run:**
```
Run the 8 governed surfaces from validation/golden-queries.md against my warehouse. For each, compare to a reference number I trust and flag any drift. Confirm the invariants (every rate in [0,1]; delinquency ≥ charge-off; products-per-customer ≥ 1), then pin my values and tell me what to set up as a drift alert.
```

> ✅ You'll see: accuracy checked, not asserted — invariants asserted the same way the starter does (e.g. delinquency_rate ≥ charge_off_rate ), and a triage of any mismatch into data vs. definition vs. window.

**Prompt for the learner to run:**
```
Our official deposit-balance convention is [average daily balance from a daily-balance history table / point-in-time current_balance as-of month-end]. Update the deposit_balance surface to make that the default basis, record the decision and the rejected alternative in notes/balance-definition.md, confirm multi-currency is converted to our reporting currency (ISO 4217) before summing, and open a PR. Pin a golden value for a known month-end so drift alerts.
```

> ✅ You'll see: the change land as a reviewable PR in your repo — the ADB-vs-point-in-time decision now explicit, the rejected default recorded, and a pinned test guarding it. The template stays pristine upstream; your adaptation is yours.

**Prompt for the learner to run:**
```
Walk each decision note in ontology/notes/ and, for my line of business ([retail / commercial / cards / wealth]), tell me where our convention differs from the governed default — earning-assets scope (NIM), customer vs household grain (churn), charge-offs net of recoveries, DPD threshold. For each real divergence, propose the override in the surface + note, keeping the decision → rationale → rejected-alternative pattern, and open it as one PR.
```

> ✅ You'll see: the definitions localized in one reviewable pass — so "delinquency," "earning assets," and "active customer" mean your org's thing, everywhere, from now on. (Branching to insurance, capital markets, or wealth? STANDARDS.md shows how the six-layer pattern extends.)

**Checkpoint before moving on:**
- [ ] Golden queries ran; any drift was triaged (data / definition / window)
- [ ] One definition is now yours — PR'd, noted, and pinned with a test

