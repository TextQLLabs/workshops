# Wealth & Asset Management Starter — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Wealth Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/wealth-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Six Layers
*🎯 Goal: know what's in the box and where everything lives*

> **Standards alignment** — STANDARDS.md maps the model to the industry standards it aligns with (FIBO, ISO 20022, OpenFIGI, GICS); SOURCES.md cites every source. The semantic layer (metrics, routing, classification) is fully separated from the physical mapping : every physical table name lives in one file , ontology/schema.tql — re-point it and the metric logic stays put. The starter is authored against a generic custody + portfolio-accounting model in ANSI SQL; MIGRATION.md is the 8-step re-point checklist and works the same on Redshift, BigQuery, Snowflake, or Databricks (budget: about a half-day with warehouse access). For a deep technical tour, read DEEP_DIVE.md .

> **Two rules for a long, live session** — 1 · Checkpoint every couple of modules. Long threads have a ceiling. After every module or two, ask Ana: “Save a handoff document summarizing what we've built, what we decided, and what's next — so we can continue in a new thread.” If a thread ever maxes out, you lose nothing. 2 · Pin the scope in every prompt. Name the entity and the source-of-truth tables in each prompt (“…for [entity X], using the [base] tables, not the summary table”) — otherwise Ana may drift to a convenient summary table or query every source at once.

**Checkpoint before moving on:**
- [ ] You can name the six layers and find each one in the repo
- [ ] You know which classification ships in the box vs. needs your own licensed feed (CUSIP, per-security GICS)
- [ ] You know the default model (generic custody + portfolio-accounting, ANSI SQL) and where the schema-mapping overlay lives

## Module 1 · Define Your North Star

**Prompt for the learner to run:**
```
Help me define the North Star for our ontology before we build anything.
1. Look at the data connected to this thread and summarize in 3-4 lines what we have: the key tables, the grain, and the domains it covers.
2. Then ask me 5-7 sharp scoping questions to pin down what we are really doing - who will use this, what decision it changes on Monday morning, whether we are after BI parity / portfolio risk and concentration / household and relationship rollup and suitability, what "working" looks like in 30 days, and where our data is messiest. Ask a few at a time, not all at once.
3. From my answers plus the data, recommend the archetype that fits (A BI parity, B portfolio risk & concentration, or C household/relationship rollup & suitability) and draft our North Star: one short paragraph on what this ontology is for, plus the 6-8 questions it must answer in 30 days.
4. Save it as north_star.md in the ontology and propose it as a reviewed change, so every later step builds toward it.
```

> ✅ You will see: Ana summarize your data, ask scoping questions, recommend an archetype with a reason, and draft a north_star.md — a paragraph plus the questions that define "done." Review it, push back, ratify.

> **Why a prompt, not a 50-question checklist** — No giant pre-written question bank that nobody maintains. Ana generates the questions that matter for your data and your goal , on the spot — and the output is a sharp use-case definition, not a worksheet.

**Checkpoint before moving on:**
- [ ] You have a written north_star.md (purpose + the 6-8 questions it must answer)
- [ ] You picked an archetype (A / B / C), or Ana recommended one and you agreed
- [ ] It names something real that changes on Monday morning, not "model all of wealth/asset management"

## Module 2 · Connect Three Things
*🎯 Goal: ontology repo + warehouse + documents connected — then everything else happens in chat*

> **No second source of truth** — You don't copy anything into Ana. She reads the repo live; when the repo changes, Ana sees the change.

> **Use your books-and-records warehouse** — Client financial data is sensitive PII in a regulated domain. Use your enterprise, in-region warehouse and keep data in the contracted region (SEC Rule 17a-4 books-and-records) — see ontology/notes/governance-mnpi-pii.md .

**Prompt for the learner to run:**
```
Read the ontology repo and give me a tour: what entities, metrics, and classification groupers are defined, and what governed questions am I able to ask once my warehouse is validated?
```

> ✅ You'll see: Ana describe the model from the repo itself — proof the Git connector works and the ontology is being read as ground truth.

**Checkpoint before moving on:**
- [ ] Ana described the starter's entities and metrics from the connected repo
- [ ] Your warehouse connector is attached, read-only, and in your contracted region
- [ ] You know which of your documents you'll bring in (or that you're skipping this)

## Module 3 · Validate Against Your Schema
*🎯 Goal: before trusting numbers, prove the ontology's assumptions match your actual tables — without writing SQL*

