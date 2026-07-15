# The Healthcare Starter Pack — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "The Healthcare Starter Pack" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/healthcare-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Six Layers
*🎯 Goal: know what's in the box and where everything lives*

### The six layers

> **Standards alignment** — STANDARDS.md maps the model to the industry data models it aligns with (Tuva, OMOP, FHIR, X12). The semantic layer (metrics, routing, terminology) is fully separated from the physical mapping : every physical table name lives in one file , ontology/schema.tql — re-point it and the metric logic stays put. The starter is authored Redshift-first with BigQuery equivalents inline; MIGRATION.md is the 8-step re-point checklist and works the same for commercial, Medicare, or Medicaid warehouses on Redshift, BigQuery, Snowflake, or Databricks (budget: about a half-day with warehouse access). For a deep technical tour, read DEEP_DIVE.md .

> **Two rules for a long, live session** — 1 · Checkpoint every couple of modules. Long threads have a ceiling. After every module or two, ask Ana: “Save a handoff document summarizing what we've built, what we decided, and what's next — so we can continue in a new thread.” If a thread ever maxes out, you lose nothing. 2 · Pin the scope in every prompt. Name the entity and the source-of-truth tables in each prompt (“…for [entity X], using the [base] tables, not the summary table”) — otherwise Ana may drift to a convenient summary table or query every source at once.

**Checkpoint before moving on:**
- [ ] You can name the six layers and find each one in the repo
- [ ] You know which code systems ship in the box vs. need your own license/key
- [ ] You know the default dialect (Redshift-first, Tuva-shaped) and where BigQuery notes live

## Module 1 · Define Your North Star

**Prompt for the learner to run:**
```
Help me define the North Star for our ontology before we build anything.
1. Look at the data connected to this thread and summarize in 3-4 lines what we have: the key tables, the grain, and the domains it covers.
2. Then ask me 5-7 sharp scoping questions to pin down what we are really doing - who will use this, what decision it changes on Monday morning, whether we are after BI parity / care-gap and quality / risk adjustment / operational queries, what "working" looks like in 30 days, and where our data is messiest. Ask a few at a time, not all at once.
3. From my answers plus the data, recommend the archetype that fits (A BI parity, B care-gap/quality, or C risk adjustment) and draft our North Star: one short paragraph on what this ontology is for, plus the 6-8 questions it must answer in 30 days.
4. Save it as north_star.md in the ontology and propose it as a reviewed change, so every later step builds toward it.
```

> ✅ You will see: Ana summarize your data, ask scoping questions, recommend an archetype with a reason, and draft a north_star.md — a paragraph plus the questions that define "done." Review it, push back, ratify.

> **Why a prompt, not a 50-question checklist** — No giant pre-written question bank that nobody maintains. Ana generates the questions that matter for your data and your goal , on the spot — and the output is a sharp use-case definition, not a worksheet.

**Checkpoint before moving on:**
- [ ] You have a written north_star.md (purpose + the 6-8 questions it must answer)
- [ ] You picked an archetype (A / B / C), or Ana recommended one and you agreed
- [ ] It names something real that changes on Monday morning, not "model all of healthcare"

## Module 2 · Connect Three Things
*🎯 Goal: ontology repo + warehouse + documents connected — then everything else happens in chat*

### 1.1 · Connect the ontology repo to Ana
This is the key step. In TextQL, add a Git connector and point it at your fork of the starter repo ( TextQLLabs/ontology-starter-kits/tree/main/healthcare — no fork yet? Ask your TextQL contact; it takes minutes). Because the ontology is git-backed, Ana now has the entire model — every metric definition, every note, every coding rule — as a reference she reads on demand.

> **No second source of truth** — You don't copy anything into Ana. She reads the repo live; when the repo changes, Ana sees the change.

### 1.2 · Connect your data warehouse
Add the connector for the warehouse holding your claims/clinical data (Redshift, BigQuery, Snowflake, …). Read-only access is enough.

> **BAA first** — A BAA must be in place before any PHI flows. Use your enterprise, BAA-covered warehouse — see ontology/notes/governance-phi.md .

