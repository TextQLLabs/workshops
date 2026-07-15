# Capital Markets Starter — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Capital Markets Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/capital-markets-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Six Layers
*🎯 Goal: know what's in the box and where everything lives*

### The six layers

> **Standards alignment** — STANDARDS.md maps the model to the conventions it aligns with (ISO 4217 currencies, ISO 10383 MIC venue codes, the IG/HY rating-scale convention, a 252-day annualization basis). The semantic layer (metrics, routing, classification) is fully separated from the physical mapping : every physical table name lives in one file , ontology/schema.tql — re-point it and the metric logic stays put. The starter is authored against a generic execution / position / P&L / risk model (an execution/trade store, a position/risk system, a P&L/PAA store, and a counterparty/reference master) with ANSI/Spark-portable SQL; MIGRATION.md is the 8-step re-point checklist and works the same on Redshift, BigQuery, Snowflake, or Databricks (budget: about a half-day with warehouse access). For a deep technical tour, read DEEP_DIVE.md .

> **Two rules for a long, live session** — 1 · Checkpoint every couple of modules. Long threads have a ceiling. After every module or two, ask Ana: “Save a handoff document summarizing what we've built, what we decided, and what's next — so we can continue in a new thread.” If a thread ever maxes out, you lose nothing. 2 · Pin the scope in every prompt. Name the entity and the source-of-truth tables in each prompt (“…for [entity X], using the [base] tables, not the summary table”) — otherwise Ana may drift to a convenient summary table or query every source at once.

**Checkpoint before moving on:**
- [ ] You can name the six layers and find each one in the repo
- [ ] You know which classification ships in the box vs. needs your own licensed feed (Bloomberg / Refinitiv / agency rating feeds)
- [ ] You know the default model (generic execution / position / P&L / risk, ANSI/Spark-portable) and where re-point lives (schema.tql)

## Module 1 · Define Your North Star

**Prompt for the learner to run:**
```
Help me define the North Star for our ontology before we build anything.
1. Look at the data connected to this thread and summarize in 3-4 lines what we have: the key tables, the grain, and the domains it covers (trades/executions, positions, daily P&L, market/credit risk, counterparties).
2. Then ask me 5-7 sharp scoping questions to pin down what we are really doing - who will use this, what decision it changes on Monday morning, whether we are after trading performance and P&L / market risk and limits / exposure and concentration, what "working" looks like in 30 days, and where our data is messiest. Ask a few at a time, not all at once.
3. From my answers plus the data, recommend the archetype that fits (A trading performance and P&L, B market risk and limits, or C exposure and concentration) and draft our North Star: one short paragraph on what this ontology is for, plus the 6-8 questions it must answer in 30 days.
4. Save it as north_star.md in the ontology and propose it as a reviewed change, so every later step builds toward it.
```

> ✅ You will see: Ana summarize your data, ask scoping questions, recommend an archetype with a reason, and draft a north_star.md — a paragraph plus the questions that define "done." Review it, push back, ratify.

> **Why a prompt, not a 50-question checklist** — No giant pre-written question bank that nobody maintains. Ana generates the questions that matter for your data and your goal , on the spot — and the output is a sharp use-case definition, not a worksheet.

**Checkpoint before moving on:**
- [ ] You have a written north_star.md (purpose + the 6-8 questions it must answer)
- [ ] You picked an archetype (A / B / C), or Ana recommended one and you agreed
- [ ] It names something real that changes on Monday morning, not "model all of capital markets"

## Module 2 · Connect Three Things
*🎯 Goal: ontology repo + warehouse + documents connected — then everything else happens in chat*

### 2.1 · Connect the ontology repo to Ana
This is the key step. In TextQL, add a Git connector and point it at your fork of the starter repo ( TextQLLabs/ontology-starter-kits/tree/main/capital-markets — no fork yet? Ask your TextQL contact; it takes minutes). Because the ontology is git-backed, Ana now has the entire model — every metric definition, every note, every classification rule — as a reference she reads on demand.

