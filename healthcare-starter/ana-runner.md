# Healthcare Starter — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Healthcare Starter workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Healthcare Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/healthcare-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own claims and clinical data" — mostly without leaving the Ana chat window.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and terminology groupers are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 2 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Pull the information schema for my claims tables and tell me where the ontology's expected table and column names don't match what I actually have. Propose the exact changes to ontology/schema.tql. |
| 3 | The Terminology Layer | Using the ontology's terminology layer, show me which CCSR category my top 10 most frequent diagnosis codes fall into. Explain how you joined the crosswalk without writing to the warehouse. |
| 4 | Ask Governed Questions | How many members have diabetes? Use the governed prevalence definition and tell me which codes/grouper you used and why. |
| 5 | Governance & PHI Defaults | Inventory every direct identifier in the connected schema and classify each per governance-phi.md section 0: join-key-only, never-output, or aggregate-only. Flag anything ambiguous for compliance review. |
| 6 | Validate Numbers & Make It Yours | Run the A/B test from validation/ab-test-vanilla-vs-ontology.md: I'll ask the seven questions in this thread (ontology connected). Answer each, cite the governed surface, and show the SQL — I'm scoring you on correct, consistent, and traceable. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
