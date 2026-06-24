# Wealth & Asset Management Starter — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Wealth & Asset Management Starter workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Wealth Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/wealth-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own custody and portfolio data" — mostly without leaving the Ana chat window.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Define Your North Star | Help me define the North Star before we build anything. Look at the connected data and summarize what we have in 3-4 lines, then ask me 5-7 sharp scoping questions (who uses this, what changes on Monday, whether we're after BI parity / portfolio risk & concentration / household rollup & suitability, what "working" looks like in 30 days, where the data is messiest) a few at a time. Then recommend the archetype that fits and draft a north_star.md — one paragraph on what this ontology is for plus the 6-8 questions it must answer — and propose it as a reviewed change. |
| 2 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and classification groupers are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 3 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Pull the information schema for my custody / portfolio-accounting tables and tell me where the ontology's expected table and column names don't match what I actually have. Propose the exact changes to ontology/schema.tql. |
| 4 | The Classification Layer | Using the ontology's classification layer, show me my firm-wide asset-class allocation, then roll the equity sleeve up to its GICS sectors. Explain how you joined the asset-class and GICS-structure seeds without writing to the warehouse. |
| 5 | Ask Governed Questions | What's our firm-wide AUM as of the horizon? Use the governed point-in-time definition, then show me the average-12m (billing/KPI) basis too — and explain why they differ. |
| 6 | Governance Defaults | Inventory every direct identifier in the connected schema and classify each per governance-mnpi-pii.md section 0: join-key-only, never-output, or internal. Flag anything ambiguous for compliance review. |
| 7 | Validate Numbers & Make It Yours | Run the golden queries from validation/golden-queries.md against my warehouse — AUM, net flows, TWR, allocation, concentration, advisor book, fee rate. For each, compare to a reference number I trust and flag any drift, explaining whether it's data, definition, or window. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
