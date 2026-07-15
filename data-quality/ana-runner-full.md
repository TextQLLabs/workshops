# Data Quality — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Data Quality" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/data-quality/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Trust Stack

### 0.1 · The failure taxonomy
Every wrong answer has one of five causes. Naming the cause tells you the fix — and the defense that should have caught it:

### 0.2 · Locate your current exposure

**Prompt for the learner to run:**
```
For our [5] most-quoted governed metrics: which have any automated check pinning their value? For each unchecked one, tell me which failure modes above could currently change its value without anyone being alerted.
```

> ✅ You'll see: the honest gap map — typically most metrics have zero checks, meaning all five failure modes are live risks. That list is this workshop's worklist.

### 0.3 · The thesis
Write this somewhere the team sees it: accuracy is checked, not asserted. A governed definition says what the number means ; only a test says the number is right . The rest of this workshop builds the tests.

**Checkpoint before moving on:**
- [ ] You can name the five failure modes and the defense for each
- [ ] The gap map exists: every headline metric's unchecked failure modes are listed
- [ ] The team knows the thesis — and can say what "checked" will mean by Module 6

## Module 1 · Golden Datasets

### 1.1 · The canonical example: the finance statement
A golden query needs an external anchor — a value validated by a process outside the system under test. The best anchors already exist in every company:

### 1.2 · Pin the first golden query

**Prompt for the learner to run:**
```
Our audited [revenue] for [last closed quarter] was [exact value] per [the income statement]. Compute the same figure through our governed [revenue] definition — same period, same entity scope. Show the result, the difference if any, and the full derivation: source tables, filters, the rendered query.
```

> ✅ You'll see: either a match (pin it) or a gap (reconcile it — Module 4's method — before pinning; pinning a number that doesn't tie to the anchor defeats the purpose). When it ties:

**Prompt for the learner to run:**
```
Record this as a golden query: question, exact parameters (period, entity scope), expected value [X], anchor source [the statement], date pinned, and the governed definition it routes through. Save it to the ontology under golden/.
```

### 1.3 · Build out the set
Repeat for each metric from Module 0's gap map. Rules that keep the set trustworthy:

### 1.4 · Run the suite once, manually

**Prompt for the learner to run:**
```
Run every golden query in golden/: execute each with its recorded parameters, compare to the expected value, and report pass/fail with the divergence and the responsible definition file for any failure.
```

> ✅ You'll see: your first suite run. Failures at this stage are findings, not problems — each is a discrepancy that existed silently before today. Module 3 schedules this; Module 4 gives you the reconciliation method for the failures.

**Checkpoint before moving on:**
- [ ] At least [3] golden queries pinned, each anchored to an externally-validated value
- [ ] Every pin is on a closed period with a named anchor
- [ ] The set lives in the ontology, versioned with the definitions it tests
- [ ] The suite ran once and every failure has an owner

## Module 2 · Eval Cases

### 2.1 · Why golden queries aren't enough
A golden query checks one value at one parameter setting. Definitions fail at their edges — the empty month, the week that straddles a year boundary, the null that one aggregation counts and another skips. Eval cases are question/expected-answer pairs designed to walk those edges.

### 2.2 · Generate candidates, curate by hand

**Prompt for the learner to run:**
```
From our governed definitions in the ontology, generate candidate eval cases: for each definition, propose edge cases across empty periods, time boundaries, timezone handling, null semantics, and contrasts with its named variants. For each candidate: the question, why this edge matters for this definition, and how to establish the expected answer.
```

> ✅ You'll see: a candidate list — deliberately over-generated. Now the human step: curate . Keep cases where (a) the edge is real for your data, and (b) you can establish the expected answer independently (hand-compute it once, or anchor it like Module 1). Discard plausible-looking cases with unverifiable expectations — an eval case with a guessed expected answer is worse than none.

### 2.3 · Establish the expected answers
For each kept case, compute the expected answer once, by an independent path (hand SQL, a spreadsheet on exported rows), and record it with its derivation. This is the slow, valuable part — budget most of the module's time here.

**Prompt for the learner to run:**
```
Save these curated eval cases to the ontology under golden/evals/: question, parameters, expected answer, how the expectation was established, and the definition(s) exercised.
```

### 2.4 · Run them like the golden suite
Eval cases join the same scheduled run as Module 1's pins (next module). One nuance: some expected answers are behaviors , not values — "returns explicit zero," "states the null policy." Mark those, and have the suite check the behavior described, not string equality.

