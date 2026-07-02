# Connect Your Data — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Connect Your Data" workshop with me on this workspace. It has steps I do in the
console/CLI, so: first inspect the current state (connectors / roles / context / git, as relevant) and
tell me where we're starting. Then go ONE step at a time — for analysis steps, hand me the prompt to
run; for console/setup steps, tell me exactly what to do, then WAIT until I say "done" and re-check
before moving on (don't assume a setup step is done). If I leave and come back, re-inspect and resume.
Start by telling me what you see.
```

## Module 0 · Before You Connect

> **Note** — Starting a POC? Upload a file today, connect the warehouse when ready for production. Don't block a pilot on firewall tickets.

> **Where to click** — Connectors (left sidebar) — add connectors, preview synced tables, resync after schema drift, manage API accesses (API tab)

**Prompt for the learner to run:**
```
List the connectors that already exist in this org with their types and visibility. I'm about to add [source] — flag anything that looks like a duplicate or overlapping source.
```

> ✅ You'll see: The current fleet — surprisingly often the source is already half-connected, and your job is consolidation, not creation.

**Checkpoint before moving on:**
- [ ] You know which of the three intake paths fits your immediate need
- [ ] You can see the New Connector button (you have connector:write)
- [ ] You've decided: service identity, scope (DB/schema, RO/RW), visibility
- [ ] You checked for duplicates before creating anything

## Module 1 · Network & Credentials

> **Note** — VPC, on-prem, single-tenant: this does not apply to you — confirm network requirements with your TextQL representative. BigQuery also needs no IP whitelisting (OAuth/service account over Google's API).

**Checkpoint before moving on:**
- [ ] Both TextQL IPs whitelisted (or VPC rules confirmed instead)
- [ ] You can tell a network failure from a credential failure by symptom
- [ ] You know which credential pattern your source uses
- [ ] Six connection details gathered, secret vaulted

## Module 2 · Warehouse Connectors

**Prompt for the learner to run:**
```
Generate a 2048-bit RSA key pair for Snowflake key-pair auth in your sandbox: an unencrypted PKCS#8 private key and its public key. Give me the private key to store in my secrets manager, and the public key with the header/footer lines stripped, ready for ALTER USER.
```

> ✅ You'll see: Both keys. Vault the private key immediately — it goes into the connector form, never into a shared doc.

> **Prefer the terminal?** — Generate the pair locally instead: # Private key openssl genrsa 2048 | openssl pkcs8 -topk8 -inform PEM -out rsa_key.p8 -nocrypt # Public key openssl rsa -in rsa_key.p8 -pubout -out rsa_key.pub

> **Note** — The Role field is your governance lever: create a TEXTQL_ROLE with explicit SELECT grants rather than reusing a broad human role. Upgrade path: per-member OAuth (Admin workshop, Module 3).

> **Note** — Test behavior: with a Dataset ID the test verifies that dataset; without one it checks the account can list datasets in the project — a key that reads one dataset but can't list will "fail" unscoped while being perfectly usable scoped. Scope it.

> **Note** — The classic gotcha: a stopped SQL warehouse . The test may pass after a slow warm-up, then morning queries crawl. Set auto-stop sensibly; warn users about first-query warm-up.

**Prompt for the learner to run:**
```
Pull the information schema for this connector. List the tables you can see with row counts where cheap to get, and flag any schemas you expected but can't access.
```

> ✅ You'll see: What Ana actually sees — the credential's view, not yours. "Tables don't match the warehouse" is grants, not bugs: compare the service identity's grants to your own.

**Checkpoint before moving on:**
- [ ] Warehouse connector created with a dedicated service identity and least-privilege grants
- [ ] Connection test passed — and you know what the test actually verifies
- [ ] Ana's schema view matches what you intended to expose
- [ ] Credential vaulted with a rotation date; PAT expirations tracked

## Module 3 · Private Networks

> **Note** — In the main connection fields: Host URL and Port are the database's private address — the one reachable from the bastion . Putting a public-looking endpoint there is the most common mistake.

**Checkpoint before moving on:**
- [ ] Bastion has a dedicated TextQL OS user and inbound SSH from both TextQL IPs
- [ ] Connector uses the database's private address in the main fields
- [ ] Host public key provided (or consciously skipped)
- [ ] You can map each error message to its broken hop

## Module 4 · BI Tools

> **Note** — Both BI integrations also have an org-level Capabilities toggle ( Admin & Governance workshop , Module 4). Connector type missing? Look there first.

> **Note** — The step people miss: creating the connector connects the server ; Ana sees nothing until you build a collection.

> **Where to click** — Connectors → your Tableau connector → New collection → project → workbook → select views (and linkable published datasources)

**Prompt for the learner to run:**
```
What dashboards and views do you have access to from this Tableau connector? Summarize what each one shows.
```

> ✅ You'll see: Exactly the views in your collection — and only those.

**Prompt for the learner to run:**
```
What Power BI workspaces and datasets can you see through this connector? Summarize what each one covers.
```

> ✅ You'll see: Exactly the workspaces the service principal was added to — and only those.

**Checkpoint before moving on:**
- [ ] Tableau: connector created from a PAT and a collection built exposing chosen views
- [ ] Power BI: app registration recorded, secret expiry calendared, workspace access granted, workspaces synced
- [ ] You verified in a chat that Ana lists the dashboards you exposed — and only those
- [ ] You can name the three Power BI failure modes

## Module 5 · Files, Drive & APIs

**Prompt for the learner to run:**
```
Profile this file: columns, types, null rates, and anything that looks like a data quality problem. Then suggest three questions worth asking it.
```

**Prompt for the learner to run:**
```
Open the Google Sheet "[name]" and read me the first 10 rows of the first tab.
```

> ✅ You'll see: The sheet contents — or a failure that is, 90% of the time, Part 2: the sheet isn't shared with the service account email.

> **Where to click** — Connectors → API tab → + New API Access

**Prompt for the learner to run:**
```
Using the [name] API access, make a simple authenticated request (e.g., fetch my account info or list one page of records) and show me the response status.
```

**Checkpoint before moving on:**
- [ ] You uploaded and profiled a file — and know when files stop being the right tool
- [ ] Drive: service account created, APIs enabled, content shared with the bot email, credential stored
- [ ] You created or reviewed one API access and tested it from a chat
- [ ] You know the domain-whitelisting perimeter and who approves additions

## Module 6 · Validate & Operate

**Prompt for the learner to run:**
```
List the schemas and tables you can see through this connector. Anything you'd expect for [domain] that's missing?
```

**Prompt for the learner to run:**
```
Run a trivial query: count rows in [small table] and show 5 sample rows.
```

**Prompt for the learner to run:**
```
Query [table in a different schema you intend to expose]. Does access work across all intended schemas?
```

**Prompt for the learner to run:**
```
Attempt to create a temp table. I expect this to FAIL — confirm the connector is read-only.
```

**Prompt for the learner to run:**
```
Run a realistic analytical query for this source: [a join + aggregation typical of real use]. How long did it take?
```

> ✅ You'll see: Pass/fail on each. #1 catches grant gaps, #4 catches accidentally read-write connectors, #5 catches the stopped-warehouse / timeout class before a user does.

> **Note** — Always rerun the smoke test immediately after rotation — a typo'd key otherwise fails at a user's next query.

**Prompt for the learner to run:**
```
For each connector in this org, when was it last successfully used in a thread? Flag any unused for 60+ days as decommission candidates.
```

**Prompt for the learner to run:**
```
Which dashboards, playbooks, and recent threads depend on the [name] connector? I'm planning to decommission it.
```

**Checkpoint before moving on:**
- [ ] Every connector created in this workshop passed the five-question smoke test
- [ ] Resync is wired into your schema-change/release process
- [ ] Every expiring credential has a calendared expiry
- [ ] You know the decommission sequence, including source-side revocation