### 1.3 · (Optional) Bring in your documents
Your real-world context — SOPs, metric definitions, plan documents, policies — often lives in messy files. Upload them in chat, connect Google Drive, or connect SharePoint/OneDrive. Ana reads them alongside the ontology and can fold what she learns into the model.

### 1.4 · Say hello

**Prompt for the learner to run:**
```
Read the ontology repo and give me a tour: what entities, metrics, and terminology groupers are defined, and what governed questions am I able to ask once my warehouse is validated?
```

> ✅ You'll see: Ana describe the model from the repo itself — proof the Git connector works and the ontology is being read as ground truth.

**Checkpoint before moving on:**
- [ ] Ana described the starter's entities and metrics from the connected repo
- [ ] Your warehouse connector is attached, read-only, and BAA-covered
- [ ] You know which of your documents you'll bring in (or that you're skipping this)

## Module 3 · Validate Against Your Schema
*🎯 Goal: before trusting numbers, prove the ontology's assumptions match your actual tables — without writing SQL*

### 2.1 · The dry run — required

**Prompt for the learner to run:**
```
Look at the ontology repo, then inspect my warehouse. Pull the information schema for my claims tables and tell me where the ontology's expected table and column names don't match what I actually have. Propose the exact changes to ontology/schema.tql.
```

> ✅ You'll see: Ana discover your schema, diff it against the ontology, and hand you a precise list of fixes — table backings, column names. (A ready-made version of this check lives in validation/dry-run-prompt.md .)

### 2.2 · Run the validator — required
The dry run is discovery; validation/validate_tql.py is the mechanical gate. It verifies every governed surface against your warehouse: each logical name resolves, each referenced column exists, each query compiles. Ana runs it for you — no terminal needed:

**Prompt for the learner to run:**
```
Run validation/validate_tql.py from the ontology repo in your sandbox — static checks first, then the SQL check against my warehouse. Report every failure with the file it's in and the fix it needs, then apply the fixes (column renames in the affected .tql, table renames in schema.tql only) and re-run until clean.
```

> ✅ You'll see: the typo'd-backing, wrong-alias, and missing-column bug classes caught here — instead of surfacing as wrong numbers in front of stakeholders.

> **Prefer the terminal?** — The same gate runs locally: python3 validation/validate_tql.py (static — no warehouse needed) · --check-sql (paste the output into Ana: rows = missing columns) · --dsn "<dsn>" --explain (live column check + compile test).

### 2.3 · Apply the fixes as a PR — required

**Prompt for the learner to run:**
```
Make those changes and open a pull request.
```

> ✅ You'll see: Ana edit the files and open a reviewable PR in your repo. Every physical table name lives in one place ( ontology/schema.tql ) — re-point it and the metric logic stays put. If you have a real member/patient table, point person_grain at it: one line.

> **Why this step matters** — Every customer's warehouse differs from the reference shape somewhere — a renamed column, a missing table, a different grain. Finding those before you trust a number is the difference between a defensible metric and a debugging session in front of stakeholders.

### 2.4 · Decide your claim grain & verify your joins — required
Two decisions shape everything downstream, and both are prompts:

**Prompt for the learner to run:**
```
Read ontology/notes/claim-grain.md, then inspect my claims tables. Is our model wide (one row per line, header flattened on) or normalized header/line/event? Recommend which table anchors claims, where the cost columns live, and how the member key is reached — and record the decision in databases/[ourschema]/README.md (copy databases/tuva/ as the template).
```

> ✅ You'll see: the grain decision made and written down before anything gets edited — it shapes every other step.

**Prompt for the learner to run:**
```
Per ontology/notes/join-key-verification.md, verify every join the ontology relies on against my warehouse: does the key exist on both sides, what's the overlap rate, and is the grain 1:1 or 1:N? Record each verdict in databases/[ourschema]/README.md and flag any join we shouldn't trust.
```

> ✅ You'll see: a join-by-join verdict list. Even the starter's own demo schema teaches the lesson — there's no patient table (demographics live on eligibility ) and codes live in normalized_code . Expect quirks like these in yours.

