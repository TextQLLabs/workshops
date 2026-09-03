# Ontology as Code — the Refinery CLI — Ana-Led Runner (FULL)

> Complete facilitation script. Special shape: the learner runs CLI commands in their TERMINAL
> and pastes JSON output back; Ana explains, catches mistakes, and coaches. One module at a time.

## Step-0 prompt

```
Hey Ana — facilitate the "Ontology as Code — the Refinery CLI" workshop with me in this thread,
using the module script below. I'll run each command in my terminal and paste the output back;
you explain what it means and coach. ONE module at a time, checkpoint before advancing. If a
command exits 3 or 4, help me interpret it (auth vs org-disabled) before retrying. Start with Module 0.
```


## Module 0 · Install & Authenticate


Goal: a working refinery on your PATH, authenticated to the right workspace, and the habit of reading refinery info before assuming anything.

### 0.1 · Install

On TextQL cloud:

**Command for the learner to run:**
```
curl -fsSL https://cli.textql.com/cli/install.sh | sh
```

On single-tenant / VPC deployments, downloads are authenticated: fetch the archive from Settings → Desktop & CLI in the web app and put refinery on your PATH. Full instructions (including a section written for coding agents): docs.textql.com/core/admin/cli.

> 📌 The CLI always matches its deploymentrefinery self-updates from the deployment it points at — it can never be newer than your server, and it changes version only when your deployment upgrades. refinery update --check reports without changing anything.

### 0.2 · Authenticate

**Command for the learner to run:**
```
refinery auth login
```

> ✅ You'll see: a device-flow URL + code (approve in the browser), or use refinery auth login --api-key KEY with a workspace API key for headless setups. Then check who you are: refinery auth status.

> 📌 ⚠️ Multi-workspace users: isolate your credentialsThe approval page defaults to the workspace you last used in the web app — it's easy to authorize the wrong one, and the CLI won't warn you. For any work targeting a specific workspace, give it its own config dir and verify: REFINERY_CONFIG_DIR=~/.config/refinery/acme refinery auth login --api-key KEY REFINERY_CONFIG_DIR=~/.config/refinery/acme refinery info

### 0.3 · Orient with refinery info — always first

**Command for the learner to run:**
```
refinery info
```

> ✅ You'll see: the deployment you're pointed at, your identity and roles, the grant's scopes, which execution tools the org allows (python/bash/sql), and your existing sandboxes. Check it before assuming a capability exists — a tool that exits 4 is disabled for the org, not broken.

Exit codes are the contract: 0 success · 1 remote error · 2 usage error · 3 auth (re-login or refinery auth upgrade when the hint says insufficient_scope) · 4 tool unavailable. Every command prints exactly one JSON document on stdout — pipe it, parse it, script it. Use --pretty only for human eyes.

**Checkpoint before moving on:**
- [ ] refinery info shows the workspace you intended, and you can name your enabled tools
- [ ] You know what exit codes 3 and 4 mean and which one auth upgrade can fix



## Module 1 · Query into Sandboxes


Goal: query live connectors into a remote Python kernel and analyze there — the data never lands on your machine.

### 1.1 · See your connectors, then look before you query

**Command for the learner to run:**
```
refinery connector db list
refinery connector db tables [connector-id] --like '%[keyword]%'
refinery connector db preview [connector-id] [schema.table] --limit 20
```

> ✅ You'll see: the org's connectors, then real table names and sample rows. Raw SQL executes verbatim in the connector's own dialect — check refinery connector db get [id] and write that dialect; there is no translation layer.

### 1.2 · Query into a sandbox you named

**Command for the learner to run:**
```
refinery connector db query [connector-id] --sql 'SELECT ... FROM [table] LIMIT 1000' --as orders --sandbox [my-task-name]
refinery exec python 'print(orders.describe())' --sandbox [my-task-name]
```

> ✅ You'll see: the result lands as a pandas dataframe inside the named sandbox; the next exec python sees it directly. Kernel state persists across calls; files under /sandbox/files survive restarts, variables don't.

> 📌 ⚠️ Name your sandbox after the task — never share "default"Two sessions that both use the default name share one kernel: variables silently overwrite each other, and one sandbox rm destroys the other's state. --sandbox churn-analysis costs nothing and saves an afternoon.

### 1.3 · The two disciplines

- Aggregate in place. Never pull large result sets to your terminal — compute in the sandbox and print only the summary. Memory is bounded; avoid df.copy().
- Python never fetches data. Retrieval order is fixed: search the ontology for an existing definition → run a governed .tql → only then raw SQL. Python is for post-processing and charting.

