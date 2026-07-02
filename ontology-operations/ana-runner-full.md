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

**Prompt for the learner to run:**
```
Assess our ontology's operational health: (1) When was each top-level folder last modified? (2) How many files have no clear owner or no explanatory note? (3) Are there pending review patches older than a week? (4) Which governed metrics have no test pinning their expected value? (5) Which folders are open to everyone vs restricted?
```

> ✅ You'll see: Staleness, ownership, review debt, test coverage, and access in one pass. Every weak spot maps to a module below.

**Checkpoint before moving on:**
- [ ] You can state the six operating questions from memory
- [ ] The health check ran and you know your two weakest areas
- [ ] You've located Files, Graph, Reviews, and History

## Module 1 · Git-Backed Operations

> **Where to click** — Ontology editor → Connect Git (top right) — GitHub, GitLab, CodeCommit, Bitbucket, any standard remote; changes flow both directions

> **Where to click** — Settings → Notifications → Ontology → Sync failures → route to the channel your data team actually watches

**Checkpoint before moving on:**
- [ ] You know which path each contributor type uses, and stopped wrong-path traffic
- [ ] Git sync connected (or deliberately not), with one review gate, not two
- [ ] Sync-failure notifications route somewhere watched; you know the incident drill
- [ ] You rendered the SQL of a .tql patch before approving it

## Module 2 · Access Control at Scale

> **Where to click** — Ontology → folder row → Manage access icon → Open/Restricted → add roles with levels

**Prompt for the learner to run:**
```
What do you know about [a metric in a folder this role shouldn't see]?
```

> ✅ You'll see: Ana genuinely unaware — not declining, unaware. Then confirm a should-see metric loads. Both directions matter.

**Checkpoint before moving on:**
- [ ] Your folder tree's shape matches your access policy
- [ ] Restricted folders verified as a restricted user — both directions
- [ ] You can name the four composing layers

## Module 3 · Accuracy Testing

> **Already have a golden suite?** — Building golden datasets and eval cases from scratch is covered in full depth in Data Quality & Validation Deep-Dive (Modules 1–3). If your suite exists, skim 3.1–3.3 and focus on 3.4–3.5 — wiring it into the operating loop is this module's real job.

**Prompt for the learner to run:**
```
From recent threads, identify the 10 most-asked governed metrics. For each, compute the value for [a fixed, closed period — e.g., last full quarter] through the ontology surface, and record: the question, the exact parameters, the expected value, and the .tql file it routes through. Format as a golden-dataset table I can save.
```

> ✅ You'll see: Your test suite, derived from actual usage. Closed periods only — "revenue last quarter" is checkable forever. Store it in the ontology itself (a golden/ folder), versioned with the definitions it tests.

**Prompt for the learner to run:**
```
Run our golden dataset: for each row, execute the question through the ontology with the recorded parameters and compare to the expected value. Report pass/fail per row, with the divergence and the responsible .tql file for any failure.
```

> ✅ You'll see: Pass/fail across your most-trusted numbers. A failure is one of: definition changed (deliberate?), data changed (backfill? bug?), schema drifted — each with a different owner.

**Prompt for the learner to run:**
```
Create a playbook "Ontology Golden Suite" that runs [weekly, Sunday 11pm]: execute the golden dataset, compare to pinned values, and report. If everything passes, one line. If anything fails, lead with the failing metric, the divergence, the .tql file, and the three most likely causes. Send to [#data-team].
```

> ✅ You'll see: Drift detection that runs before Monday's questions — the highest-leverage artifact in this workshop. "It was right as of last night."

**Checkpoint before moving on:**
- [ ] A golden set exists — real usage, closed periods, stored in the ontology
- [ ] You ran the suite manually and triaged failures to their cause
- [ ] The weekly drift playbook is deployed
- [ ] Patch review includes golden-row checks; pins update in the same patch as deliberate changes

## Module 4 · The Improvement Loop

**Prompt for the learner to run:**
```
Review threads from the last 30 days for moments where a user corrected a definition, restated business logic, or re-explained a term mid-conversation. List the correction, how often it recurs across threads and users, and whether the ontology currently covers it. Rank by recurrence.
```

> ✅ You'll see: The unproposed patches — knowledge that evaporates after each thread. Recurring ones become this week's patches.

**Prompt for the learner to run:**
```
For the top recurring correction: locate where it belongs in the existing ontology, draft the minimal targeted edit (don't touch unrelated files), and open it for review with a description of which threads motivated it.
```

> ✅ You'll see: One focused patch. Ten one-file patches a month beat one forty-file patch a quarter — reviewable, revertible, and the golden suite isolates exactly which change broke what.

**Checkpoint before moving on:**
- [ ] You merged the Warning Breakdown and corrections list into one ranked backlog
- [ ] You shipped at least one minimal targeted patch from it
- [ ] Proposers get fast reviews and reasoned denials
- [ ] The three loop metrics are in your monthly routine

## Module 5 · Scaling Across Teams

**Prompt for the learner to run:**
```
Scan the team folders for definitions that duplicate or near-duplicate each other, or that threads from other teams keep referencing. Recommend which should be promoted to shared/, and flag any whose duplicates have drifted apart — those need reconciliation, not just promotion.
```

> ✅ You'll see: The promotion queue — including the dangerous case: near-duplicates that diverged. Reconcile (one definition wins; the alternative survives under a clearly distinct name), then promote.

**Checkpoint before moving on:**
- [ ] shared/ vs team folders reflects the boundary rule
- [ ] You ran the promotion scan and reconciled diverged duplicates
- [ ] Each team folder has designated editors and the right access
- [ ] You could onboard the next team from the five-step sequence

## Module 6 · The Runbook

**Prompt for the learner to run:**
```
Create runbook.md at the ontology root using this outline. Pre-fill everything you can observe — folder owners from file history, the access map from folder settings, the golden suite playbook and its schedule — and mark the rest [TODO]. Open it as a patch for review.
```

> ✅ You'll see: A draft runbook with the observable facts filled in — the remaining TODOs are the decisions only you can make.

**Checkpoint before moving on:**
- [ ] runbook.md exists at the ontology root, every section filled
- [ ] Your backup ran a full cycle from it without asking you anything
- [ ] The cadence table is real — each row has an owner and a calendar slot

