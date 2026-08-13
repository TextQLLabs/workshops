# Connect Your Data — Ana-Led Runner (FULL)

> Full-instruction runner — every module's prompts, expected results, and checkpoints.
> Air-gapped/VPC: upload this file. Token-limited tenants: use the concise `ana-runner.md`.

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

### 0.1 · Know your options

> **Note** — Starting a POC? Upload a file today, connect the warehouse when ready for production. Don't block a pilot on firewall tickets.

### 0.2 · The Connectors page

> **Where to click** — Connectors (left sidebar) — add connectors, preview synced tables, resync after schema drift, manage API accesses (API tab)

### 0.3 · Decision 1 — service account, not personal credentials

### 0.4 · Decision 2 — scope: what should Ana see?
Database/schema scoping, read-only vs read-write, grant-level scoping — and a fourth decision for sensitive sources: **TQL-only**. Marking the connection TQL-only in its settings refuses plain SQL on it in every surface; only governed ontology query files (where row-level security is enforced) can reach the data. Decide at creation like read-only — starting locked means no user ever builds a workflow on the bypass. Flag the full framework (per-role raw-sql permission, org-wide switch) to whoever owns workspace governance: Admin & Governance workshop, Module 3.4b.

### 0.5 · Decision 3 — who in TextQL gets this connector?
Public vs private is one dropdown at creation but a migration after users build on it. Production warehouses with sensitive data usually start private and get role-scoped.

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

### 1.1 · The IP allowlist
For SaaS deployments, TextQL connects from two fixed IPs. Whitelist both:

> **Note** — VPC, on-prem, single-tenant: this does not apply to you — confirm network requirements with your TextQL representative. BigQuery also needs no IP whitelisting (OAuth/service account over Google's API).

### 1.2 · Provider-specific firewall steps
AWS (RDS, Aurora, Redshift): RDS Console → database → Connectivity & Security → VPC security group → Inbound rules → Edit. Two rules per engine port, one per IP ( /32 ):

### 1.3 · Verify the path before you blame the credential
A timeout is almost always network (firewall, wrong host/port, private subnet); an authentication error is the credential. Fix in that order. No public endpoint at all → Module 3 .

### 1.4 · The credential patterns you'll meet

### 1.5 · Gather your details

**Checkpoint before moving on:**
- [ ] Both TextQL IPs whitelisted (or VPC rules confirmed instead)
- [ ] You can tell a network failure from a credential failure by symptom
- [ ] You know which credential pattern your source uses
- [ ] Six connection details gathered, secret vaulted

## Module 2 · Warehouse Connectors

### 2.1 · Snowflake
Auth: key-pair only for new connectors (Snowflake deprecates single-factor passwords for service users by Oct 2026; TextQL connects as a service user).

**Prompt for the learner to run:**
```
Generate a 2048-bit RSA key pair for Snowflake key-pair auth in your sandbox: an unencrypted PKCS#8 private key and its public key. Give me the private key to store in my secrets manager, and the public key with the header/footer lines stripped, ready for ALTER USER.
```

> ✅ You'll see: Both keys. Vault the private key immediately — it goes into the connector form, never into a shared doc.

> **Prefer the terminal?** — Generate the pair locally instead: # Private key openssl genrsa 2048 | openssl pkcs8 -topk8 -inform PEM -out rsa_key.p8 -nocrypt # Public key openssl rsa -in rsa_key.p8 -pubout -out rsa_key.pub

> **Note** — The Role field is your governance lever: create a TEXTQL_ROLE with explicit SELECT grants rather than reusing a broad human role. Upgrade path: per-member OAuth (Admin workshop, Module 3).

### 2.2 · BigQuery

> **New — Workload Identity Federation** — BigQuery now also supports Workload Identity Federation : authenticate through federated credentials instead of a downloaded service-account key. Prefer WIF where your org restricts SA key creation — no long-lived key file to store or rotate.

> **Note** — Test behavior: with a Dataset ID the test verifies that dataset; without one it checks the account can list datasets in the project — a key that reads one dataset but can't list will "fail" unscoped while being perfectly usable scoped. Scope it.

### 2.3 · Redshift
Password path: Host (cluster endpoint), Port 5439, User, Password, Database, Schema, Dialect (Redshift native vs Postgres-compatible).

### 2.4 · Databricks
Prepare: SQL Warehouses → create/reuse → start it → Connection details → copy Server hostname and HTTP path . Generate a PAT (User Settings → Access tokens) — prefer a service principal's token; set a tracked expiration.

> **Note** — The classic gotcha: a stopped SQL warehouse . The test may pass after a slow warm-up, then morning queries crawl. Set auto-stop sensibly; warn users about first-query warm-up.

### 2.5 · PostgreSQL (and the password family)
Host, Port 5432, Username, Password, Database, Schema — readable from postgresql://user:pass@host:5432/db . Same pattern: MySQL, Aurora, Supabase. Create a read-only user first:

### 2.6 · After any connector: the first sync

**Prompt for the learner to run:**
```
Pull the information schema for this connector. List the tables you can see with row counts where cheap to get, and flag any schemas you expected but can't access.
```

> ✅ You'll see: What Ana actually sees — the credential's view, not yours. "Tables don't match the warehouse" is grants, not bugs: compare the service identity's grants to your own.

### Troubleshooting

**Checkpoint before moving on:**
- [ ] Warehouse connector created with a dedicated service identity and least-privilege grants
- [ ] Connection test passed — and you know what the test actually verifies
- [ ] Ana's schema view matches what you intended to expose
- [ ] Credential vaulted with a rotation date; PAT expirations tracked

## Module 3 · Private Networks

### 3.1 · When a tunnel is the right answer

### 3.2 · Prepare the bastion

### 3.3 · Fill the form
In the PostgreSQL connector form, check Connect via SSH tunnel (bastion host) :

> **Note** — In the main connection fields: Host URL and Port are the database's private address — the one reachable from the bastion . Putting a public-looking endpoint there is the most common mistake.

### 3.4 · Diagnose failures in order

**Checkpoint before moving on:**
- [ ] Bastion has a dedicated TextQL OS user and inbound SSH from both TextQL IPs
- [ ] Connector uses the database's private address in the main fields
- [ ] Host public key provided (or consciously skipped)
- [ ] You can map each error message to its broken hop

## Module 4 · BI Tools

> **Note** — Both BI integrations also have an org-level Capabilities toggle ( Admin & Governance workshop , Module 4). Connector type missing? Look there first.

### 4.2 · Tableau
Prepare: generate a Personal Access Token ; note PAT name (ID) and value. Decompose your URL — from https://10ax.online.tableau.com/#/site/textqldev/home : Server = https://10ax.online.tableau.com , Site = textqldev (after site/ ).

> **Note** — The step people miss: creating the connector connects the server ; Ana sees nothing until you build a collection.

> **Where to click** — Connectors → your Tableau connector → New collection → project → workbook → select views (and linkable published datasources)

**Prompt for the learner to run:**
```
What dashboards and views do you have access to from this Tableau connector? Summarize what each one shows.
```

> ✅ You'll see: Exactly the views in your collection — and only those.

### 4.3 · Power BI
The most setup-intensive connector: Azure AD app registration (service principal). Needs Azure portal access, Power BI workspace admin, Pro/Premium license.

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

### 5.1 · File uploads — zero setup
Attach a CSV/Excel in a thread (the "+" button) and ask away. Right for: POCs, one-offs, data not in any warehouse. Know: 1M rows / 100 cols default; can be disabled org-wide (Capabilities); a file lives with its thread — not a governed source. A recurring file is your signal to connect the system it came from.

**Prompt for the learner to run:**
```
Profile this file: columns, types, null rates, and anything that looks like a data quality problem. Then suggest three questions worth asking it.
```

### 5.2 · Google Drive & Sheets — the service account pattern
Part 1 — GCP Console: create/reuse a project → enable Google Drive API and Google Sheets API → create a service account → download a JSON key .

**Prompt for the learner to run:**
```
Open the Google Sheet "[name]" and read me the first 10 rows of the first tab.
```

> ✅ You'll see: The sheet contents — or a failure that is, 90% of the time, Part 2: the sheet isn't shared with the service account email.

### 5.3 · External API connectors

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

### 6.1 · The six-question smoke test
Run in a fresh chat against every new connector before announcing it:

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

**Prompt for the learner to run (TQL-only connections):**
```
Attempt a plain SQL query against this connection. I expect this to FAIL with the TQL-only restriction — then answer the same question through a governed ontology query to prove the approved path works.
```

> ✅ You'll see: Pass/fail on each. #1 catches grant gaps, #4 catches accidentally read-write connectors, #5 catches the stopped-warehouse / timeout class before a user does, #6 proves the lock is on *and* the governed path still answers — both halves matter before announcing a locked connection.

### 6.2 · Schema drift and resync
Warehouses evolve; the schema snapshot doesn't follow automatically. Connectors → connector → Resync after tables are added/renamed/dropped. Habit: wire resync into your dbt/ETL release checklist. "Ana doesn't see the new table" → resync is triage step 1 (and check the Postgres default-privileges grant, Module 2.5).

### 6.3 · Credential rotation

> **Note** — Always rerun the smoke test immediately after rotation — a typo'd key otherwise fails at a user's next query.

### 6.4 · Monitor the fleet

**Prompt for the learner to run:**
```
For each connector in this org, when was it last successfully used in a thread? Flag any unused for 60+ days as decommission candidates.
```

### 6.5 · Decommission without breakage

**Prompt for the learner to run:**
```
Which dashboards, playbooks, and recent threads depend on the [name] connector? I'm planning to decommission it.
```

### You're done
Your data layer is connected with dedicated service identities, least-privilege scopes, a cleared network path, validated behavior, and operating habits for rotation, drift, and decommission.

**Checkpoint before moving on:**
- [ ] Every connector created in this workshop passed the six-question smoke test
- [ ] Resync is wired into your schema-change/release process
- [ ] Every expiring credential has a calendared expiry
- [ ] You know the decommission sequence, including source-side revocation