**Prompt for the learner to run:**
```
Find the dataset-specific literals flagged inline in the .tql files — encounter_type values, normalized_code_type spelling, and the cost basis default (charge vs allowed vs paid) — check each against what's actually in my warehouse, and propose the corrections in the same PR.
```

> ✅ You'll see: the enumerations the validator can't know, corrected from your real data instead of assumed.

> **The full checklist** — This module covers the core; MIGRATION.md is the complete 8-step re-point (discover → grain → schema.tql → joins → validator → literals → governance → glossary → goldens). Half a day with warehouse access; most of it is verification, not editing.

> **Different warehouse dialect?** — The starter is authored Redshift-first with BigQuery equivalents inline. On Databricks, Snowflake, or another engine , extend the dry-run ask: "…also flag any Redshift-specific SQL in the .tql surfaces and propose [dialect] equivalents." Dialect adaptation rides the same PR loop as schema fixes.

### Verify the pack’s assumptions against your data
This starter already handles the healthcare cases that break naive ontologies — claim adjustments and reversals, dual-eligible members, capitated and encounter data — but it assumes a representation. Confirm yours matches before you trust the numbers.

**Prompt for the learner to run:**
```
Check how our data represents claim adjustments and reversals, dual-eligible members, and capitated / encounter data. Compare each to what the starter ontology assumes, flag any mismatch, and propose the exact fix.
```

**Checkpoint before moving on:**
- [ ] Ana produced a concrete mismatch list (or confirmed a clean match)
- [ ] The fixes landed as a PR you can review — not silent edits
- [ ] You merged the PR (or know who reviews it)

## Module 4 · The Terminology Layer
*🎯 Goal: grouper-powered questions — CCSR, HCC/RAF, chronic conditions — with zero writes to your warehouse*

### 3.1 · Prove the groupers work

**Prompt for the learner to run:**
```
Using the ontology's terminology layer, show me which CCSR category my top 10 most frequent diagnosis codes fall into. Explain how you joined the crosswalk without writing to the warehouse.
```

> ✅ You'll see: raw ICD-10 codes resolved to meaningful clinical categories, with the join-in-sandbox pattern explained.

### 3.2 · Refresh or extend the reference data — optional
You don't need this on day one — the current crosswalks are already committed. Come back when a standards version updates (CCSR/CMS-HCC release annually) or when you need certified VSAC value sets. And you don't run the scripts yourself — Ana does:

**Prompt for the learner to run:**
```
The terminology standards have a new release. Run reference/terminology/load_terminology.py in your sandbox to re-download CCSR / CMS-HCC / GEMs and regenerate the CSVs, then open a PR with the refreshed files and a summary of what changed between versions.
```

**Prompt for the learner to run:**
```
Using fetch_vsac.py with our UMLS API key [key], hydrate the VSAC value sets registered in value-sets.json. Confirm we're committing only the OIDs, not the licensed codes, per LICENSING.md.
```

> ✅ You'll see: refreshed reference tables arriving as a reviewable PR — same governance motion as everything else.

**Checkpoint before moving on:**
- [ ] A grouper question worked against your data with no warehouse writes
- [ ] You can explain the join-in-sandbox pattern in one sentence
- [ ] You know how terminology refreshes (loader) and how licensed value sets hydrate (UMLS key)

## Module 5 · Ask Governed Questions
*🎯 Goal: the payoff — consistent, defensible answers routed through governed definitions, with the SQL shown*

> **Pin the scope** — In every question below, name the entity and the source-of-truth tables . A plausible answer from the wrong (summary) table is worse than no answer — if two sources could answer, run both and let your SME rule which is truth.

### 4.1 · Prevalence (the "diabetes question")

**Prompt for the learner to run:**
```
How many members have diabetes? Use the governed prevalence definition and tell me which codes/grouper you used and why.
```

> ✅ You'll see: the whole ICD-10 branch handled by the grouper — one consistent, defensible number, not a guess over 70,000 codes.

### 4.2 · Cost PMPM

**Prompt for the learner to run:**
```
What's our cost PMPM for the last 12 complete months, broken down by month? Use the governed definition and show the member-month denominator logic.
```

