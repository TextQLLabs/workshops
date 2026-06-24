# Biopharma Starter — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Biopharma Starter workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Biopharma Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/biopharma-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own Rx, sales, access, and field data" — mostly without leaving the Ana chat window. Biopharma **commercial** only; RWE reuses the sibling healthcare-claims starter, and R&D/CDISC is out of scope.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and classification are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 2 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Pull the information schema for my commercial tables (product, hcp, rx_fact, sales_fact, call_activity, formulary, …) and tell me where the ontology's expected table and column names don't match what I actually have. Propose the exact changes to ontology/schema.tql. Then confirm the Rx vendor + vintage and the HCP-identity crosswalk. |
| 3 | The Classification Layer | Using the ontology's classification layer, roll my top products up to molecule and therapeutic area and show which ATC class each falls into. Explain how you joined the crosswalk in your sandbox without writing to the warehouse. Then show one cut by territory/region and one by channel. |
| 4 | Ask Governed Questions | What's our TRx, NRx, and NBRx for [brand] over [period], trended monthly? Use the governed rx_volume surface, name the metrics, and pin the Rx data vintage. |
| 5 | Governance Defaults | Inventory every direct identifier in the connected schema and classify each per governance-hcp-pii.md section 0: join-key-only, never-output, or aggregate-only. Flag anything ambiguous for compliance review. |
| 6 | Validate Numbers & Make It Yours | Run the golden queries from validation/golden-queries.md against my warehouse. For each surface, compare to a reference number I trust, assert the invariants (nbrx ≤ nrx ≤ trx; net ≤ gross; share/access/reach ∈ [0,1]; sales_revenue ties out to gross_to_net), and triage drift into data / definition / window / Rx vintage. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
