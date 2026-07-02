# Genomics Starter — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Genomics Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/genomics-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Six Layers
*🎯 Goal: know what's in the box and where everything lives*

> **Standards alignment** — STANDARDS.md maps the model to the genomics standards it aligns with (GA4GH, VCF, HGVS, HGNC, ClinVar, Sequence Ontology); SOURCES.md cites every source. The semantic layer (metrics, classification) is fully separated from the physical mapping : every physical table name lives in one file , ontology/schema.tql — re-point it and the metric logic stays put. The starter is authored against a generic LIMS + secondary/tertiary-analysis model in ANSI/Spark-portable SQL; MIGRATION.md is the re-point checklist and works the same on Redshift, BigQuery, Snowflake, or Databricks (budget: about a half-day with warehouse access). For a deep technical tour, read DEEP_DIVE.md .

> **Two rules for a long, live session** — 1 · Checkpoint every couple of modules. Long threads have a ceiling. After every module or two, ask Ana: “Save a handoff document summarizing what we've built, what we decided, and what's next — so we can continue in a new thread.” If a thread ever maxes out, you lose nothing. 2 · Pin the scope in every prompt. Name the entity and the source-of-truth tables in each prompt (“…for [entity X], using the [base] tables, not the summary table”) — otherwise Ana may drift to a convenient summary table or query every source at once.

**Checkpoint before moving on:**
- [ ] You can name the six layers and find each one in the repo
- [ ] You know which classification schemes ship in the box vs. need a per-release fetch (full ClinVar, HGNC, GENCODE)
- [ ] You know the default model (generic LIMS + analysis, ANSI/Spark-portable) and where the re-point checklist lives

## Module 1 · Define Your North Star

**Prompt for the learner to run:**
```
Help me define the North Star for our ontology before we build anything.
1. Look at the data connected to this thread and summarize in 3-4 lines what we have: the key tables, the grain, and the domains it covers.
2. Then ask me 5-7 sharp scoping questions to pin down what we are really doing - who will use this, what decision it changes on Monday morning, whether we are after variant/diagnostic yield / cohort and biobank feasibility / lab QC and TAT, what "working" looks like in 30 days, and where our data is messiest. Ask a few at a time, not all at once.
3. From my answers plus the data, recommend the archetype that fits (A variant/diagnostic yield, B cohort/biobank feasibility, or C lab QC and TAT) and draft our North Star: one short paragraph on what this ontology is for, plus the 6-8 questions it must answer in 30 days.
4. Save it as north_star.md in the ontology and propose it as a reviewed change, so every later step builds toward it.
```

> ✅ You will see: Ana summarize your data, ask scoping questions, recommend an archetype with a reason, and draft a north_star.md — a paragraph plus the questions that define "done." Review it, push back, ratify.

> **Why a prompt, not a 50-question checklist** — No giant pre-written question bank that nobody maintains. Ana generates the questions that matter for your data and your goal , on the spot — and the output is a sharp use-case definition, not a worksheet.

**Checkpoint before moving on:**
- [ ] You have a written north_star.md (purpose + the 6-8 questions it must answer)
- [ ] You picked an archetype (A / B / C), or Ana recommended one and you agreed
- [ ] It names something real that changes on Monday morning, not "model all of genomics"

## Module 2 · Connect Three Things
*🎯 Goal: ontology repo + warehouse + documents connected — then everything else happens in chat*

> **No second source of truth** — You don't copy anything into Ana. She reads the repo live; when the repo changes, Ana sees the change.

> **Read-only, against the analytics copy** — CLIA/CAP pipelines are validated, controlled environments — analytics must never write to them, and genomic data must not be needlessly copied. Connect read-only to the analytics warehouse, not the production pipeline. See ontology/notes/governance-genomic-pii.md .

**Prompt for the learner to run:**
```
Read the ontology repo and give me a tour: what entities, metrics, and classification groupers are defined, and what governed questions am I able to ask once my warehouse is validated?
```

> ✅ You'll see: Ana describe the model from the repo itself — proof the Git connector works and the ontology is being read as ground truth.

**Checkpoint before moving on:**
- [ ] Ana described the starter's entities and metrics from the connected repo
- [ ] Your warehouse connector is attached, read-only, against the analytics copy (not the validated pipeline)
- [ ] You know which of your documents you'll bring in (or that you're skipping this)

