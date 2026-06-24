# FinServ Starter — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the FinServ Starter workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "FinServ Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/finserv-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own customers, accounts, and transactions" — mostly without leaving the Ana chat window.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and classification systems are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 2 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Pull the information schema for my core-banking tables and tell me where the ontology's expected table and column names don't match what I actually have. Propose the exact changes to ontology/schema.tql. |
| 3 | The Classification Layer | Using the ontology's classification layer, show me which spend category my top 10 most frequent transaction MCCs fall into. Explain how you joined the MCC crosswalk without writing to the warehouse. |
| 4 | Ask Governed Questions | What's our loan delinquency rate? Use the governed definition and tell me the basis and threshold you used and why — then show me how the number moves on account-count vs dollar, and at 60+ / 90+ DPD. |
| 5 | Governance & PII Defaults | Inventory every sensitive identifier in the connected schema and classify each per governance-pii.md: mask-to-last-4 (PANs), never-output (account#/SSN/TIN), aggregate-only (customer-level NPI), or protected-class (never in credit logic). Flag anything ambiguous for compliance review. |
| 6 | Validate Numbers & Make It Yours | Run the 8 governed surfaces from validation/golden-queries.md against my warehouse. For each, compare to a reference number I trust and flag any drift. Confirm the invariants (every rate in [0,1]; delinquency ≥ charge-off; products-per-customer ≥ 1), then pin my values. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
