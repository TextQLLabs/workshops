# The Healthcare Starter Pack — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). Module list for the The Healthcare Starter Pack workshop.
> Delivery: **inline** or give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "The Healthcare Starter Pack" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/healthcare-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own claims and clinical data — and the model grows itself as I work" — mostly without leaving the Ana chat window.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Define Your North Star | Help me define the North Star for our ontology before we build anything. 1. Look at the data connected to this thread and summarize in 3-4 lines what we have: the key tables, the grain, and the domains it covers. 2. Then ask me 5-7 sharp scoping questions to pin down what we are really doing - who will use this, what decision it changes on Monday morning, whether we are after BI parity / care-gap and quality / risk adjustment / operational queries, what "working" looks like in 30 days, and where our data is messiest. Ask a few at a time, not all at once. 3. From my answers plus the data, recommend the archetype that fits (A BI parity, B care-gap/quality, or C risk adjustment) and draft our North Star: one short paragraph on what this ontology is for, plus the 6-8 questions it must answer in 30 days. 4. Save it as north_star.md in the ontology and propose it as a reviewed change, so every later step builds toward it. |
| 2 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and terminology groupers are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 3 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Pull the information schema for my claims tables and tell me where the ontology's expected table and column names don't match what I actually have. Propose the exact changes to ontology/schema.tql. |
| 4 | The Terminology Layer | Using the ontology's terminology layer, show me which CCSR category my top 10 most frequent diagnosis codes fall into. Explain how you joined the crosswalk without writing to the warehouse. |
| 5 | Ask Governed Questions | How many members have diabetes? Use the governed prevalence definition and tell me which codes/grouper you used and why. |
| 6 | Governance & PHI Defaults | Inventory every direct identifier in the connected schema and classify each per governance-phi.md section 0: join-key-only, never-output, or aggregate-only. Flag anything ambiguous for compliance review. |
| 7 | Validate Numbers & Make It Yours | Run the A/B test from validation/ab-test-vanilla-vs-ontology.md: I'll ask the seven questions in this thread (ontology connected). Answer each, cite the governed surface, and show the SQL — I'm scoring you on correct, consistent, and traceable. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