### 4.3 · Readmissions

**Prompt for the learner to run:**
```
What's our 30-day readmission rate for the last complete quarter? Walk me through the governed definition — index admissions, exclusions, and the window.
```

### 4.4 · Utilization and risk

**Prompt for the learner to run:**
```
Show inpatient utilization per 1,000 members for the last year, and then the comorbidity/RAF profile of our highest-cost decile. Cite the governed surfaces you used.
```

> ✅ You'll see: utilization normalized the standard way, and risk wired to the official CMS HCC model — including the HCC hierarchy (trumping) and a by-HCC breakdown of what drives the score.

> **Know what RAF means here** — The governed surface computes the diagnosis-driven HCC portion of RAF. The full CMS RAF adds demographic (age/sex) and interaction terms — which require demographic fields your eligibility data may not carry. Ana will say so rather than fabricate; the decision record in ontology/notes/ documents the scope.

> **Why everyone gets the same number** — Metrics like PMPM and readmission can be computed several ways. The ontology pins one governed definition — with the decision recorded in ontology/notes/ — so Finance and Quality stop disagreeing.

### 4.5 · When the answer isn't governed yet — watch the model grow
Now ask something from your shortlist that the starter doesn't already cover. This is the important beat: a starter pack is a head start, not the finished model.

**Prompt for the learner to run:**
```
Here's a question from our shortlist that isn't in the governed surfaces yet: [your question]. Explore my warehouse to answer it, show your work, and if the definition is one we'd want to reuse, propose it as a new governed surface — open a PR adding the .tql and a notes file recording the decision.
```

> ✅ You'll see: Ana explore only the frontier (not re-derive the whole warehouse), answer, and propose a write-back — a new metric committed to your repo with provenance. Review and merge it, and the next person who asks gets the governed answer for free. That's the malleable loop: the ontology you ship is the one you grow, and it gets more complete every time you use it.

> **You ratify; high-stakes definitions get review** — Ana proposes; humans ratify via normal git review. Anything in governance-phi.md scope or a core metric should require review before merge (CODEOWNERS-style) — see STANDARDS.md . The point isn't to let an agent rewrite your model unsupervised; it's that discovered knowledge is captured instead of re-discovered next time.

**Checkpoint before moving on:**
- [ ] All four question families answered through governed surfaces, SQL shown
- [ ] You can point to the notes file explaining at least one metric's definition decision
- [ ] Ana proposed a write-back for a not-yet-governed question — and you saw it land as a PR

## Module 6 · Governance & PHI Defaults
*🎯 Goal: see the compliance behavior that's on by default — and verify it fires*

### 5.1 · Inventory your identifiers — day one
governance-phi.md §0 classifies every direct identifier in the connected schema into exactly one role — and the key distinction is that using an identifier as a join key is not the same as outputting it :

**Prompt for the learner to run:**
```
Inventory every direct identifier in the connected schema and classify each per governance-phi.md section 0: join-key-only, never-output, or aggregate-only. Flag anything ambiguous for compliance review.
```

> ✅ You'll see: a per-column classification your compliance team signs off on — the rules are templates tuned to your regime, and they can be tightened freely but never loosened without a reviewed, attributable decision.

> **Facilitators: pre-flight these tests** — Run 5.2 and 5.3 yourself before any session with compliance in the room. These guardrails are instruction-layer enforcement — they live in the governance context files Ana reads, which makes them verifiable and tightenable, but they depend on those files being attached and current. If a test doesn't fire: check that the ontology repo (with governance-phi.md and config/org_context.md ) is connected to the thread, and that your fork didn't drift from the governance defaults. Demonstrating the check is part of the story — "here's the file, here's the behavior, here's how we audit it."

### 5.2 · Test the small-cell rule

**Prompt for the learner to run:**
```
Break down [a rare condition] prevalence by county. Apply our suppression rules and tell me what you suppressed and why.
```