**Checkpoint before moving on:**
- [ ] Candidates generated across all five edge families
- [ ] Curation discarded every case whose expectation couldn't be independently established
- [ ] Each kept case has a hand-verified expected answer with its derivation recorded
- [ ] Behavior-type expectations are marked as such for the runner

## Module 3 · Drift Detection

### 3.1 · Schedule the suite

**Prompt for the learner to run:**
```
Create a playbook "Golden Suite" that runs [weekly, Sunday 11pm]: execute every golden query and eval case under golden/, compare to expected values/behaviors, and report. All pass: one line, nothing more. Any failure: lead with the failing item, expected vs actual, the divergence, the responsible definition file, and the three most likely causes ranked. Deliver to [#data-quality].
```

> ✅ You'll see: the drift alarm deployed. The all-pass-one-line rule is the alert discipline — a suite that sends a 40-row table of green checkmarks weekly trains everyone to stop reading it. (Playbook mechanics, the 3-preview rule: the Automation Deep-Dive workshop.)

### 3.2 · The two drifts
A golden query fails for exactly two structural reasons — and they have different owners:

### 3.3 · The triage prompt

**Prompt for the learner to run:**
```
The golden suite failed on [metric] for [period]: expected [X], got [Y]. Triage: (1) check the ontology history — was the definition changed since the last passing run, by whom, in which patch? (2) If the definition is unchanged, compare the underlying rows now vs what would produce the pinned value — what changed in the data: row counts, new/removed rows, value changes? (3) State which drift this is and who owns it.
```

> ✅ You'll see: the failure attributed — definition drift routes to the ontology review process (the git-history check is exactly Ontology Operations Module 1's discipline), data drift routes to the pipeline team with the changed-rows evidence attached.

### 3.4 · Close the loop on every red
A failed run isn't handled until one of three things happens, recorded in the ontology note: the data is corrected (and the suite re-passes), the definition change is ratified (and the pin updates in that patch), or the anchor itself is re-validated (rare — the audited number was restated). A suite that fires and gets silenced without one of these three is worse than no suite: it's a broken alarm everyone has learned to ignore.

**Checkpoint before moving on:**
- [ ] The suite runs on schedule with the all-pass-one-line discipline
- [ ] You can state the two drifts, their confirmation paths, and their owners
- [ ] One (real or simulated) failure was triaged with the three-step prompt
- [ ] The close-the-loop rule is agreed: every red ends in correction, ratification, or re-validation

## Module 4 · Reconciliation

### 4.1 · The four axes of every mismatch
Two systems reporting different values for "the same" number always differ along one or more of four axes. Work them in order — each is cheaper to check than the next:

### 4.2 · The decomposition prompt

**Prompt for the learner to run:**
```
[System A] reports [X] for [metric, period]; [System B] reports [Y] — a gap of [Z]. Decompose along four axes, in order: (1) Population — pull the entity/row sets from both sides and diff them; what's in one but not the other, and how much of the gap does that explain? (2) Filter — apply each side's inclusion rules to the common population; remaining gap? (3) Timing — align time-windows and conventions; remaining gap? (4) Definition — for the now-identical population, filters, and window, show both formulas side by side on the same rows. Attribute the full gap across the four axes.
```

> ✅ You'll see: the mismatch decomposed into an attribution table — "$[a] population (test accounts), $[b] timing (timezone), $[c] definition (net vs gross)" — instead of one mysterious number. The row-level diff in step 1 is the workhorse: gaps stop being arguments when both sides can see the actual rows.

### 4.3 · Resolve and record
Attribution isn't resolution. For each axis-finding, a decision: which side is right for the governed definition? Then make it permanent:

**Prompt for the learner to run:**
```
Record this reconciliation in the ontology: the two systems, the gap, the attribution across the four axes, the decisions made (which population/filter/timing/formula is canonical and why), and the governed definition this confirms or amends. If the definition is amended, propose the patch — including the updated golden pin in the same patch.
```

> ✅ You'll see: the resolution saved as a note + (if needed) a definition patch. The next person who hits this mismatch finds the answer in the ontology instead of re-running the investigation — that's the difference between a reconciliation and a reconciliation workflow .

### 4.4 · The recurring ones become checks
If a pair of systems disagrees once, they'll disagree again. Promote the reconciliation into the suite: a recurring check that computes both sides and alerts when the gap exceeds the explained, accepted residual.

