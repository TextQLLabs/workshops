# MedTech Starter — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the MedTech Starter workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "MedTech Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/medtech-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own product, installed-base, service, and complaint data" — mostly without leaving the Ana chat window.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Define Your North Star | Help me define the North Star before we build anything. Summarize my connected data in 3-4 lines (key tables, grain, domains — product catalog, accounts, sales, installed base, field service, post-market complaints), then ask me 5-7 sharp scoping questions a few at a time — who uses this, what decision it changes Monday morning, whether we're after commercial performance, post-market quality and safety, or field service and reliability, what "working" looks like in 30 days, where the data's messiest. From my answers, recommend the archetype that fits and draft north_star.md (one paragraph on what this ontology is for + the 6-8 questions it must answer in 30 days), and propose it as a reviewed change. |
| 2 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and classification dimensions are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 3 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Run validation/dry-run-prompt.md: pull the information schema for my product, account, sales_txn, device, service_order, and complaint tables, tell me where the ontology's expected names don't match, and settle the grain — product (SKU) vs. device (installed unit) vs. service_order vs. complaint, whether device carries status ('active'/'decommissioned') and install_date so the active snapshot resolves, and whether is_corrective / first_visit_resolved / is_mdr / is_serious exist as booleans. Propose the exact changes to ontology/schema.tql. |
| 4 | The Classification Layer | Using the ontology's classification layer, roll my product_family values up to modality, map my device_class values to FDA risk class (I/II/III → low/moderate/high) via the 21 CFR 860 seed, and group my complaint_type values into the malfunction / injury / use-error grouping. Explain how you joined the seed CSVs without writing to the warehouse. |
| 5 | Ask Governed Questions | What's our product revenue for 2024 (product_revenue.tql) and how many active devices are in the installed base as of the reporting date (installed_base.tql)? Tell me the basis — which categories are in revenue (capital/consumable/service), and that installed base counts active units as-of the reporting date, not every device ever shipped — and why. |
| 6 | Governance & Quality-Record Defaults | Inventory every sensitive field in the connected schema and classify each per governance-quality.md section 0: join-key-only, confidential (account/customer/device detail), regulated quality record (21 CFR 820/803), or minimum-necessary. Flag anything ambiguous for compliance/quality review. |
| 7 | Validate Numbers & Make It Yours | Walk me through MDR reportability definition and timing (notes/governance-quality.md). Then check my data: confirm mdr_rate.tql computes the share of received complaints flagged is_mdr per 21 CFR 803, and if I have an adjudication-status field and a became-aware date, approximate a "decided-MDR within the 30-day clock" rate to show how far it sits from the raw is_mdr share. Then reconcile revenue / installed base / attach / complaint rate / MDR rate to a number I trust and flag any drift (data / definition / basis). |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
