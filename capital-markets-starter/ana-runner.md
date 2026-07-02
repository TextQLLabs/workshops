# Capital Markets Starter — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Capital Markets Starter workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Capital Markets Starter" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/capital-markets-starter/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop that takes you from "I have this ontology repo and nothing else" to "I'm asking Ana governed questions about my own trade, position, P&L, and risk data" — mostly without leaving the Ana chat window.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Six Layers | *(Ana orients you)* — explain **The Six Layers** in 3 bullets, grounded in my connected data, then move on. |
| 1 | Define Your North Star | Help me define the North Star before we build anything. Summarize my connected data in 3-4 lines (key tables, grain, domains — trades/executions, positions, daily P&L, market/credit risk, counterparties), then ask me 5-7 sharp scoping questions a few at a time — who uses this, what decision it changes Monday morning, whether we're after trading performance and P&L, market risk and limits, or exposure and concentration, what "working" looks like in 30 days, where the data's messiest. From my answers, recommend the archetype that fits and draft north_star.md (one paragraph on what this ontology is for + the 6-8 questions it must answer in 30 days), and propose it as a reviewed change. |
| 2 | Connect Three Things | Read the ontology repo and give me a tour: what entities, metrics, and classification dimensions are defined, and what governed questions am I able to ask once my warehouse is validated? |
| 3 | Validate Against Your Schema | Look at the ontology repo, then inspect my warehouse. Run validation/dry-run-prompt.md: pull the information schema for my trade, position, pnl, risk, instrument, book, and counterparty tables, tell me where the ontology's expected names don't match, and settle the grain — trade (execution) vs. position snapshot (one row per book × instrument × as_of_date, SIGNED market_value + long / − short) vs. pnl/risk daily — whether trade.notional is precomputed or derived from quantity × price, whether market_value is signed, and whether exposure is a snapshot-as-of or a window question. Propose the exact changes to ontology/schema.tql. |
| 4 | The Classification Layer | Using the ontology's classification layer, roll my instrument asset_class values up to super-asset-class (equities / fixed_income / macro) and desk grouping, classify counterparty rating into IG vs. HY with the numeric notch, and group venue codes into lit / dark / OTC with MIC region. Explain how you joined the seed CSVs without writing to the warehouse. |
| 5 | Ask Governed Questions | What's our trading volume for 2024 (trading_volume.tql) — notional and trade count — and trading P&L over the same window (trading_pnl.tql), split realized vs. unrealized? Then position value and gross/net exposure as of 2024-12-31 (position_value.tql, gross_net_exposure.tql) — confirm market_value is SIGNED (net = SUM, gross = SUM(ABS)) over one snapshot. Tell me the basis for each — window vs. snapshot-as-of, realized vs. unrealized — and why. |
| 6 | Governance & MNPI Defaults | Inventory every direct identifier in the connected schema and classify each per governance-mnpi.md section 0: join-key-only, minimum-necessary, information-barrier-gated (desk Chinese wall), or MNPI-never-output. Flag anything ambiguous for compliance / surveillance review. Then break notional + counterparty exposure down by counterparty × asset class × desk and confirm small-cell suppression fires (no single small counterparty named) and cross-wall desk detail is blocked. |
| 7 | Validate Numbers & Make It Yours | Walk me through the VaR methodology / limit-basis distinction (notes/var-utilization.md): 1-day 95% management VaR over the approved book limit vs. a 10-day 99% regulatory / FRTB-ES basis or a firm-level limit. Then check my data: confirm var_utilization.tql computes SUM(var_95)/SUM(var_limit) on a single risk_date, and if I have a different-horizon VaR or firm-level limit, approximate a regulatory-style utilization to show how far it sits from the management number. Then reconcile trading P&L / gross-net exposure / VaR utilization / concentration to a number I trust and flag any drift (data / definition / basis — signed vs. unsigned MV, gross vs. net, VaR horizon/limit, 252 vs. 260 annualization). |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
