# Real Estate Starter — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Real Estate Starter workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Real Estate Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/realestate-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own property, lease, and ledger data" — mostly without leaving the Ana chat window.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Define Your North Star | Help me define the North Star before we build anything. Summarize my connected data in 3-4 lines (key tables, grain, domains), then ask me 5-7 sharp scoping questions a few at a time — who uses this, what decision it changes Monday morning, whether we're after portfolio/asset reporting parity, revenue and collections, or operations and leasing, what "working" looks like in 30 days, where the data's messiest. From my answers, recommend the archetype that fits and draft north_star.md (one paragraph on what this ontology is for + the 6-8 questions it must answer in 30 days), and propose it as a reviewed change. |
| 2 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and classification dimensions are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 3 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Run validation/dry-run-prompt.md: pull the information schema for my property, unit, lease, ledger, and property-financial tables, tell me where the ontology's expected names don't match, and settle the grain — including whether unit status is a point-in-time snapshot or a period series. Propose the exact changes to ontology/schema.tql. |
| 4 | The Classification Layer | Using the ontology's classification layer, roll my granular property-type values up to sector (residential/commercial/industrial) and group my charge_type values into the revenue/recoverable grouping. Explain how you joined the seed CSVs without writing to the warehouse. |
| 5 | Ask Governed Questions | What's our physical occupancy as of the reporting date? Use the governed definition (occupancy_rate.tql) and tell me the basis — physical vs. economic, and whether down/offline units are in the denominator — and why. |
| 6 | Governance & PII Defaults | Inventory every direct identifier in the connected schema and classify each per governance-pii-fairhousing.md section 0: join-key-only, never-output, fair-housing-sensitive, or aggregate-only. Flag anything ambiguous for compliance review. |
| 7 | Validate Numbers & Make It Yours | Walk me through physical-vs-economic occupancy (notes/occupancy-rent.md). Then check my data: confirm occupancy_rate.tql computes physical occupancy (occupied/rentable, down units excluded), and compute economic occupancy (collected/contractual rent over gross potential at market, net of concessions) to show how far it sits below physical. Then reconcile NOI / occupancy / collections to a number I trust and flag any drift (data / definition / basis). |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