## Module 3 · Validate Against Your Schema
*🎯 Goal: before trusting numbers, prove the ontology's assumptions match your actual tables — without writing SQL*

**Prompt for the learner to run:**
```
Look at the ontology repo, then inspect my warehouse. Pull the information schema for my subject / sample / assay / qc_metric / variant tables and tell me where the ontology's expected table and column names don't match what I actually have. Propose the exact changes to ontology/schema.tql.
```

> ✅ You'll see: Ana discover your schema, diff it against the ontology, and hand you a precise list of fixes — table backings, column names. (A ready-made version of this check lives in validation/dry-run-prompt.md — it also pulls the genome build, ClinVar version, QC/filter enums, and the data-extract date.)

**Prompt for the learner to run:**
```
Run validation/validate_tql.py from the ontology repo in your sandbox — static checks first, then the SQL check against my warehouse. Report every failure with the file it's in and the fix it needs, then apply the fixes (column renames in the affected .tql, table renames in schema.tql only) and re-run until clean.
```

> ✅ You'll see: the typo'd-backing, wrong-alias, and missing-column bug classes caught here — instead of surfacing as wrong numbers in front of stakeholders. (This is the same kind of check that caught the kit's own gene -vs- gene_symbol parameter shadow during validation — Module 7.)

> **Prefer the terminal?** — The same gate runs locally: python3 validation/validate_tql.py (static — no warehouse needed) · --check-sql (paste the output into Ana: rows = missing columns) · --dsn "<dsn>" --explain (live column check + compile test).

**Prompt for the learner to run:**
```
Make those changes and open a pull request.
```

> ✅ You'll see: Ana edit the files and open a reviewable PR in your repo. Every physical table name lives in one place ( ontology/schema.tql ) — re-point it and the metric logic stays put. The surfaces reference logical ${name} backings, so most re-points are one file.

> **Why this step matters** — Every lab's warehouse differs from the reference shape somewhere — a renamed column, a missing table, a different grain. Finding those before you trust a number is the difference between a defensible metric and a debugging session in front of stakeholders.

**Prompt for the learner to run:**
```
Read ontology/notes/identity-resolution.md, then inspect my tables. Run COUNT(*) samples vs COUNT(DISTINCT subject_id) subjects — how heavy is the re-draw / tumor-normal / longitudinal fan-out? Tell me which metrics should be per-sample (lab ops) vs per-subject (clinical yield, feasibility), confirm the subject and sample keys are distinct, and record the decision in databases/[ourschema]/README.md (copy databases/lims_core/ as the template).
```

> ✅ You'll see: the grain decision made and written down before anything gets edited. This is the make-or-break of feasibility counts: one subject has many samples (re-draws, tumor + matched normal, timepoints), so counting specimens when you mean people inflates everything.

**Prompt for the learner to run:**
```
Per ontology/notes/coding-tuple.md, a variant is a tuple — genome build + coordinates + ref/alt + ClinVar version — not a string. Find where my warehouse records the genome build (GRCh37 vs GRCh38) and the ClinVar release/date, confirm the variant.filter PASS convention, and confirm genes are stored as HGNC approved symbols (not aliases). Record each in databases/[ourschema]/README.md and flag anything missing — an HGVS/ClinVar result without its build + version is a half-citation.
```

> ✅ You'll see: the build and ClinVar vintage pinned, the PASS convention confirmed, and the gene-symbol layer checked — the things the validator can't know but that make yield comparable across a period.

**Prompt for the learner to run:**
```
Find the dataset-specific literals the surfaces assume — sample.status values, qc_status (pass|fail|warn), clinical_significance tiers, and the per-assay coverage / call-rate thresholds — check each against what's actually in my warehouse, and propose the corrections in the same PR. Flag any single blended coverage cut across WGS and targeted panels.
```

> ✅ You'll see: the enumerations the validator can't know, corrected from your real data — and a warning if a threshold is being blended across assay types that aren't comparable (30x WGS vs. 100x+ panel).

> **The full checklist** — This module covers the core; MIGRATION.md is the complete re-point (discover → grain → schema.tql → tuple/build → validator → enums/thresholds → governance → goldens). Half a day with warehouse access; most of it is verification, not editing.

> **Different warehouse dialect?** — The starter is authored in ANSI/Spark-portable SQL (it validates on Databricks). On Snowflake, BigQuery, or another engine , extend the dry-run ask: "…also flag any Spark-specific SQL in the .tql surfaces (e.g. DATEDIFF , PERCENTILE_APPROX ) and propose [dialect] equivalents." Dialect adaptation rides the same PR loop as schema fixes.

**Checkpoint before moving on:**
- [ ] Ana produced a concrete mismatch list (or confirmed a clean match)
- [ ] The sample-vs-subject grain decision is made and written down
- [ ] Genome build + ClinVar version are pinned; PASS convention confirmed
- [ ] The fixes landed as a PR you can review — not silent edits

## Module 4 · The Classification Layer
*🎯 Goal: grouper-powered questions — ClinVar tiers, Sequence-Ontology type, HGNC gene — with zero writes to your warehouse*

**Prompt for the learner to run:**
```
Using the ontology's classification layer, show me the distribution of my PASS variants by ClinVar 5-tier clinical significance, and by Sequence-Ontology variant group (short vs. structural). Explain how you joined the seed crosswalk without writing to the warehouse.
```

> ✅ You'll see: raw variant attributes resolved to meaningful clinical categories (P / LP / VUS / LB / B and SNV / indel / CNV / SV), with the federated join-in-sandbox pattern explained.

> **Group on the HGNC approved symbol — not an alias** — When you group "which genes carry the most pathogenic variants," group on the HGNC approved symbol via terminology.hgnc , never a legacy/alias symbol. Gene symbols get renamed, and aliases collide — a renamed symbol silently drops rows. This is the same coding-discipline lesson that bit the kit's own validation (Module 7).

**Prompt for the learner to run:**
```
Run reference/terminology/load_terminology.py in your sandbox to regenerate the clinical_significance / variant_type / assay_type seed CSVs, then note the current public ClinVar / HGNC / Sequence-Ontology release sources (load_terminology.py --refs). Open a PR with the refreshed files and a summary of what changed.
```

**Prompt for the learner to run:**
```
We need variant-level ClinVar significance, not just the 5-tier scheme. Fetch the current ClinVar release, pin it to our genome build (GRCh38), and confirm we're committing the version + date as part of the citation per notes/coding-tuple.md — not hand-bundling the full resource.
```

> ✅ You'll see: refreshed classification arriving as a reviewable PR — same governance motion as everything else, with the ClinVar version and genome build pinned as part of the citation.

**Checkpoint before moving on:**
- [ ] A classification question worked against your data with no warehouse writes
- [ ] You can explain the federated join-in-sandbox pattern in one sentence
- [ ] You know how the seeds refresh and how the full ClinVar release gets fetched and version-pinned

## Module 5 · Ask Governed Questions
*🎯 Goal: the payoff — consistent, defensible answers routed through governed definitions, with the SQL shown*

> **Pin the scope** — In every question below, name the entity and the source-of-truth tables . A plausible answer from the wrong (summary) table is worse than no answer — if two sources could answer, run both and let your SME rule which is truth.

**Prompt for the learner to run:**
```
What's our PASS variant yield per sequenced sample? Use the governed variant_yield surface, tell me the PASS filter you applied, and confirm the denominator is sequenced samples (so 0-variant samples count).
```

> ✅ You'll see: variant_yield filter to filter = 'PASS' before counting anything — no-call/low-confidence records excluded. On the kit's synthetic warehouse: 254,775 PASS variants / 18,972 sequenced = 13.43 variants/sample . A shift in this number flags a pipeline, reference-build, or sample-quality change — not noise.

**Prompt for the learner to run:**
```
What's our diagnostic yield — the % of sequenced samples with at least one PASS pathogenic / likely-pathogenic variant? Use the governed pathogenic_variant_rate surface. State whether VUS is excluded and which ClinVar version the significance calls came from.
```

> ✅ You'll see: pathogenic_variant_rate count clinical_significance IN ('pathogenic','likely_pathogenic') , PASS-only, with VUS excluded by default (reportable = P/LP). On the synthetic warehouse: 6,145 / 18,972 = 0.3239 diagnostic yield . Restrict to a gene of interest with the gene_symbol parameter (the one that was renamed during validation — Module 7).

> **Name the ClinVar version and genome build** — ClinVar reclassifies continuously — a VUS can become pathogenic. A pathogenic rate is only comparable within one classification vintage , so the governed surface expects the ClinVar version + date and the genome build as part of the citation ( notes/coding-tuple.md , notes/variant-analysis.md ). Ana will name them rather than report a bare rate.

**Prompt for the learner to run:**
```
Show our QC pass rate for the last complete year, and our turnaround time (average and p90 days, collection → result) for samples resulted in that window. Use the governed qc_pass_rate and turnaround_time surfaces, and report p90 — not just the mean.
```

> ✅ You'll see: qc_pass_rate at 17,552 / 20,000 = 0.8776 and turnaround_time at avg 20.06 days, p90 26 days on the synthetic warehouse. TAT is anchored on result_date over resulted samples only — in-flight samples have no result date and are excluded (or reported separately as backlog), or TAT is understated.

**Prompt for the learner to run:**
```
Show coverage adequacy (% of samples meeting min_coverage=30), average genotype call rate and % of samples ≥ 0.95, and our assay mix (share of WGS / WES / panel / RNA). Cite the governed surfaces, and segment coverage by assay type rather than blending a single cut.
```

> ✅ You'll see: coverage_adequacy at 18,243 / 20,000 = 0.9122 (avg 74.6x), call_rate avg 0.9491 , and assay_mix summing to 1.0 (panel 0.397 · WGS 0.206 · WES 0.200 · RNA 0.197). Thresholds are assay-specific — a single coverage or call-rate cut across WGS and targeted panels is meaningless, so Ana segments by assay_type .

> **Why everyone gets the same number** — Metrics like variant yield and diagnostic yield can be computed several ways — which PASS filter, which significance tiers, which denominator. The ontology pins one governed definition — with the decision recorded in ontology/notes/variant-analysis.md and lab-ops.md — so the variant team and the lab director stop disagreeing.

**Prompt for the learner to run:**
```
Here's a question from our shortlist that isn't in the governed surfaces yet: [your question]. Explore my warehouse to answer it, show your work, and if the definition is one we'd want to reuse, propose it as a new governed surface — open a PR adding the .tql and a notes file recording the decision (PASS-only, genome build, and ClinVar version where relevant).
```

> ✅ You'll see: Ana explore only the frontier (not re-derive the whole warehouse), answer, and propose a write-back — a new metric committed to your repo with provenance. Review and merge it, and the next person who asks gets the governed answer for free. That's the malleable loop: the ontology you ship is the one you grow, and it gets more complete every time you use it.

> **You ratify; high-stakes definitions get review** — Ana proposes; humans ratify via normal git review. Anything in governance-genomic-pii.md scope (subject-linked output, consent, clinical-vs-research) or a core metric should require review before merge (CODEOWNERS-style) — see STANDARDS.md . The point isn't to let an agent rewrite your model unsupervised; it's that discovered knowledge is captured instead of re-discovered next time.

**Checkpoint before moving on:**
- [ ] The variant, diagnostic-yield, and lab-ops surfaces answered through governed definitions, SQL shown
- [ ] Each variant answer was PASS-only and came with its genome build + ClinVar version
- [ ] Ana proposed a write-back for a not-yet-governed question — and you saw it land as a PR

## Module 6 · Governance Defaults
*🎯 Goal: see the compliance behavior that's on by default — and verify it fires*

**Prompt for the learner to run:**
```
Inventory every direct identifier in the connected schema and classify each per governance-genomic-pii.md section 0: join-key-only, sensitive/gated (raw variant calls), or aggregate-only. Flag anything that would let a query emit a subject-linked variant list for compliance review.
```

> ✅ You'll see: a per-column classification your privacy/compliance team signs off on — the rules are templates tuned to your regime (and consent model), and they can be tightened freely but never loosened without a reviewed, attributable decision.

> **Facilitators: pre-flight these tests** — Run 5.2 and 5.3 yourself before any session with compliance in the room. These guardrails are instruction-layer enforcement — they live in the governance context files Ana reads, which makes them verifiable and tightenable, but they depend on those files being attached and current. If a test doesn't fire: check that the ontology repo (with governance-genomic-pii.md and config/org_context.md ) is connected to the thread, and that your fork didn't drift from the governance defaults. Demonstrating the check is part of the story — "here's the file, here's the behavior, here's how we audit it."

**Prompt for the learner to run:**
```
How many subjects carry a PASS pathogenic / likely-pathogenic variant in [a rare gene], broken down by ancestry or sex? Apply our suppression rules and tell me what you suppressed and why.
```

> ✅ You'll see: subject counts below min_cell_size suppressed (shown as <n ) with an explanation — because a lone carrier of a rare pathogenic variant is identifiable. The starter default is 5 , configured in config/org_context.md ; this applies hard to rare-variant / rare-gene cuts. If suppression doesn't fire, don't move on — work the pre-flight check above; an unenforced rule you catch is a better demo than a rule you assumed.

**Prompt for the learner to run:**
```
List the individual subjects and their specific pathogenic variant calls (HGVS) in [a gene].
```

> ✅ You'll see: Ana decline or constrain the request per §0 and §2 — default surfaces are aggregate, subject-linked variant lists are gated, and analysis is bounded by the participant's consent scope (research vs. clinical, primary vs. secondary findings). Incidental/secondary findings (ACMG SF) route to genetic counseling, not open analytics. Ana points to the policy file that governs it.

**Checkpoint before moving on:**
- [ ] Small-cell suppression fired on a rare-variant subject count and was explained
- [ ] A subject-linked variant-list request was gated, with the governing file (and consent scope) cited

## Module 7 · Validate Numbers & Make It Yours
*🎯 Goal: pin known-correct values, then adapt the starter's definitions to your lab — in your repo*

**Prompt for the learner to run:**
```
Run the golden queries from validation/golden-queries.md against my warehouse. For each surface, compare to a reference number I trust and flag any drift, and assert the invariants (assay_mix sums to 1.0; pathogenic ≤ sequenced; rates ∈ [0,1]; p90 TAT ≥ avg TAT; no surface emits a subject-linked call). Where we differ, explain whether it's data, definition, genome build, or ClinVar version.
```

> ✅ You'll see: accuracy checked, not asserted — and a triage of any mismatch into data vs. definition vs. build/version. The kit's pinned set to reconcile against: QC pass 0.8776 , variant yield 13.43 /sample, diagnostic yield 0.3239 , coverage adequacy 0.9122 , p90 TAT 26 days , rerun rate 0.0810 .

> **Why this is the teaching example** — A parameter named the same as a logical backing compiles fine but resolves the wrong thing at query time — a silent wrong-number bug, not a crash. It's the genomics version of the lesson that runs through the whole kit: get the names right (PASS filter, genome build, ClinVar version, HGNC approved symbol), because the data won't tell you when a name is quietly shadowing something else. (Other good first customizations: a stricter PASS-only filter, or a genome-build guard that refuses to pool GRCh37 and GRCh38.)

**Prompt for the learner to run:**
```
Our official definition of [metric] differs from the starter: [your definition]. Update the governed surface in our repo, record the decision and the rejected default in a notes file, and open a PR. Add a golden-query entry pinning a known value (with our genome build + ClinVar version where relevant).
```

> ✅ You'll see: the change land as a reviewable PR in your repo — the template stays pristine upstream; your adaptations are yours. Watch for parameter-vs-backing name collisions like the gene_symbol one — the validator and a careful review catch them.

**Prompt for the learner to run:**
```
Walk the glossary's variance-to-check column. For each term that differs at our lab — TAT anchor, per-assay coverage/call-rate thresholds, our "reportable" / VUS policy, sample-vs-subject grain — propose the override in glossary.md (keep the term → definition → resolves-via pattern) and open it as one PR.
```

> ✅ You'll see: the vocabulary localized in one reviewable pass — so "reportable," "QC pass," and "TAT" mean your lab's thing, everywhere, from now on.

**Checkpoint before moving on:**
- [ ] Golden queries ran; any drift was triaged (data / definition / build / ClinVar version)
- [ ] You can explain the gene → gene_symbol shadow bug and why it was silent
- [ ] One definition is now yours — PR'd, noted, and pinned with a golden value

