# P&C Insurance Starter — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the P&C Insurance Starter workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "P&C Insurance Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/insurance-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own policy, premium, and claims data" — mostly without leaving the Ana chat window.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Define Your North Star | Help me define the North Star before we build anything. Summarize my connected data in 3-4 lines (key tables, grain, domains), then ask me 5-7 sharp scoping questions a few at a time — who uses this, what decision it changes Monday morning, whether we're after loss-ratio/reserving parity, underwriting/pricing, or claims ops, what "working" looks like in 30 days, where the data's messiest. From my answers, recommend the archetype that fits and draft north_star.md (one paragraph on what this ontology is for + the 6-8 questions it must answer in 30 days), and propose it as a reviewed change. |
| 2 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and classification dimensions are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 3 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Run validation/dry-run-prompt.md: pull the information schema for my policy, premium, and claims tables, tell me where the ontology's expected names don't match, and settle the policy/coverage/claim/transaction grain. Propose the exact changes to ontology/schema.tql. |
| 4 | The Classification Layer | Using the ontology's classification layer, roll my granular line-of-business values up to the NAIC major line and group my top 10 cause-of-loss codes into peril groups (with the cat flag). Explain how you joined the seed CSVs without writing to the warehouse. |
| 5 | Ask Governed Questions | What's our loss ratio for accident year 2024? Use the governed definition (loss_ratio.tql) and tell me the basis — incurred or paid, earned or written premium, accident or calendar year — and why. |
| 6 | Governance & PII Defaults | Inventory every direct identifier in the connected schema and classify each per governance-pii.md section 0: join-key-only, never-output, sensitive, or aggregate-only. Flag anything ambiguous for compliance review. |
| 7 | Validate Numbers & Make It Yours | Walk me through written-vs-earned premium (notes/premium-definition.md), then prove the ~12× trap on my data: show that SUM(written_premium) off the monthly premium_earned series inflates vs. written counted once at the policy grain. Confirm earned_premium.tql sums the EARNED series for ratios. Then reconcile loss ratio / combined / reserves to a number I trust and flag any drift (data / definition / basis). |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
