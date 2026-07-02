# Genomics Starter — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Genomics Starter workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Genomics Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/genomics-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own LIMS, QC, and variant data" — mostly without leaving the Ana chat window. Genomic data is inherently identifying; default surfaces stay aggregate and PASS-only.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and classification groupers are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 2 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Pull the information schema for my subject / sample / assay / qc_metric / variant tables and tell me where the ontology's expected table and column names don't match what I actually have. Confirm the genome build, ClinVar version, and the variant.filter PASS convention. Propose the exact changes to ontology/schema.tql. |
| 3 | The Classification Layer | Using the ontology's classification layer, show me the distribution of my PASS variants by ClinVar 5-tier clinical significance and by Sequence-Ontology variant group. Explain how you joined the seed crosswalk without writing to the warehouse. |
| 4 | Ask Governed Questions | What's our PASS variant yield per sequenced sample, and our diagnostic yield (% of sequenced samples with ≥1 PASS pathogenic/likely-pathogenic variant)? Use the governed surfaces, keep it PASS-only, and name the genome build + ClinVar version. |
| 5 | Governance Defaults | Inventory every direct identifier in the connected schema and classify each per governance-genomic-pii.md section 0: join-key-only, sensitive/gated (raw variant calls), or aggregate-only. Flag anything that would emit a subject-linked variant list, and confirm small-cell suppression on subject counts. |
| 6 | Validate Numbers & Make It Yours | Run the golden queries from validation/golden-queries.md against my warehouse: compare each surface to a number I trust, assert the invariants (assay_mix sums to 1.0; pathogenic ≤ sequenced; rates ∈ [0,1]; no subject-linked calls), and triage any drift into data / definition / genome build / ClinVar version. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