**Prompt for the learner to run:**
```
Look at the ontology repo, then inspect my warehouse. Pull the information schema for my custody / portfolio-accounting tables and tell me where the ontology's expected table and column names don't match what I actually have. Propose the exact changes to ontology/schema.tql.
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

> ✅ You'll see: Ana edit the files and open a reviewable PR in your repo. Every physical table name lives in one place ( ontology/schema.tql ) — re-point it and the metric logic stays put. If you have a resolved household table, point household_grain at it: one line.

> **Why this step matters** — Every firm's warehouse differs from the reference shape somewhere — a renamed column, a missing table, a different grain. Finding those before you trust a number is the difference between a defensible metric and a debugging session in front of stakeholders.

**Prompt for the learner to run:**
```
Read ontology/notes/grain.md, then inspect my data. Confirm: is `position` a per-date snapshot (one row per account × security × as_of_date), and is `account_value` a series? What's the snapshot cadence (daily vs month-end), and which valuation date is my horizon? Set analysis_end_date in schema.tql to a real snapshot date, and record the decision in databases/[ourschema]/README.md.
```

> ✅ You'll see: the snapshot-vs-series decision made and written down before anything gets edited — summing position snapshots across dates multiplies AUM, the #1 bug in this domain ( notes/grain.md ).

**Prompt for the learner to run:**
```
Per ontology/notes/identity-resolution.md, find my identity / household resolution layer (household / relationship / master / crosswalk tables), and verify every join the ontology relies on: does the key exist on both sides, what's the overlap rate, and is the grain 1:1 or 1:N? Point household_grain at the resolved id, record each verdict in databases/[ourschema]/README.md, and flag any join we shouldn't trust.
```

> ✅ You'll see: a join-by-join verdict list, and identity resolved before any client/AUM count — so the same investor under multiple custodian ids (post-M&A, joint/trust, multi-custodian) isn't double-counted.

**Prompt for the learner to run:**
```
Find the dataset-specific decisions the validator can't know: is net_external_flow clean (client flows only, no market movement)? Is market_value in one reporting currency? Which security identifiers are present (FIGI public; CUSIP/GICS licensed)? Check each against my warehouse and propose the corrections in the same PR.
```

> ✅ You'll see: the things a compile check can't catch — dirty flows that void returns, multi-currency books, missing FIGI — corrected from your real data instead of assumed.

> **The full checklist** — This module covers the core; MIGRATION.md is the complete 8-step re-point (discover → grain & cadence → schema.tql → identity → validator → literals → governance → glossary → goldens). Half a day with warehouse access; most of it is verification, not editing.

> **Different warehouse dialect?** — The starter is authored in ANSI SQL with a portable float idiom ( * 1.0 ) that works on Redshift, Spark/Databricks, DuckDB, and BigQuery. On any engine, extend the dry-run ask: "…also flag any engine-specific SQL in the .tql surfaces and propose [dialect] equivalents." Dialect adaptation rides the same PR loop as schema fixes.

**Checkpoint before moving on:**
- [ ] Ana produced a concrete mismatch list (or confirmed a clean match)
- [ ] The fixes landed as a PR you can review — not silent edits
- [ ] You merged the PR (or know who reviews it)

## Module 4 · The Classification Layer
*🎯 Goal: grouper-powered questions — asset-class & GICS look-through, FIGI crosswalks — with zero writes to your warehouse*

**Prompt for the learner to run:**
```
Using the ontology's classification layer, show me my firm-wide asset-class allocation, then roll the equity sleeve up to its GICS sectors. Explain how you joined the asset-class and GICS-structure seeds without writing to the warehouse.
```

> ✅ You'll see: raw instruments resolved to meaningful asset-class and sector buckets, with the join-in-sandbox pattern explained — and weights that sum to 1.000 at one snapshot date.

> **Two classification traps to name** — The identifier tuple. A security reference is original (ticker) + normalized (FIGI) + system + version , not a column ( notes/coding-tuple.md ). Join on the normalized, license-clean FIGI — never parse tickers, which are reused across venues and change on corporate actions. Look-through & version. A fund hides its underlying exposure, and GICS gets revised (the 2018 real-estate split). Decide whether allocation is top-level or looks through to underlying issuers, and name the GICS version when a number depends on it ( notes/asset-classification.md ).

**Prompt for the learner to run:**
```
Run reference/terminology/load_terminology.py in your sandbox to regenerate the asset_class and gics_structure seed CSVs, then open a PR with the refreshed files and a summary of what changed between versions.
```

**Prompt for the learner to run:**
```
Using load_terminology.py --figi with our OPENFIGI_API_KEY, fetch the OpenFIGI crosswalk for the tickers/ISINs in my security master. Confirm we're committing only the public FIGI mapping, not the licensed CUSIP/GICS codes, per LICENSING.md.
```

> ✅ You'll see: refreshed reference tables arriving as a reviewable PR — same governance motion as everything else. (The licensed per-security GICS mapping itself comes from your MSCI/S&P feed onto security.gics_sector ; we never bundle it.)

**Checkpoint before moving on:**
- [ ] A grouper question worked against your data with no warehouse writes
- [ ] You can explain the join-in-sandbox pattern in one sentence
- [ ] You know what's public (asset-class, GICS structure, FIGI) vs. licensed (CUSIP, per-security GICS) and how the loader refreshes seeds

## Module 5 · Ask Governed Questions
*🎯 Goal: the payoff — consistent, defensible answers routed through governed definitions, with the SQL shown*

> **Pin the scope** — In every question below, name the entity and the source-of-truth tables . A plausible answer from the wrong (summary) table is worse than no answer — if two sources could answer, run both and let your SME rule which is truth.

**Prompt for the learner to run:**
```
What's our firm-wide AUM as of the horizon? Use the governed point-in-time definition, then show me the average-12m (billing/KPI) basis too — and explain why they differ.
```

> ✅ You'll see: AUM from the account_value series at one valuation date (golden: $60,372,722,173 across 102,340 open accounts), not a sum of position snapshots — and the average-12m basis (≈$58.6B) used for billing. point_in_time ≠ average_12m ; the surface pins both ( notes/aum-definition.md ).

**Prompt for the learner to run:**
```
What were net flows (net new money) for the trailing 12 months, decomposed into gross inflows and gross outflows? Use the governed definition and confirm it excludes market movement.
```

> ✅ You'll see: the organic-growth number isolated from market beta (golden: net −$1,672,009,009 = in $648,148,543 + out −$2,320,157,552), sourced from account_value.net_external_flow ( notes/aum-definition.md ).

**Prompt for the learner to run:**
```
What was our time-weighted return over the trailing 12 months? Walk me through the governed definition — why TWR is the default, how flows are removed, and how it differs from the money-weighted (client-experience) number.
```

> ✅ You'll see: the GIPS-standard TWR (golden: +10.15% ), with client flows geometrically removed, and MWR (Modified Dietz) exposed explicitly — never silently swapped. They answer different questions: manager skill vs. the client's dollar experience ( notes/return-definition.md ).

**Prompt for the learner to run:**
```
Show our asset-class allocation at the horizon, and then how many holdings exceed 10% of their account (and how many accounts that affects, with the max weight). Cite the governed surfaces, and confirm both are computed at ONE as_of_date.
```

> ✅ You'll see: allocation weights that sum to 1.000 (golden: US_EQUITY 0.4436 · FIXED_INCOME 0.3446 · INTL_EQUITY 0.1571 · CASH 0.0547 ), and a concentration breach list (golden: 201,120 holdings >10% across 60,701 accounts, max weight 0.8601 ) — both point-in-time, both vs. the account denominator.

> **Know what concentration means here** — The governed surface flags a holding's weight in its own account above a threshold (default 10%), at one snapshot. scope="issuer" combines all share classes of one issuer — the more conservative view. Whether diversified funds are looked-through or exempt is a decision you pin ( notes/concentration-definition.md ); on the synthetic set, issuer == position because dim_security has no issuer grain.

**Prompt for the learner to run:**
```
What's our effective fee rate (realized yield, bps) over the trailing 12 months? Use the governed definition and show the average-AUM denominator logic.
```

> ✅ You'll see: realized yield as fee revenue / average AUM × 10,000 (golden: $306,644,253 / $58,582,267,503 = 52.34 bps ), with the billing-basis average AUM denominator — not point-in-time ( notes/fee-definition.md ).

> **Why everyone gets the same number** — Metrics like AUM, return, and fee yield can be computed several ways (point-in-time vs average AUM, TWR vs MWR, gross vs net). The ontology pins one governed definition — with the decision recorded in ontology/notes/ — so Finance, the CIO office, and the advisors stop disagreeing.

**Prompt for the learner to run:**
```
Here's a question from our shortlist that isn't in the governed surfaces yet: [your question]. Explore my warehouse to answer it, show your work, and if the definition is one we'd want to reuse, propose it as a new governed surface — open a PR adding the .tql and a notes file recording the decision.
```

> ✅ You'll see: Ana explore only the frontier (not re-derive the whole warehouse), answer, and propose a write-back — a new metric committed to your repo with provenance. Review and merge it, and the next person who asks gets the governed answer for free. That's the malleable loop: the ontology you ship is the one you grow, and it gets more complete every time you use it.

> **You ratify; high-stakes definitions get review** — Ana proposes; humans ratify via normal git review. Anything in governance-mnpi-pii.md scope or a core metric (AUM, return, a benchmark assignment) should require review before merge — see STANDARDS.md . The point isn't to let an agent rewrite your model unsupervised; it's that discovered knowledge is captured instead of re-discovered next time.

**Checkpoint before moving on:**
- [ ] All five metric families answered through governed surfaces, SQL shown
- [ ] You can point to the notes file explaining at least one metric's definition decision
- [ ] Ana proposed a write-back for a not-yet-governed question — and you saw it land as a PR

## Module 6 · Governance Defaults
*🎯 Goal: see the compliance behavior that's on by default — and verify it fires*

**Prompt for the learner to run:**
```
Inventory every direct identifier in the connected schema and classify each per governance-mnpi-pii.md section 0: join-key-only, never-output, or internal. Flag anything ambiguous for compliance review.
```

> ✅ You'll see: a per-column classification your compliance team signs off on — the rules are templates tuned to your regime, and they can be tightened freely but never loosened without a reviewed, attributable decision.

> **Facilitators: pre-flight these tests** — Run 6.2 and 6.3 yourself before any session with compliance in the room. These guardrails are instruction-layer enforcement — they live in the governance context files Ana reads, plus persona/RBAC ( ana.md ), which makes them verifiable and tightenable, but they depend on those files being attached and current. If a test doesn't fire: check that the ontology repo (with governance-mnpi-pii.md and config/org_context.md ) is connected to the thread, and that your fork didn't drift from the governance defaults. Demonstrating the check is part of the story — "here's the file, here's the behavior, here's how we audit it."

**Prompt for the learner to run:**
```
Break down AUM and client count by advisor × segment for a review. Apply our suppression rules and tell me what you suppressed and why.
```

> ✅ You'll see: cells whose client/household count is under min_cell_size suppressed with an explanation — and never an emitted client identifier. The starter default is 5 (books and segments are smaller than payer populations), configured in config/org_context.md . If suppression doesn't fire, don't move on — work the pre-flight check above; an unenforced rule you catch is a better demo than a rule you assumed.

**Prompt for the learner to run:**
```
Show me position-level holdings detail for [a restricted / watch-listed name], across desks.
```

> ✅ You'll see: Ana decline or constrain the request per §2 — holdings/orders/pipeline can be MNPI, default personas see aggregates not position-level detail for restricted lists, and cross-desk queries that would reveal another desk's positions are gated. The ontology must not become a side channel around the information barrier; she points to the policy file that governs it.

**Checkpoint before moving on:**
- [ ] Small-cell suppression fired and was explained
- [ ] An MNPI / restricted-list request was gated, with the governing file cited

## Module 7 · Validate Numbers & Make It Yours
*🎯 Goal: pin known-correct values, then adapt the starter's definitions to your firm — in your repo*

**Prompt for the learner to run:**
```
Run the golden queries from validation/golden-queries.md against my warehouse — AUM, net flows, TWR, allocation, concentration, advisor book, fee rate. For each, compare to a reference number I trust and flag any drift. Where we differ, explain whether it's data, definition, or time-window. Also assert the invariants: allocation weights sum to 1.000, and net_flows = gross_inflows + gross_outflows.
```

> ✅ You'll see: accuracy checked, not asserted — and a triage of any mismatch into data vs. definition vs. window. One invariant to expect: AUM ≠ Σ positions (governed AUM comes from account_value ; positions exclude cash / held-away). A gap there is a data-quality signal, not a metric bug — notes/grain.md .

> **The field lesson — why golden values exist** — Before the cast fix, concentration_risk returned the full 303,448 holdings (every holding "breached"). After fixing the precision to DECIMAL(18,6) , it returns 201,120 — the real count over 10%. That delta is exactly why every governed surface gets a pinned golden value: a definition can be structurally right and numerically wrong, and only a pinned number catches it ( notes/concentration-definition.md + the concentration_risk.tql header + golden-queries.md ).

**Prompt for the learner to run:**
```
Our house concentration limit is [your %], not 10% — and we want issuer scope (all share classes combined), with funds looked-through, not top-level. Update concentration_risk.tql in our repo to take that limit, record the decision and the rejected default in notes/concentration-definition.md, and open a PR. Add a golden-query test pinning the new breach count — and keep the CAST(... AS DECIMAL(18,6)) precision guard so the threshold doesn't round.
```

> ✅ You'll see: the change land as a reviewable PR in your repo , the rejected default recorded, and a new pinned golden value — the template stays pristine upstream; your adaptations are yours. (Other good first customizations: TWR vs MWR as your client-report default, or point-in-time vs average-12m AUM for your KPI.)

**Prompt for the learner to run:**
```
Walk the glossary's variance column for our sub-vertical ([retail wealth / institutional asset management / brokerage]). For each term that differs at our firm, propose the override in glossary.md — keep the term → definition → resolves-via pattern — and open it as one PR.
```

> ✅ You'll see: the vocabulary localized in one reviewable pass — so "client," "AUM," and "return" mean your firm's thing, everywhere, from now on.

**Checkpoint before moving on:**
- [ ] Golden queries ran; any drift was triaged (data / definition / window)
- [ ] One definition is now yours — PR'd, noted, and pinned with a test

