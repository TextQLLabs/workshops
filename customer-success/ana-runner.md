# Customer Success — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Customer Success workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Customer Success" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/customer-success/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A workshop of short, hands-on modules for CSMs, CS leaders, and CS ops: one-prompt account health snapshots, QBR prep that writes itself, churn signals with root cause, renewal and expansion views, and a book of business that reports on itself weekly. Non-technical — if you can write an email, you…

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | Map Your Customer Data | Describe the customer data you can see: which source holds accounts and renewal dates, which holds product usage, which holds support tickets, and which holds invoices? For each, name the actual objects/tables and the key fields — account ID, renewal date, usage measure, ticket severity, invoice status. Then tell me how the sources link together (what joins an account to its usage and tickets?). |
| 1 | Health Snapshot | How is [sentinel account] doing? |
| 2 | QBR Prep | Prepare QBR material for [account] covering [last quarter]: (1) value delivered — usage growth, features adopted, measurable outcomes, stated in their terms not ours; (2) the usage story — trend chart with annotations for notable changes; (3) open issues — unresolved tickets and their status, honestly framed; (4) expansion hooks — unused products/seats they're a fit for, usage patterns suggesting upsell readiness; (5) proposed agenda — three discussion topics based on all of the above. Format as a deck-ready summary: section headers, bullets, one chart per section where useful. |
| 3 | Churn Signals | Scan my book for churn leading indicators: usage down more than [20%] quarter-over-quarter; key contacts gone quiet or departed; ticket volume spiking — or dropping to zero after being active; seats contracting; payment delays. List accounts with two or more signals, worst first. |
| 4 | Renewal & Expansion | All accounts with renewals in the next [two quarters]: renewal date, ARR, health read (from the four pillars), and a risk flag — red if two-plus churn signals, yellow if one, green otherwise. Order by renewal date. Total the ARR at risk in each color. |
| 5 | Automate the Book | Build my backing table: every account I own, with columns Account, CSM, RenewalDate, and Tier, as a CSV I can download and use as the template's backing table. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