**Prompt for the learner to run:**
```
Add a reconciliation check to the golden suite: compute [metric] from [System A] and [System B] for the last closed period; alert if the gap exceeds [the accepted residual] — including the four-axis attribution in the alert.
```

**Checkpoint before moving on:**
- [ ] One real mismatch decomposed along all four axes with the gap fully attributed
- [ ] The resolution is recorded in the ontology — decisions and rationale, not just numbers
- [ ] Any definition amendment shipped with its golden-pin update in the same patch
- [ ] The recurring pair became a scheduled reconciliation check

## Module 5 · The DQ Agent

### 5.1 · The watcher's scope
This is the Module 0 "bad data" and "stale loads" defense — catching problems at load time, before any query computes on them:

### 5.2 · Deploy the agent

**Prompt for the learner to run:**
```
Create a feed agent "Data Quality Watch" over [the core tables]: check freshness against expected load windows, row counts against trailing same-weekday baselines, null rates on [key columns] against baseline, and day-over-day discontinuities in [key aggregates]. Post ONLY on genuine issues: a table outside its load window by more than [grace period], counts off by more than [threshold], a null spike, or a discontinuity. One post per incident: the table, the symptom, the evidence, and the most likely upstream cause. Expected-empty tables [list] don't alert. A late load within [2 hours] is "late," not "failed." All-clear days: silence.
```

> ✅ You'll see: the watcher deployed with the editorial bar built in. The late-vs-failed distinction and the expected-empty list are what keep it credible — an agent that cries wolf on every slow load gets muted by Thursday. (Agent mechanics and editorial bars: Automation Deep-Dive .)

### 5.3 · The wow: trigger on ETL completion — required
A scheduled check at 6am races the pipeline; some days data lands at 6:15. The better pattern — webhook-trigger the agent from the pipeline itself , so validation runs the moment data lands:

> ✅ You'll see: validation that cannot race the load — it runs when the load says it's done, every time, including ad-hoc backfills that no schedule would have covered. The downstream effect: by the time the golden suite or any user query touches the data, the load-time checks have already passed (or already alerted). (Webhook setup details: Automation Deep-Dive and the Developer Workshop .)

### 5.4 · Wire it to the rest of the stack
The agent is the first line; connect it to the others: a data-quality alert on a table should suppress (or annotate) that night's golden-suite failure — "revenue golden failed AND the orders load was partial" is one incident, not two. Note the dependency in the runbook (next module) so responders see them together.

**Checkpoint before moving on:**
- [ ] The agent covers freshness, counts, nulls, and discontinuities with explicit thresholds
- [ ] Expected-empty and late-vs-failed rules are in place — silence is the default
- [ ] The ETL webhook trigger fires the agent on load completion, not on a clock
- [ ] The agent-to-golden-suite dependency is understood (one incident, not two alerts)

## Module 6 · The Quality Runbook

### 6.1 · The template
Create golden/RUNBOOK.md in the ontology (it's context — Ana can answer "who owns the golden suite?" from it). Adapt:

**Prompt for the learner to run:**
```
Create golden/RUNBOOK.md in the ontology from this template: fill in the owners, anchors, and cadences you can observe from golden/, the Golden Suite playbook, and the Data Quality Watch agent; mark everything else [TODO]. Open it for review.
```

### 6.2 · Pressure-test it

**Prompt for the learner to run:**
```
Simulate: the golden suite just failed on [your most important metric] — walk me through the runbook's red-alert protocol step by step for this case, and tell me where the runbook is ambiguous or missing an owner.
```

> ✅ You'll see: the runbook's gaps found by walking a scenario through it — cheaper now than during a real incident. Fix what it finds.

### 6.3 · The operating posture
The runbook's quiet message to the org: data quality here isn't a project that happened once — it's checks that run per-load and per-week, with named owners, an incident log, and a quarterly refresh. When an auditor, a board member, or a skeptical VP asks "how do you know these numbers are right?", the answer is this file and its history. Accuracy is checked, not asserted — and now you can show the checks.

**Checkpoint before moving on:**
- [ ] The runbook exists in the ontology with every owner named
- [ ] The red-alert protocol survived the simulation (or was fixed where it didn't)
- [ ] The quarterly eval-refresh is calendared with an owner
- [ ] You can answer "how do you know these numbers are right?" by pointing at the file