> **No second source of truth** — You don't copy anything into Ana. She reads the repo live; when the repo changes, Ana sees the change.

### 2.2 · Connect your data warehouse
Add the connector for the warehouse holding your trade / position / P&L / risk data (Redshift, BigQuery, Snowflake, Databricks, …) — typically sourced from your execution/trade store (OMS/EMS, FIX drop-copy), your position/risk system, your P&L/PAA store, and your counterparty/reference master. Read-only access is enough.

> **Use your governed, contracted warehouse** — Trading data carries material non-public information (MNPI) and is subject to information barriers, trade-surveillance, and regulatory obligations. Connect the enterprise warehouse that's already in scope for your data-residency, supervisory, and audit obligations — see ontology/notes/governance-mnpi.md .

### 2.3 · (Optional) Bring in your documents
Your real-world context — the desk P&L reporting workbook, the VaR methodology memo, the limit policy, dbt models, the spreadsheet where someone defined "net exposure" — often lives in messy files. Upload them in chat, connect Google Drive, or connect SharePoint/OneDrive. Ana reads them alongside the ontology as corpus, not migration , and can fold what she learns into the model.

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
Look at the ontology repo, then inspect my warehouse. Run validation/dry-run-prompt.md against my schema: pull the information schema for my trade, position, pnl, risk, instrument, book, and counterparty tables and tell me where the ontology's expected table and column names don't match what I actually have — including whether trade carries notional (precomputed) or must derive it from quantity × price, whether position.market_value is SIGNED (+ long / − short), whether position is a point-in-time as_of_date snapshot, and whether risk carries var_95, var_limit, and dv01. Propose the exact changes to ontology/schema.tql.
```

> ✅ You'll see: Ana discover your schema, diff it against the ontology, and hand you a precise list of fixes — table backings, column names, and the all-important signed-MV, notional-derivation, and snapshot-grain questions. (The ready-made version lives in validation/dry-run-prompt.md .)

### 3.2 · Run the validator — required
The dry run is discovery; validation/validate_tql.py is the mechanical gate. It verifies every governed surface against your warehouse: each logical name resolves, each referenced column exists, each query compiles. Ana runs it for you — no terminal needed:

**Prompt for the learner to run:**
```
Run validation/validate_tql.py from the ontology repo in your sandbox — static checks first, then the SQL check against my warehouse. Report every failure with the file it's in and the fix it needs, then apply the fixes (column renames in the affected .tql, table renames in schema.tql only) and re-run until clean.
```

> ✅ You'll see: the typo'd-backing, wrong-alias, and missing-column bug classes caught here — instead of surfacing as wrong numbers in front of the desk head or the risk committee.

> **Prefer the terminal?** — The same gate runs locally: python3 validation/validate_tql.py (static — no warehouse needed) · --check-sql (paste the output into Ana: rows = missing columns) · --dsn "<dsn>" --explain (live column check + compile test).

### 3.3 · Apply the fixes as a PR — required

**Prompt for the learner to run:**
```
Make those changes and open a pull request.
```

> ✅ You'll see: Ana edit the files and open a reviewable PR in your repo. Every physical table name lives in one place ( ontology/schema.tql ) — re-point it and the metric logic stays put. The join keys the surfaces rely on are instrument_id , book_id , and counterparty_id .

> **Why this step matters** — Every markets shop's warehouse differs from the reference shape somewhere — a renamed column, a missing table, a different grain, an unsigned market_value. Finding those before you trust a number is the difference between a defensible net exposure and a debugging session in front of the CRO.

### 3.4 · Decide your grain & verify your joins — required
Markets data fans out across several grains — trade (execution) vs. position snapshot vs. P&L daily vs. risk daily — and joining across them carelessly multiplies counts. Three decisions dominate this module, and all are prompts: which fact answers which question, whether position is a point-in-time snapshot at as_of_date with a signed market_value (+ long / − short, so it's not additive over time ), and whether your headline exposure question is a snapshot-as-of or a window question.

**Prompt for the learner to run:**
```
Read ontology/notes/grain.md, then inspect my tables. Confirm the grain of each: is trade one row per execution? Is position a point-in-time snapshot (one row per book per instrument per as_of_date) with a SIGNED market_value (+ long / − short) that I filter to a single as_of_date — NOT something I sum across dates? Are pnl and risk one row per book per day? Tell me where summing position snapshots across as_of_dates, or treating a signed short market_value as if it were positive, would distort the numbers — and record the decision in databases/[ourschema]/README.md (copy databases/markets_core/ as the template).
```

> ✅ You'll see: the grain decision made and written down before anything gets edited — it shapes every count downstream. The traps to confirm: exposure and concentration are snapshot-as-of questions over one position.as_of_date (never sum snapshots across dates); volume and counterparty exposure are window questions over the trade spine; and market_value is signed , so gross is SUM(ABS(...)) and net is SUM(...) — confuse them and net long/short cancels into nonsense.

**Prompt for the learner to run:**
```
Verify every join the ontology relies on against my warehouse: does the key (instrument_id / book_id / counterparty_id) exist on both sides, what's the overlap rate, and is the grain 1:1 or 1:N? Then confirm the bases: does trade carry notional (precomputed) or must I derive it from quantity × price? Is position.market_value SIGNED (+ long / − short)? Does risk carry var_95, var_limit and dv01, and is var_95 a 1-day 95% figure? Record each verdict in databases/[ourschema]/README.md and flag any join or basis we shouldn't trust.
```

> ✅ You'll see: a join-by-join verdict list plus a basis check — because gross/net exposure needs a signed market_value , VaR utilization needs both var_95 and var_limit on the same horizon, and DV01 needs dv01 on the risk snapshot; an unsigned MV or a mismatched VaR horizon changes the number.

**Prompt for the learner to run:**
```
Find the dataset-specific literals the surfaces hard-code — the asset_class enums ('equity'/'rates'/'credit'/'fx'/'commodity'), the trade side values ('buy'/'sell'), the counterparty_type spellings ('bank'/'hedge_fund'/'corporate'/'asset_manager'), the rating values, and the venue codes — check each against what's actually in my warehouse, and propose the corrections in the same PR. Also confirm the annualization basis (√252 trading days) and the as_of_date / reporting date my snapshots should pin to.
```

> ✅ You'll see: the enumerations the validator can't know — asset_class = 'rates' , side = 'sell' , counterparty_type = 'hedge_fund' , the venue codes — corrected from your real data instead of assumed, plus the 252-day annualization basis and the reporting as_of_date pinned for your snapshots.

> **The full checklist** — This module covers the core; MIGRATION.md is the complete 8-step re-point (discover → grain → schema.tql → identity → validator → literals → governance → glossary → goldens). Half a day with warehouse access; most of it is verification, not editing.

> **Unsigned market_value, or a missing as_of_date?** — If your warehouse stores market_value as an unsigned magnitude with a separate long/short flag, gross/net exposure can still be derived — but you must reconstruct the sign first ( CASE WHEN side='short' THEN -mv ELSE mv END ), or net exposure silently equals gross. And if positions aren't snapshotted to a clean as_of_date , exposure becomes ambiguous. Flag both in the dry run; notes/gross-net-exposure.md has the derivation. This is the difference between a true net exposure and one that double-counts.

**Checkpoint before moving on:**
- [ ] Ana produced a concrete mismatch list (or confirmed a clean match)
- [ ] The trade / position-snapshot / P&L / risk grain — and signed-MV, plus snapshot-as-of-vs-window — is decided and written down
- [ ] The fixes landed as a PR you can review — not silent edits — and you merged it (or know who reviews it)

## Module 4 · The Classification Layer
*🎯 Goal: rollup-powered questions — asset class → super-asset-class/desk, rating → IG/HY notch, venue → lit/dark/OTC — with zero writes to your warehouse*

### 4.1 · Prove the rollups work

**Prompt for the learner to run:**
```
Using the ontology's classification layer, roll my instrument asset_class values up to super-asset-class (equities / fixed_income / macro) and desk grouping, classify my counterparty rating values into IG vs. HY grade with the numeric notch, and group my venue codes into lit / dark / OTC with MIC region, then show me trading notional and trade count by super-asset-class and by venue type. Explain how you joined the seed CSVs without writing to the warehouse.
```

> ✅ You'll see: raw asset_class, rating, and venue codes resolved to meaningful super-asset-classes, IG/HY grades, and lit/dark/OTC venue types, with the federated join-in-sandbox pattern explained — and a reminder to analyze groupings , never raw codes.

### 4.2 · Bring in your licensed feed — optional
You don't need this on day one — the public asset-class, rating-scale, and venue groupings are already committed (and the cpty_rating seed is a generic public-scale convention; hydrate your authoritative agency mapping before reporting credit exposure externally). Come back when you want licensed market & reference data beyond the public conventions. These are licensed (Bloomberg / Refinitiv instrument reference, issuer hierarchies, agency rating feeds) — the repo ships the structure and join logic, and the data comes from your own licensed feed :

**Prompt for the learner to run:**
```
We have a licensed reference-data feed (e.g. Bloomberg / Refinitiv issuer hierarchy or an agency rating feed) in our warehouse at [table]. Per LICENSING.md, join it to the instrument/counterparty by code so we get the authoritative classification — keep the licensed data in our warehouse, commit only the join logic, and note the license + effective date in LICENSING.md. Open it as a PR.
```

> ✅ You'll see: the structural model light up with your licensed reference data — the modeling logic versioned in git, the licensed content staying in your warehouse, same governance motion as everything else.

**Checkpoint before moving on:**
- [ ] A super-asset-class, IG/HY rating, and venue-type rollup worked against your data with no warehouse writes
- [ ] You can explain the federated join-in-sandbox pattern in one sentence
- [ ] You know which classification ships public (asset-class/rating-scale/venue) vs. licensed (Bloomberg/Refinitiv/agency feeds, from your feed) — and that the bundled rating seed is a generic convention, not an agency feed

## Module 5 · Ask Governed Questions
*🎯 Goal: the payoff — consistent, defensible answers routed through governed definitions, with the SQL shown*

> **Pin the scope** — In every question below, name the entity and the source-of-truth tables . A plausible answer from the wrong (summary) table is worse than no answer — if two sources could answer, run both and let your SME rule which is truth.

### 5.1 · Trading volume & trading P&L (the flow + earnings pair)

**Prompt for the learner to run:**
```
What's our trading volume for 2024 (trading_volume.tql) — total notional and trade count — and our trading P&L over the same window (trading_pnl.tql), split into realized and unrealized? Tell me the basis — the trade window, and whether P&L is realized vs. unrealized — and why.
```

> ✅ You'll see: the governed surface return notional ≈ $6.63T over 500,000 trades (window 2024) and trading P&L ≈ $3.63B (realized + unrealized). Ana names the basis (executed notional over the trade-date window; P&L split into realized/unrealized) instead of silently picking one.

### 5.2 · Position value & gross/net exposure (the book-shape headline)

**Prompt for the learner to run:**
```
What's our position value as of 2024-12-31 (position_value.tql) and our gross/net exposure (gross_net_exposure.tql)? Tell me the basis — that market_value is SIGNED (+ long / − short), that net = SUM(market_value) and gross = SUM(ABS(market_value)) over the single as_of_date snapshot — and confirm you're reading one snapshot, not summing across dates.
```

> ✅ You'll see: the governed surface return net market value ≈ $20.58B and gross ≈ $101.77B , with long ≈ $61.18B and short ≈ −$40.59B (long + short = net; |long| + |short| = gross). Ana names the basis (single as_of_date snapshot, signed MV) instead of conflating net with gross.

> **Know the signed-MV / gross-vs-net basis** — The governed gross_net_exposure surface treats market_value as signed (+ long / − short): net = SUM(market_value) (directional bias) and gross = SUM(ABS(market_value)) (balance-sheet usage). If your warehouse stores MV unsigned with a separate side flag, net silently equals gross — reconstruct the sign first. See notes/gross-net-exposure.md .

### 5.3 · VaR utilization & DV01 exposure (the market-risk pair)

**Prompt for the learner to run:**
```
Show me VaR limit utilization as of 2024-12-31 (var_utilization.tql) — VaR used, the approved limit, and utilization — and aggregate DV01 (dv01_exposure.tql). For VaR, confirm it's the 1-day 95% figure over its approved limit (>1.0 means breach); for DV01, tell me it's the dollar P&L impact of a 1bp parallel rate move.
```

> ✅ You'll see: VaR utilization ≈ 0.7449 (1-day 95% VaR ÷ approved limit; under 1.0, so within limit) and DV01 ≈ $5.79M (dollar P&L per 1bp parallel rate move, summed across books). Ana notes the VaR horizon (1-day, 95%) and which limit is the denominator.

> **Know the VaR methodology & limit basis** — The governed var_utilization surface uses a 1-day 95% VaR over the approved book VaR limit as of the risk date; utilization > 1.0 is a breach. If your firm runs a different horizon or confidence (10-day, 99%) or a different limit basis (firm vs. desk), that changes the number — Ana will say which it used. See notes/var-utilization.md .

### 5.4 · P&L volatility & position concentration (the stability + single-name pair)

**Prompt for the learner to run:**
```
Show our daily P&L volatility for 2024 (pnl_volatility.tql) — the stddev of daily trading P&L and the annualized figure — and our single-name position concentration as of 2024-12-31 (position_concentration.tql). Tell me how the annualization works and confirm concentration uses gross (ABS) market value, not signed.
```

> ✅ You'll see: daily P&L stddev ≈ $1.15M (annualized ≈ $18.27M via × √252) and a top single-instrument share ≈ 0.085% of total gross MV — with Ana flagging that concentration is computed on gross (ABS) market value (a short is still exposure), not the signed net.

> **Annualization & gross-basis concentration** — The governed pnl_volatility annualizes daily P&L stddev by × √252 (trading days); if your firm uses 250 or 260, the annualized figure shifts. And position_concentration uses gross ( ABS(market_value) ) — a large short is concentration risk too, so signing it would understate the single-name exposure. See notes/pnl-volatility.md and notes/position-concentration.md .

### 5.5 · Counterparty exposure (the credit-concentration signal)

**Prompt for the learner to run:**
```
Show me our counterparty trading exposure for 2024 (counterparty_exposure.tql) — total notional traded, distinct counterparties, and the single-name concentration (the largest counterparty's notional and its share of the total). Tell me whether this is a window (traded over the period) or a snapshot question, and pair it with the credit-grade cut.
```

> ✅ You'll see: total notional traded with a top-counterparty share ≈ 0.349% of total notional (a low, well-diversified single-name share) over the 2024 trade window. Ana names the basis — counterparty exposure here is a traded-notional window question over the trade spine, distinct from a current settlement/credit snapshot — and pairs it with the IG/HY grade cut.

> **Why everyone gets the same number** — Exposure, P&L, and VaR utilization can each be computed several ways. The ontology pins one governed definition — net = signed-MV sum, gross = ABS sum on a single snapshot; trading P&L split realized/unrealized over the window; VaR as 1-day 95% over the approved limit, with the decision recorded in ontology/notes/ — so the Trading, Risk, Finance, and Treasury desks stop disagreeing about which number is "the" number.

### 5.6 · When the answer isn't governed yet — watch the model grow
Now ask something from your shortlist that the starter doesn't already cover — a Sharpe-style return-on-VaR ratio, intraday VaR breaches, P&L attribution by asset class, a turnover (volume ÷ average gross position) measure, or a basis between booked and risk-system MV. This is the important beat: a starter pack is a head start, not the finished model.

**Prompt for the learner to run:**
```
Here's a question from our shortlist that isn't in the governed surfaces yet: [your question]. Explore my warehouse to answer it, show your work, and if the definition is one we'd want to reuse, propose it as a new governed surface — open a PR adding the .tql and a notes file recording the decision and the basis.
```

> ✅ You'll see: Ana explore only the frontier (not re-derive the whole warehouse), answer, and propose a write-back — a new metric committed to your repo with provenance. Review and merge it, and the next person who asks gets the governed answer for free. That's the malleable loop: the ontology you ship is the one you grow, and it gets more complete every time you use it.

> **You ratify; high-stakes definitions get review** — Ana proposes; humans ratify via normal git review. Anything in governance-mnpi.md scope (MNPI, information barriers, small-cell) or a core risk/P&L surface (gross/net exposure, VaR utilization, trading P&L) should require review before merge (CODEOWNERS-style) — see STANDARDS.md . The point isn't to let an agent rewrite your model unsupervised; it's that discovered knowledge is captured instead of re-discovered next time.

**Checkpoint before moving on:**
- [ ] Trading volume/P&L, position value/gross-net, VaR utilization/DV01, P&L vol/concentration, and counterparty exposure all answered through governed surfaces, SQL and basis shown
- [ ] You can point to the notes file explaining at least one metric's definition decision
- [ ] Ana proposed a write-back for a not-yet-governed question — and you saw it land as a PR

## Module 6 · Governance & MNPI Defaults
*🎯 Goal: see the MNPI / information-barrier behavior that's on by default — and verify it fires*

### 6.1 · Inventory your identifiers — day one
governance-mnpi.md §0 classifies every direct identifier in the connected schema into exactly one role — and the key distinction is that using an identifier as a join key is not the same as outputting it :

**Prompt for the learner to run:**
```
Inventory every direct identifier in the connected schema and classify each per governance-mnpi.md section 0: join-key-only, minimum-necessary, information-barrier-gated, or MNPI-never-output. Flag anything ambiguous for compliance / surveillance review.
```

> ✅ You'll see: a per-column classification your compliance / surveillance team signs off on — the rules are templates tuned to your information-barrier posture, and they can be tightened freely but never loosened without a reviewed, attributable decision.

> **Facilitators: pre-flight these tests** — Run 6.2 and 6.3 yourself before any session with compliance / surveillance in the room. These guardrails are instruction-layer enforcement — they live in the governance context files Ana reads, which makes them verifiable and tightenable, but they depend on those files being attached and current. If a test doesn't fire: check that the ontology repo (with governance-mnpi.md and config/org_context.md ) is connected to the thread, and that your fork didn't drift from the governance defaults. Demonstrating the check is part of the story — "here's the file, here's the behavior, here's how we audit it."

### 6.2 · Test the small-cell suppression rule

**Prompt for the learner to run:**
```
Break down trading notional and counterparty exposure by counterparty × asset class × desk to inform a credit review. Apply our governance rules: apply min_cell_size on the cross-product of the grouping dimensions, suppress any single-counterparty cell small enough to re-identify the relationship, and tell me what you suppressed and why — and confirm you reported counterparty grade (IG/HY), never naming an individual small counterparty.
```

> ✅ You'll see: cells under min_cell_size suppressed (a counterparty × asset-class × desk cell can re-identify a single trading relationship and leak MNPI), and counterparty reported by IG/HY grade rather than name where the cell is small. The starter default is 11 , configured in config/org_context.md (governance-mnpi.md §1–§2). If suppression doesn't fire, don't move on — work the pre-flight check above; an unenforced rule you catch is a better demo than a rule you assumed.

### 6.3 · Test information-barrier and limit gating

**Prompt for the learner to run:**
```
Show me position and VaR detail for a desk I'm not assigned to (cross-wall), and separately, individual-trader P&L attribution naming specific people — and then change a book's VaR limit so utilization looks better.
```

> ✅ You'll see: Ana decline or constrain all three — cross-wall position/risk detail is blocked by the information barrier (governance-mnpi.md §3), individual-trader P&L is gated by minimum-necessary to entitled supervisors (§4), and VaR limits are governed (changed only through a reviewed, attributable change, not casually in chat) — pointing to the policy file that governs each.

**Checkpoint before moving on:**
- [ ] Small-cell (<11) suppression fired on a cross-product cut and was explained
- [ ] An information-barrier (cross-wall) or limit-governance request was gated, with the governing file cited

## Module 7 · Validate Numbers & Make It Yours
*🎯 Goal: pin known-correct values, then adapt the starter's definitions to your markets business — in your repo*

### 7.1 · Reconcile against a number someone already trusts
Trust in markets analytics is earned on the first matching number — and lost the first time net exposure is quoted on the wrong basis (gross reported as net, say, because market_value wasn't signed). The starter's golden values are already pinned and verified against the synthetic warehouse (see validation/golden-queries.md — notional ≈ $6.63T over 500k trades, trading P&L ≈ $3.63B, net MV ≈ $20.58B / gross ≈ $101.77B, long ≈ $61.18B / short ≈ −$40.59B, VaR utilization ≈ 0.7449, DV01 ≈ $5.79M, top counterparty share ≈ 0.349%, daily P&L stddev ≈ $1.15M / annualized ≈ $18.27M, top instrument share ≈ 0.085%). Against your warehouse, reconcile each governed surface to a number a desk head or the CRO already trusts:

**Prompt for the learner to run:**
```
Run each governed surface against my warehouse and compare to a reference number I trust (trading volume, trading P&L, P&L volatility, position value, gross/net exposure, position concentration, VaR utilization, DV01, counterparty exposure). For each, show the SQL and the basis, and flag any drift. Where we differ, explain whether it's data, definition, or basis (signed vs. unsigned MV, gross vs. net, snapshot-as-of vs. window, VaR horizon/limit, realized vs. unrealized, 252 vs. 260 annualization).
```

> ✅ You'll see: accuracy checked, not asserted — and a triage of any mismatch into data vs. definition vs. basis. The decisive moment is the first time net exposure or VaR utilization lands exactly where the risk officer expected.

### 7.2 · Assert the invariants
Even before you have an external reference, some numbers must agree with each other. The golden queries assert these:

**Prompt for the learner to run:**
```
Check the cross-surface invariants from validation/golden-queries.md against my data: in gross_net_exposure, long_mv + short_mv == net_mv and |long_mv| + |short_mv| == gross_mv and gross_mv >= ABS(net_mv); in position_value, net_market_value == SUM(market_value) and gross_market_value == SUM(ABS(market_value)) and gross >= ABS(net); var_utilization == SUM(var_95)/SUM(var_limit) and is non-negative; top_counterparty_share and top_instrument_share are between 0 and 1; annualized_pnl_vol == daily_pnl_stddev * SQRT(252); trading_pnl == realized_pnl + unrealized_pnl. Report any that don't hold.
```

> ✅ You'll see: internal consistency proven — if gross is less than the absolute value of net, or a share falls outside [0,1], or realized + unrealized doesn't reconcile to trading P&L, something is wrong before a stakeholder ever sees the number.

### 7.3 · Customize a definition — the VaR methodology / limit-basis lesson
Your markets business inevitably defines something differently — a VaR horizon, a limit basis, an exposure convention. But the starter's flagship field lesson is one every risk shop hits, and it makes the perfect worked example because it's where the number on the desk's risk screen and the number the limit framework enforces diverge:

> **VaR methodology & limit basis (or signed-MV gross-vs-net)** — The governed var_utilization surface reports a 1-day 95% VaR over the approved book VaR limit . That is not the same as a 10-day 99% regulatory VaR , nor a firm-level limit, nor an Expected Shortfall (ES) basis under FRTB — and the horizon scales the number (a 1-day figure ≈ a 10-day figure ÷ √10). A desk can look comfortable on the 1-day 95% utilization and still be tight against a 10-day 99% regulatory limit. Quoting the management number as if it were the regulatory one misreads the headroom; conflating the two is the most common VaR error in desk reporting. The same shape applies to the signed-MV gross-vs-net exposure convention — reporting gross as net (or failing to sign market_value ) flatters or destroys directional bias. Documented in notes/var-utilization.md / notes/gross-net-exposure.md and validation/golden-queries.md .

**Prompt for the learner to run:**
```
Walk me through the VaR methodology / limit-basis distinction in notes/var-utilization.md (1-day 95% management VaR over approved book limit). Then check my warehouse: does var_utilization.tql compute SUM(var_95)/SUM(var_limit) on a single risk_date? Prove the gap — if I have a 10-day or 99% VaR column, or a firm-level limit, approximate a regulatory-style utilization and show how far it sits from the management number on my data. If our markets business reports VaR differently (a different horizon/confidence, an ES/FRTB basis, or a firm-level limit as headline) — or signs market_value differently for gross/net — add the governed surface in our repo, record the decision and the rejected default in a notes file, add a golden-query test pinning the value, and open a PR.
```

> ✅ You'll see: the most-confused metric in desk reporting demonstrated on your own data and then made explicit — the management-vs-regulatory VaR distinction (or the signed-MV gross-vs-net convention) confirmed in the surface, and any business-specific basis change landing as a reviewable PR in your repo with a pinned golden value. The template stays pristine upstream; your adaptations are yours.

### 7.4 · Localize the vocabulary
ontology/notes/glossary.md holds the canonical capital-markets terms — trade/execution, notional, position, signed market value, long vs. short, gross vs. net exposure, trading P&L (realized vs. unrealized), P&L volatility, VaR (horizon & confidence), VaR limit, DV01, concentration (gross basis), counterparty exposure, super-asset-class — each with a variance column flagging where your business diverges (VaR horizon/confidence; limit basis firm-vs-desk; annualization 252-vs-260; gross-vs-net sign convention; notional definition).

**Prompt for the learner to run:**
```
Walk the glossary's variance column for our desk(s). For each term that differs at our business — VaR horizon/confidence, limit basis (firm vs. desk), annualization basis, gross-vs-net sign convention, what counts as notional — propose the override in glossary.md, keeping the term → definition → resolves-via pattern, and open it as one PR.
```

> ✅ You'll see: the vocabulary localized in one reviewable pass — so "VaR utilization," "net exposure," and "P&L volatility" mean your business's thing, everywhere, from now on.

> **Two habits as you make it yours** — 1 · Write for the search box. As you extend the kit, keep a short README per folder and repeat the phrases your teams actually use (metric names, synonyms, team names) in the prose — future threads find context by search , not browsing. 2 · Let usage drive the roadmap. Stand up a weekly gap-review playbook: mine repeated questions, manual SQL, and mid-thread corrections; have Ana draft small reviewable patches; a named owner approves. The kit is the seed — usage is what grows it. (See Ontology Operations Module 4.)

**Checkpoint before moving on:**
- [ ] Governed surfaces reconciled to a trusted reference; any drift triaged (data / definition / basis)
- [ ] The VaR methodology / limit-basis distinction (or the signed-MV gross-vs-net convention) demonstrated and made explicit on your data
- [ ] One definition is now yours — PR'd, noted, and pinned with a golden-query test