> ✅ You'll see: cells under min_cell_size suppressed with an explanation — including complementary cells that would let someone back out a suppressed value. The starter default is 11 (the CMS standard), configured in config/org_context.md ; common alternatives are 10 (some state agencies) or 30 (some internal policies). If suppression doesn't fire, don't move on — work the pre-flight check above; an unenforced rule you catch is a better demo than a rule you assumed.

### 5.3 · Test sensitive-diagnosis gating

**Prompt for the learner to run:**
```
Show me member-level detail for [a 42 CFR Part 2-protected category].
```

> ✅ You'll see: Ana decline or constrain the request per the gating rules — and point to the policy file that governs it.

**Checkpoint before moving on:**
- [ ] Small-cell suppression fired and was explained
- [ ] A sensitive-category request was gated, with the governing file cited

## Module 7 · Validate Numbers & Make It Yours
*🎯 Goal: pin known-correct values, then adapt the starter's definitions to your organization — in your repo*

### 6.1 · Run the A/B test — vanilla vs. ontology
The starter ships a measured proof: validation/ab-test-vanilla-vs-ontology.md — seven questions asked in two threads against the same connector , one with the ontology repo connected and one without, scored on correct / consistent / traceable . The governed answers are pre-filled from validated golden values.

**Prompt for the learner to run:**
```
Run the A/B test from validation/ab-test-vanilla-vs-ontology.md: I'll ask the seven questions in this thread (ontology connected). Answer each, cite the governed surface, and show the SQL — I'm scoring you on correct, consistent, and traceable.
```

> ✅ You'll see: the coded, multi-definition, and risk questions land exactly on the governed values, while the control question ("how many members total?") shows it's not magic — it's governance where governance matters. The headline: correct on the hard ones, consistent across phrasings, traceable every time.

### 6.2 · Run the golden queries
The starter's golden values are already pinned and verified against the demo dataset (see validation/golden-queries.md and the end-to-end script in validation/run-golden-queries.md ). Against your warehouse, re-pin them to numbers you trust:

**Prompt for the learner to run:**
```
Run the golden queries from validation/ against my warehouse. For each, compare to a reference number I trust and flag any drift. Where we differ, explain whether it's data, definition, or time-window.
```

> ✅ You'll see: accuracy checked, not asserted — and a triage of any mismatch into data vs. definition vs. window.

### 6.3 · Customize a definition
Your organization inevitably defines something differently — a readmission exclusion, a line-of-business filter, a different risk-model year.

**Prompt for the learner to run:**
```
Our official definition of [metric] differs from the starter: [your definition]. Update the governed surface in our repo, record the decision and the rejected default in a notes file, and open a PR. Add a golden-query test pinning a known value.
```

> ✅ You'll see: the change land as a reviewable PR in your repo — the template stays pristine upstream; your adaptations are yours.

### 6.4 · Localize the vocabulary
ontology/notes/glossary.md holds the canonical claims terms — member, member-month, claim, encounter, charge vs allowed vs paid, denial — each with a line-of-business variance column flagging where commercial, Medicare, and Medicaid deployments diverge (retroactive Medicaid eligibility, MA encounter completeness, HHS-HCC/CDPS instead of CMS-HCC…).

**Prompt for the learner to run:**
```
Walk the glossary's LOB-variance column for our line of business ([commercial / Medicare / Medicaid]). For each term that differs at our org, propose the override in glossary.md — keep the term → definition → source pattern — and open it as one PR.
```

> ✅ You'll see: the vocabulary localized in one reviewable pass — so "member month" and "allowed amount" mean your org's thing, everywhere, from now on.

> **Two habits as you make it yours** — 1 · Write for the search box. As you extend the kit, keep a short README per folder and repeat the phrases your teams actually use (metric names, synonyms, team names) in the prose — future threads find context by search , not browsing. 2 · Let usage drive the roadmap. Stand up a weekly gap-review playbook: mine repeated questions, manual SQL, and mid-thread corrections; have Ana draft small reviewable patches; a named owner approves. The kit is the seed — usage is what grows it. (See Ontology Operations Module 4.)

**Checkpoint before moving on:**
- [ ] Golden queries ran; any drift was triaged (data / definition / window)
- [ ] One definition is now yours — PR'd, noted, and pinned with a test

