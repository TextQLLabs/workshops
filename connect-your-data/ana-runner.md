# Connect Your Data — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Connect Your Data workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> ⚙️ Setup/operational workshop — some steps are console/config, not data queries; Ana guides rather than runs them.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Connect Your Data" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/connect-your-data/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop for data engineers, platform admins, and technical leads: get every kind of data source connected to TextQL correctly the first time — warehouses, private-network databases, BI tools, files, Google Drive, and external APIs — with credentials done right and a validation habit…

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | Before You Connect | List the connectors that already exist in this org with their types and visibility. I'm about to add [source] — flag anything that looks like a duplicate or overlapping source. |
| 1 | Network & Credentials | *(Ana orients you)* — explain **Network & Credentials** in 3 bullets, grounded in my connected data, then move on. |
| 2 | Warehouse Connectors | Generate a 2048-bit RSA key pair for Snowflake key-pair auth in your sandbox: an unencrypted PKCS#8 private key and its public key. Give me the private key to store in my secrets manager, and the public key with the header/footer lines stripped, ready for ALTER USER. |
| 3 | Private Networks | *(Ana orients you)* — explain **Private Networks** in 3 bullets, grounded in my connected data, then move on. |
| 4 | BI Tools | What dashboards and views do you have access to from this Tableau connector? Summarize what each one shows. |
| 5 | Files, Drive & APIs | Profile this file: columns, types, null rates, and anything that looks like a data quality problem. Then suggest three questions worth asking it. |
| 6 | Validate & Operate | List the schemas and tables you can see through this connector. Anything you'd expect for [domain] that's missing? |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
