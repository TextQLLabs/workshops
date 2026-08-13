# Ontology Operations — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Ontology Operations" workshop with me on this workspace. It has steps I do in the
console/CLI, so: first inspect the current state (connectors / roles / context / git, as relevant) and
tell me where we're starting. Then go ONE step at a time — for analysis steps, hand me the prompt to
run; for console/setup steps, tell me exactly what to do, then WAIT until I say "done" and re-check
before moving on (don't assume a setup step is done). If I leave and come back, re-inspect and resume.
Start by telling me what you see.
```

## Module 0 · The Operating Question

### 0.1 · Building vs operating

### 0.2 · The health check

**Prompt for the learner to run:**
```
Assess our ontology's operational health: (1) When was each top-level folder last modified? (2) How many files have no clear owner or no explanatory note? (3) Are there pending review patches older than a week? (4) Which governed metrics have no test pinning their expected value? (5) Which folders are open to everyone vs restricted?
```

> ✅ You'll see: Staleness, ownership, review debt, test coverage, and access in one pass. Every weak spot maps to a module below.

### 0.3 · Know your editor surfaces
Four tabs you'll live in: Files (the tree), Graph (entities and lineage), Reviews (pending patches), History (every change, attributable and revertible). Find all four now.

**Checkpoint before moving on:**
- [ ] You can state the six operating questions from memory
- [ ] The health check ran and you know your two weakest areas
- [ ] You've located Files, Graph, Reviews, and History

## Module 1 · Git-Backed Operations

### 1.1 · The three population paths, operationally

### 1.2 · Connect (or audit) the Git sync

> **Where to click** — Ontology editor → Connect Git (top right) — GitHub, GitLab, CodeCommit, Bitbucket, any standard remote; changes flow both directions

### 1.3 · Sync failures are incidents
A failed sync means Ana reads stale context — today's questions, last week's definitions. Accuracy incident, not inconvenience.

> **Where to click** — Settings → Notifications → Ontology → Sync failures → route to the channel your data team actually watches

### 1.4 · Review discipline

> **Routing hygiene — part of every merge** — Every change that adds or moves a file should also touch the routing docs : the folder README (what lives here, which files are canonical) and — deliberately — the search phrases a future thread would use, repeated in prose. Discoverability is part of the deliverable; a definition nobody can find might as well not exist.

**Checkpoint before moving on:**
- [ ] You know which path each contributor type uses, and stopped wrong-path traffic
- [ ] Git sync connected (or deliberately not), with one review gate, not two
- [ ] Sync-failure notifications route somewhere watched; you know the incident drill
- [ ] You rendered the SQL of a .tql patch before approving it

## Module 2 · Access Control at Scale

### 2.1 · The model

> **Where to click** — Ontology → folder row → Manage access icon → Open/Restricted → add roles with levels

### 2.2 · Design the folder tree for access

### 2.3 · Verify as the user

**Prompt for the learner to run:**
```
What do you know about [a metric in a folder this role shouldn't see]?
```

> ✅ You'll see: Ana genuinely unaware — not declining, unaware. Then confirm a should-see metric loads. Both directions matter.

### 2.4 · Compose with the other layers
Folder access controls what context Ana loads . It composes with: org RBAC (who can use ontology at all), connector scoping (what data the role queries), and fail-closed RLS inside .tql files (which rows return). Restricted folder + fail-closed .tql = defense in depth.

### 2.5 · Make the RLS boundary enforceable
.tql RLS rules only bind queries that go *through* them. Admins can now close the plain-SQL bypass (per-connection **TQL-only**, per-role **raw-sql permission**, org-wide switch — Admin & Governance workshop, Module 3.4b). Two consequences for the ontology team: RLS-bearing .tql files are now security boundaries and deserve a designated reviewer; and **the ontology must be built before the lock goes on** — TQL-only removes the ungoverned path without creating a governed one, so a connection with no query files that gets locked is unqueryable. Build first, verify coverage, lock second.

**Prompt for the learner to run:**
```
List the .tql query files for the [name] connection that contain row-level filters. For each: which user attributes drive the filter, when it last changed, and who reviewed the change. Then check the last 30 days of questions against this connection — were any answered via raw SQL rather than these files?
```

> ✅ You'll see: the RLS inventory with review history, plus the raw-SQL traffic that would be blocked (or go ungoverned) after lockdown — the pre-flight for turning on TQL-only.

**Checkpoint before moving on:**
- [ ] Your folder tree's shape matches your access policy
- [ ] Restricted folders verified as a restricted user — both directions
- [ ] You can name the four composing layers
- [ ] RLS-bearing .tql files have a designated reviewer, and coverage was checked before any connection went TQL-only

## Module 3 · Accuracy Testing

> **Already have a golden suite?** — Building golden datasets and eval cases from scratch is covered in full depth in Data Quality & Validation Deep-Dive (Modules 1–3). If your suite exists, skim 3.1–3.3 and focus on 3.4–3.5 — wiring it into the operating loop is this module's real job.

### 3.1 · Governed ≠ correct
Governance ensures one definition is used everywhere. It doesn't ensure that definition still computes the right number after a schema change, a backfill, or an upstream redefinition. That's testing.

### 3.2 · Build the golden set

**Prompt for the learner to run:**
```
From recent threads, identify the 10 most-asked governed metrics. For each, compute the value for [a fixed, closed period — e.g., last full quarter] through the ontology surface, and record: the question, the exact parameters, the expected value, and the .tql file it routes through. Format as a golden-dataset table I can save.
```

> ✅ You'll see: Your test suite, derived from actual usage. Closed periods only — "revenue last quarter" is checkable forever. Store it in the ontology itself (a golden/ folder), versioned with the definitions it tests.

### 3.3 · Run the suite

**Prompt for the learner to run:**
```
Run our golden dataset: for each row, execute the question through the ontology with the recorded parameters and compare to the expected value. Report pass/fail per row, with the divergence and the responsible .tql file for any failure.
```

> ✅ You'll see: Pass/fail across your most-trusted numbers. A failure is one of: definition changed (deliberate?), data changed (backfill? bug?), schema drifted — each with a different owner.

### 3.4 · Schedule it — the drift alarm

**Prompt for the learner to run:**
```
Create a playbook "Ontology Golden Suite" that runs [weekly, Sunday 11pm]: execute the golden dataset, compare to pinned values, and report. If everything passes, one line. If anything fails, lead with the failing metric, the divergence, the .tql file, and the three most likely causes. Send to [#data-team].
```

> ✅ You'll see: Drift detection that runs before Monday's questions — the highest-leverage artifact in this workshop. "It was right as of last night."

### 3.5 · Test the changes, too

**Checkpoint before moving on:**
- [ ] A golden set exists — real usage, closed periods, stored in the ontology
- [ ] You ran the suite manually and triaged failures to their cause
- [ ] The weekly drift playbook is deployed
- [ ] Patch review includes golden-row checks; pins update in the same patch as deliberate changes

## Module 4 · The Improvement Loop

### 4.1 · Usage drives the roadmap

### 4.2 · Mine the corrections

**Prompt for the learner to run:**
```
Review threads from the last 30 days for moments where a user corrected a definition, restated business logic, or re-explained a term mid-conversation. List the correction, how often it recurs across threads and users, and whether the ontology currently covers it. Rank by recurrence.
```

> ✅ You'll see: The unproposed patches — knowledge that evaporates after each thread. Recurring ones become this week's patches.

### 4.3 · Small patches, always

**Prompt for the learner to run:**
```
For the top recurring correction: locate where it belongs in the existing ontology, draft the minimal targeted edit (don't touch unrelated files), and open it for review with a description of which threads motivated it.
```

> ✅ You'll see: One focused patch. Ten one-file patches a month beat one forty-file patch a quarter — reviewable, revertible, and the golden suite isolates exactly which change broke what.

### 4.4 · Close the loop with proposers
Review fast (the SLA), and decline with reasons — "we define it this way because X, see notes/Y.md" teaches; silence discourages the next proposal. Notifications handle the verdict; the reason is on you.

### 4.5 · Measure the loop
Monthly: warn rate trend (is missing-context shrinking?), patch throughput (proposed vs reviewed, median queue age), correction recurrence (is the 4.2 list shrinking?). Patches flowing but warnings flat = patching the wrong gaps.

**Checkpoint before moving on:**
- [ ] You merged the Warning Breakdown and corrections list into one ranked backlog
- [ ] You shipped at least one minimal targeted patch from it
- [ ] Proposers get fast reviews and reasoned denials
- [ ] The three loop metrics are in your monthly routine

## Module 5 · Scaling Across Teams

### 5.1 · The shared/team split

### 5.2 · Promotion: team → shared

**Prompt for the learner to run:**
```
Scan the team folders for definitions that duplicate or near-duplicate each other, or that threads from other teams keep referencing. Recommend which should be promoted to shared/, and flag any whose duplicates have drifted apart — those need reconciliation, not just promotion.
```

> ✅ You'll see: The promotion queue — including the dangerous case: near-duplicates that diverged. Reconcile (one definition wins; the alternative survives under a clearly distinct name), then promote.

### 5.3 · Role scoping completes the picture
Folder access (Module 2) + role-based response behavior (build workshop) + connector scope: each team sees shared/ + its folder, in its house style, through its data. One model, many faces.

### 5.4 · Onboarding a new team

**Checkpoint before moving on:**
- [ ] shared/ vs team folders reflects the boundary rule
- [ ] You ran the promotion scan and reconciled diverged duplicates
- [ ] Each team folder has designated editors and the right access
- [ ] You could onboard the next team from the five-step sequence

## Module 6 · The Runbook

### 6.1 · Fill it in
Create runbook.md at the ontology root (it's context too — Ana can answer "who owns the finance folder?" from it). Sections:

**Prompt for the learner to run:**
```
Create runbook.md at the ontology root using this outline. Pre-fill everything you can observe — folder owners from file history, the access map from folder settings, the golden suite playbook and its schedule — and mark the rest [TODO]. Open it as a patch for review.
```

> ✅ You'll see: A draft runbook with the observable facts filled in — the remaining TODOs are the decisions only you can make.

### 6.2 · Test the bus factor
Do: have your backup owner run one full cycle using only the runbook — review queue, manual golden run, one warning triaged. Where they stall, the runbook is incomplete. Fix now, not during the vacation.

### 6.3 · The operating rhythm, assembled

### You're done
Disciplined change flow, verified access, a tested core with a drift alarm, a usage-fed improvement loop, a no-fork scaling pattern, and a runbook that survives you. The full circle: the golden suite feeds the Admin workshop's observability loop; the governed metrics feed the Dashboards workshop's Ontology SQL sources; the definitions feed every workshop's answers.

**Checkpoint before moving on:**
- [ ] runbook.md exists at the ontology root, every section filled
- [ ] Your backup ran a full cycle from it without asking you anything
- [ ] The cadence table is real — each row has an owner and a calendar slot

