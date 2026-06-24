# Clinical Trials Starter — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Clinical Trials Starter workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Clinical Trials Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/clinical-trials-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0. Keep everything arm-agnostic (blinded).
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own studies, sites, and subjects — without unblinding anyone" — mostly without leaving the Ana chat window.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and classification are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 2 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Pull the information schema for my study/site/subject and event tables and tell me where the ontology's expected table and column names don't match what I actually have. Confirm the subject-vs-visit grain and set analysis_end_date to my data cut. Propose the exact changes to ontology/schema.tql. |
| 3 | The Classification Layer | Using the ontology's classification layer, show me my adverse events grouped by CTCAE severity grade (1–5) with labels, and my subjects grouped by status. Explain how you joined the seed CSVs without writing to the warehouse. |
| 4 | Ask Governed Questions | How is each study tracking to its planned enrollment? Use the governed enrollment_vs_target surface, and tell me what "enrolled" means in the definition you used. (Then walk screen-fail, velocity, AE/SAE, deviations, query aging — all arm-agnostic.) |
| 5 | Governance Defaults | Break down the AE rate by treatment arm for [a study] — and inventory every direct identifier per governance-blinding-pii.md section 0. (Ana should DECLINE the by-arm split, cite the blinding rule, and classify identifiers join-key-only / never-output / aggregate-only / blinded-gated.) |
| 6 | Validate Numbers & Make It Yours | Run the nine governed surfaces against my warehouse as golden queries; compare each to a number a clin-ops/DM owner trusts and triage any drift into data / definition / data-cut. Then help me customize the "enrolled" funnel denominator in our repo. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology. Reminder: nothing is ever split by treatment arm in a blinded trial.