**Checkpoint before moving on:**
- [ ] A dataframe loaded via --as is visible from a follow-up exec python in your named sandbox
- [ ] You checked the connector's dialect before writing SQL for it
- [ ] You can recite the retrieval order (ontology → .tql → raw SQL) and where Python fits



## Module 2 · Run Governed Queries


Goal: read the org's context the way the product's agent does, and execute approved query surfaces with typed parameters.

### 2.1 · The mount

Every sandbox is preloaded with the org's Ontology at /sandbox/files/library, scoped to what your roles can see. Use the pre-imported helper instead of walking directories:

**Command for the learner to run:**
```
refinery exec python 'import library; print(library.list_files(subdir=""))' --sandbox [my-task-name]
```

> ✅ You'll see: .tql models, docs, datasets. Two files are instructions, not data: ANA.md (the org's standing rules — root + per-subdirectory, all binding) and ana-config.yaml (what auto-attaches, per connector/role). Read them before analyzing or your answers will differ from the product's for the same question. Files certified golden win when sources disagree.

### 2.2 · Execute a governed surface

**Command for the learner to run:**
```
refinery exec python 'import library; print(library.read_file("[path/to/query.tql]"))' --sandbox [my-task-name]
refinery connector db query [connector-id] --tql [path/to/query.tql] --param '[name]=[value]' --as result --sandbox [my-task-name]
```

> ✅ You'll see: read the file's params block first — they're typed, may be nullable, may have defaults; omit a key to take its default and never invent a name. A List takes objects {"key","operator","value"} with word operators (equals, in, between…). --tql always runs the approved ontology copy — editing the mount changes nothing.

**Checkpoint before moving on:**
- [ ] You read the root ANA.md and one subdirectory's before touching its data
- [ ] A governed .tql executed with a parameter you chose from its params block
- [ ] You can say why editing a .tql in the mount doesn't change what --tql runs



## Module 3 · Author .tql Correctly


Goal: write a typed, parameterized query surface using the deployment's own authoring skill as the grammar reference — not guesswork.

### 3.1 · Pull the authoritative grammar

The deployment ships the same instruction sets the product's agent uses. Discover and fetch them — don't guess trigger names:

**Command for the learner to run:**
```
refinery rpc call textql.rpc.public.patches.OntologyManagementService/ListSkills --body '{"includeUnlisted":true}'
refinery rpc call textql.rpc.public.patches.OntologyManagementService/GetSkill --body '{"trigger":"writing-tql"}'
```

> ✅ You'll see: the complete .tql language reference — file layout, the params grammar, fragment functions, and a "rejected forms" table of the mistakes everyone makes. Keep it open while authoring.

### 3.2 · The guardrails that save you an hour

| You'll try | The language wants |
|---|---|
| metrics: Set = ["a"] | Set defaults may only be [] — handle the empty case in the body with isEmpty |
| concatSep(", ", frags) | Application syntax, no parens/commas: concatSep ", " frags |
| matchSet as a function | It's syntax: matchSet metrics { "label" -> sql"...", } (arrow arms; trailing comma allowed here only) |
| '${param}' in SQL | Unquoted ${param} — the renderer handles quoting; IN ${list} adds parens |
| '' inside a sql''…'' body | No adjacent single quotes there — put quoted SQL literals in sql"…" fragments |

### 3.3 · The authoring loop

1. Write the file (params block → let bindings → one sql''…'' body ending in an explicit LIMIT).
2. Execute it against a live connector with real parameters. Not "it parses" — it returns correct rows.
3. Reconcile one number against a raw-SQL computation of the same thing before you call it done.

> 📌 ⚠️ Execute before you save — a true storyA flows query surface joined a snapshot table (several rows per entity) to a fact table. It parsed, rendered, and ran — and inflated every total ~6.5×. The only reason no user ever saw the wrong number: the surface was executed against the live connector and reconciled before installation, the join deduplicated, re-verified to the dollar. An untested query surface is a liability wearing a governance costume.

**Checkpoint before moving on:**
- [ ] You fetched writing-tql from your own deployment and used its rejected-forms table
- [ ] Your .tql executed against a live connector with parameters
- [ ] One output number reconciled against an independent raw-SQL computation



## Module 4 · Ontology as Code


Goal: ship your authored files into the org's Ontology from the terminal (or CI), without giving up the review discipline that makes governance credible.

### 4.1 · Search, then call

