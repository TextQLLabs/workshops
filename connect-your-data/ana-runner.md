# Connect Your Data — Ana-Led Runner (v2: gated)

> A **gated** runner (v2). This workshop has steps you do in the **console/CLI**, so Ana inspects your
> workspace state, **gates on each prerequisite**, hands you the setup action and **waits**, then re-checks
> — and resumes if you step away. Pairs with `../ana-workshop-facilitator.md`. Delivery: inline / URL.
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this)

```
Hey Ana — facilitate the "Connect Your Data" workshop with me on this workspace. It has steps I do in the
console/CLI, so: first inspect the current state (connectors / roles / context / git, as relevant) and
tell me where we're starting. Then go ONE step at a time — for analysis steps, hand me the prompt to
run; for console/setup steps, tell me exactly what to do, then WAIT until I say "done" and re-check
before moving on (don't assume a setup step is done). If I leave and come back, re-inspect and resume.
Start by telling me what you see.
```

## Methodology (what this teaches)

A hands-on workshop for data engineers, platform admins, and technical leads: get every kind of data source connected to TextQL correctly the first time — warehouses, private-network databases, BI tools, files, Google Drive, and external APIs — with credentials done right and a validation habit…

## Instructions to Ana (the v2 HOW)

1. **Inspect current state first** — connectors / roles / context / git, as relevant; tell me where we start.
2. **Gate on each prerequisite.** For a console/setup step, hand me the exact action and **WAIT**; re-check on return; **never fake completion**.
3. **One step at a time.** Analysis steps → hand me the prompt + what to look for. Setup steps → the console action, then wait.
4. **Persist progress** so a new thread resumes where we left off.
5. **Adapt to my actual workspace; never fabricate.**

## Modules — Ana gates on setup steps, hands analysis prompts to the learner

| # | Module | Step — Ana hands you the prompt, or the console action |
|---|---|---|
| 0 | Before You Connect | List the connectors that already exist in this org with their types and visibility. I'm about to add [source] — flag anything that looks like a duplicate or overlapping source. |
| 1 | Network & Credentials | *(Ana orients you)* — explain **Network & Credentials** in 3 bullets, grounded in my connected data, then move on. |
| 2 | Warehouse Connectors | Generate a 2048-bit RSA key pair for Snowflake key-pair auth in your sandbox: an unencrypted PKCS#8 private key and its public key. Give me the private key to store in my secrets manager, and the public key with the header/footer lines stripped, ready for ALTER USER. |
| 3 | Private Networks | *(Ana orients you)* — explain **Private Networks** in 3 bullets, grounded in my connected data, then move on. |
| 4 | BI Tools | What dashboards and views do you have access to from this Tableau connector? Summarize what each one shows. |
| 5 | Files, Drive & APIs | Profile this file: columns, types, null rates, and anything that looks like a data quality problem. Then suggest three questions worth asking it. |
| 6 | Validate & Operate | List the schemas and tables you can see through this connector. Anything you'd expect for [domain] that's missing? |

## When done

Recap in 3 bullets. Offer to save anything built as a **Playbook**. Do **not** write the workshop into the governed ontology.
