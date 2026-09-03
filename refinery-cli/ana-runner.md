# Ontology as Code — the Refinery CLI — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT).
> Special shape: the learner runs CLI commands in their **terminal** and pastes output back; Ana explains and coaches. Ana never needs to run the CLI herself.
> **Full version:** `ana-runner-full.md` — use it by default; this concise file is for token-limited environments.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Ontology as Code — the Refinery CLI" workshop with me in this thread.
Pull the steps from https://textqllabs.github.io/workshops/refinery-cli/ (or the module list below).
I'll run each CLI command in my own terminal and paste the JSON output back to you; you explain
what it means, catch mistakes, and coach — ONE module at a time, checkpoint before advancing.
Start with Module 0.
```

## Modules — the learner runs each command in their terminal, pastes output, Ana coaches

| # | Module | Anchor command |
|---|---|---|
| 0 | Install & Authenticate | `refinery info` — verify deployment, identity, scopes, enabled tools before assuming anything |
| 1 | Query into Sandboxes | `refinery connector db query [id] --sql '...' --as df --sandbox [task-name]` then `refinery exec python 'print(df.describe())' --sandbox [task-name]` |
| 2 | Run Governed Queries | `refinery connector db query [id] --tql [path].tql --param '[k]=[v]'` — after reading the file's params block and the root ANA.md |
| 3 | Author .tql Correctly | `refinery rpc call ...OntologyManagementService/GetSkill --body '{"trigger":"writing-tql"}'` — author against the deployment's own grammar, execute against a live connector, reconcile one number |
| 4 | Ontology as Code | `refinery rpc call OntologyManagementService/UpsertOntologyFile --body '{...}'` — direct upsert only pre-users; reviewed patches after |
| 5 | Prove the Lift | ground truth via SQL → baseline with ontology OFF → install → identical re-run → lift table |

## When done

Recap in 3 bullets. Offer to save the learner's eval harness and .tql files as reusable artifacts. Remind: writes via UpsertOntologyFile are immediate — production orgs use reviewed patches.
