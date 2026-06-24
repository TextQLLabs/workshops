# Provider Ops Starter — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Provider Ops Starter workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Provider Ops Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/provider-ops-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own encounter, census, OR, and revenue-cycle data" — mostly without leaving the Ana chat window.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Define Your North Star | Help me define the North Star before we build anything. Summarize my connected data in 3-4 lines (key tables, grain, domains — encounters, census, OR, ambulatory, revenue cycle), then ask me 5-7 sharp scoping questions a few at a time — who uses this, what decision it changes Monday morning, whether we're after capacity and patient flow, surgical and acuity performance, or revenue cycle and access, what "working" looks like in 30 days, where the data's messiest. From my answers, recommend the archetype that fits and draft north_star.md (one paragraph on what this ontology is for + the 6-8 questions it must answer in 30 days), and propose it as a reviewed change. |
| 2 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and classification dimensions are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 3 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Run validation/dry-run-prompt.md: pull the information schema for my encounter, patient, bed_census, or_case, appointment, and charge tables, tell me where the ontology's expected names don't match, and settle the grain — encounter spine vs. bed_census midnight snapshot vs. charge line, whether los_days is precomputed or derived, whether observation is separated from inpatient, and whether admit/discharge are real timestamps (ED LOS needs them). Propose the exact changes to ontology/schema.tql. |
| 4 | The Classification Layer | Using the ontology's classification layer, roll my specialty/department values up to service line, group my discharge_disposition values into the UB-04 grouping, and join my drg_code values to the CMS MS-DRG seed for MDC and relative weight. Explain how you joined the seed CSVs without writing to the warehouse. |
| 5 | Ask Governed Questions | What's our average inpatient length of stay for 2024 (length_of_stay.tql) and how many inpatient discharges does that cover (discharge_volume.tql)? Tell me the basis — encounter type, observation in/out, deaths/transfers in the denominator — and why. |
| 6 | Governance & PHI Defaults | Inventory every direct identifier in the connected schema and classify each per governance-phi.md section 0: join-key-only, never-output, HIPAA-sensitive (age/ZIP3/date), or minimum-necessary. Flag anything ambiguous for compliance review. |
| 7 | Validate Numbers & Make It Yours | Walk me through operational-vs-CMS readmission (notes/readmission-operational.md). Then check my data: confirm readmission_rate.tql computes the operational all-cause 30-day rate (index excludes deaths, any-cause inpatient readmit within 30 days), and if I have a condition flag + planned-admission flag, approximate a CMS-style condition-specific, planned-excluded rate to show how far it sits from the operational number. Then reconcile ALOS / occupancy / CMI / readmission to a number I trust and flag any drift (data / definition / basis). |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