refinery rpc reaches every backend method. Always search before you call — never guess a method or field name:

**Command for the learner to run:**
```
refinery rpc search ontology upsert
refinery rpc call OntologyManagementService/UpsertOntologyFile --body '{"path":"databases/[source]/queries/[name].tql","content":"...","commitMessage":"add [name] query surface"}'
```

> ✅ You'll see: the file lands in the org's Ontology immediately. Loop it over a local directory and a whole semantic layer installs in seconds — every file with its own commit message.

> 📌 ⚠️ Writes are real and immediateUpsertOntologyFile commits straight to the org's Ontology — no draft, no review step. That's right for a workspace you own (POCs, sandboxes, pre-user builds) and wrong for a production org with users: there, author in a sandbox and file a reviewed patch, or route through git-sync PRs. The rule of thumb: direct upsert until the first real user joins; reviewed changes after.

### 4.2 · What to install

Follow the Save-to-Ontology conventions: a root ANA.md routing document, one folder per source with a README (tables, grains, joins, dead-ends), a definitions.md (one definition per term; unknowns marked OPEN with "don't guess" instructions), tested query surfaces, and dated append-only findings. Profile the data first — every coded value a definition references should have been observed, never assumed.

### 4.3 · Prove it round-trips

**Command for the learner to run:**
```
refinery connector db query [connector-id] --tql databases/[source]/queries/[name].tql --param '[name]=[value]' --sandbox [my-task-name]
```

> ✅ You'll see: --tql runs the APPROVED copy — so this is the proof your installed file is the file you tested. If render errors name grammar problems, Module 3's table is the fix-it list.

**Checkpoint before moving on:**
- [ ] Your files are in the org's Ontology with meaningful commit messages
- [ ] The installed .tql executes governed, with output matching your pre-install test
- [ ] You can state when direct upsert is acceptable and when reviewed patches are required



## Module 5 · Prove the Lift


Goal: a scripted evaluation showing exactly what your ontology changed — the artifact that turns "trust me" into a table.

### 5.1 · The design

1. Ground truth first. For each eval question, compute the expected answer with direct SQL on the warehouse — before any agent runs. Where the data can't answer (coded fields with no dictionary), the expected answer is an honest "unanswerable as posed."
2. Baseline with the ontology OFF. Run every question through the platform with schema inference only, and capture answers + SQL. Sequence matters: capture the control before ontology content enters the workspace.
3. Install (Module 4), re-run identical questions, diff.

### 5.2 · Script it

Drive the runs headlessly — each question one chat via the platform API with the ontology tool toggled, or via refinery agent run for agent-shaped evals; write results to a JSONL you can resume. Score against ground truth; grade judgment questions by hand.

### 5.3 · What lift actually looks like

- Routing fixes — questions that previously searched the wrong schema and declared data missing now find it (a routing README is often the single highest-lift file).
- Definitional pins — "total X" stops having two defensible answers; responses cite the governed definition they used.
- Honest refusals — where no governed definition exists, the agent says so and asks for the dictionary instead of fabricating. For evaluators whose criteria are accuracy and trust, that's a feature.
- And an honest ledger — expect the baseline to beat your first-draft ground truth on a question or two. Revise the ground truth and say so; the eval is scored against data, not pride.

**Checkpoint before moving on:**
- [ ] Ground truth existed before the first agent run, honest "unanswerable" entries included
- [ ] Baseline captured with the ontology off, before install
- [ ] The lift table separates routing fixes, definitional pins, and honest refusals



## Wrap-Up


The loop you now own: orient → query → read the mount → author against the deployment's grammar → execute-before-save → install → prove the lift. Everything in it is scriptable, which means everything in it can run in CI: lint .tql on every PR by rendering it, execute golden surfaces against a staging connector, block merge on a reconciliation failure.

### The capstone

**Command for the learner to run:**
```
refinery exec python 'import library; files=library.list_files(subdir=""); print(len(files), "ontology files visible");' --sandbox capstone
```

Then pick one recurring business question your team answers by hand, and take it through the full loop this week: governed surface, installed, reconciled, and a before/after run saved next to your golden datasets.

### Where to go from here

- Build Your Ontology, End to End — what to put IN the ontology (this workshop is how to ship it).
- Ontology Operations — running it in production: reviews, golden suites, drift alarms.
- Developer Workshop — API & MCP — the REST and MCP surfaces that pair with the CLI.
- Data Quality & Validation — turning Module 5's eval into a standing golden suite.

