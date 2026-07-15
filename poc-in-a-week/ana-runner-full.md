# POC in a Week — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate my "POC in a Week" with me. It runs across several sessions and has setup steps I
do in the console (connect data, etc.), so:
- RE-INSPECT first: what connectors are attached, what's in the ontology, what playbooks/feeds exist —
  and from that, tell me which Day I'm on.
- Guide me through the NEXT step only. If a step needs a console action (connect a warehouse, add a
  role), hand it to me, tell me exactly what to do, and WAIT — don't assume it's done. When I say "done,"
  re-check the state to confirm before moving on.
- When I finish a Day, save "POC Day N complete" + what we did to my personal context so a new thread can
  resume. Start by telling me what you see and which Day we're on.
```

## Module 0 · Before Day 1

### 0.1 · The access checklist
Chase these the week before; each missing item costs a half-day of the POC week:

### 0.2 · The question list — the artifact that matters most
With the team (not alone), write down 5–10 real questions — ones someone actually asked in the last quarter, ideally ones that were painful to answer. For each, record the current reality:

### 0.3 · Success criteria — what does "won" look like?
Agree these with the economic buyer or champion in writing before anything is connected:

**Checkpoint before moving on:**
- [ ] Every access item is secured or has a named owner and date
- [ ] The question list has 5–10 real questions with current time-to-answer recorded
- [ ] The list includes a cross-system question, a contested-definition question, and a hard one
- [ ] Success criteria are written and agreed with the person who'll judge Friday

## Module 1 · Day 1: Connect & Validate

### 1.1 · Connect the warehouse and documents
Connect the warehouse with the Module 0 credentials ( Connectors → New Connector ; source-specific detail in the Connect Your Data workshop — including the smoke-test discipline). Upload the sample documents to the workspace.

**Prompt for the learner to run:**
```
Pull the information schema for this connector. List the schemas and tables you can see, with approximate row counts where cheap. Flag anything that looks like a core entity (customers, orders, events) and anything you'd expect to see but can't.
```

> ✅ You'll see: what Ana can actually reach — which is the credential's view. Anything missing is a grants problem to fix today , not Wednesday.

### 1.2 · The improvement loop — question by question
Now work the Day-0 question list — but not as a test you sit through and grade at the end. Each question gets its own complete cycle: ask cold → score → teach → verify → next . You still capture the honest floor (the cold answer, verbatim, timed) — you just don't live there. Every question ends as a win, usually within 10–15 minutes.

### The loop, per question

**Prompt for the learner to run:**
```
[Question #1 from your Day-0 list, pasted verbatim — no rephrasing, no follow-up steering]
```

**Prompt for the learner to run:**
```
That answer was [wrong / incomplete / used the wrong definition — the correct answer is X per our trusted report]. Don't fix it yet: first tell me exactly what context you were missing — a definition, a document, a join, a table mapping — and where it should live so every future question gets it.
```

**Prompt for the learner to run:**
```
[The same question again, verbatim.] Cite the definition or context you used this time.
```

> ✅ You'll see: the miss become a diagnosis, the diagnosis become context, and the same question come back correct — in one sitting, in front of the people who asked it. Log each cycle in the running table: question · cold answer · cold ✓/✗ · time & steps taken · what we taught · warm answer · warm ✓/✗ · warm time .

> **Why loop instead of batch** — The old pattern — run all ten cold, grade them, fix them two days later — measures the same floor but makes Day 1 feel like a failure parade, and the audience is people, not robots. The loop produces identical before/after evidence (every cycle logs both halves) with the opposite emotional arc: value out of the gate, ten times in a row . Misses are still the point — a question list with no misses was too soft — but a miss now lives for fifteen minutes, not two days.

> **Keep the cold ask honest** — The loop only works if step 1 stays clean: verbatim question, fresh thread, no coaching before the first answer. Resist the urge to warm it up — the floor is what makes the ceiling impressive.

**Checkpoint before moving on:**
- [ ] The warehouse is connected and the schema-visibility check passed (or grants are being fixed today)
- [ ] 3–4 questions went through the full loop today: asked cold (once, no steering), scored, taught, verified
- [ ] Every cycle is logged with both halves: cold answer AND verified fix (the Day-5 evidence)
- [ ] The judge scored correctness today, against the trusted source

## Module 2 · Day 2: Seed the Ontology

### 2.1 · Choose the seeding path

### 2.2 · Run the seeding

**Prompt for the learner to run:**
```
Read the connected schema and the uploaded documents. Draft the ontology for this domain: the core entities and how they join, a business glossary from the documents, and the candidate metrics worth governing — flagging any metric where the documents and the schema suggest different definitions.
```

> ✅ You'll see: a drafted model with definitional conflicts flagged. The flags are tomorrow's gold — most of Day 1's misses trace to exactly these.

### 2.3 · Govern the three that matter
From the question list, take the 3 most definition-sensitive metrics — the ones where Day 1's cold run picked a definition instead of your definition:

**Prompt for the learner to run:**
```
Govern these three metrics: [metric 1] means [the org's definition — source, filters, edge cases]; [metric 2] means [...]; [metric 3] means [...]. Save them as governed definitions, and confirm by computing each for [last closed period] so we can check against [trusted source].
```

> ✅ You'll see: the three definitions saved and computed — and the computed values checked against the trusted source today , by the judge. Governing a metric to the wrong value is worse than not governing it.

### 2.4 · Keep going — it's iterative
Three governed metrics is the floor, not the ceiling — you have the day, and ontology building is an iterative loop, not a phase that ends . Keep working down the question list: govern the next definition, add the note that explains the next entity, fold in another document. Two guardrails keep the iteration honest: tie each addition to a real question from the list (speculative polish steals the signal tomorrow's misses would give you), and verify each governed metric as you add it rather than batch-checking later.

**Checkpoint before moving on:**
- [ ] The governed definitions created today are ACTIVE (approved/merged), not pending review — Day 3's warm run depends on it
- [ ] The seeding path was chosen deliberately (pack-adapted or built from schema + documents)
- [ ] The drafted model exists with definitional conflicts flagged
- [ ] The three contested metrics are governed and their computed values verified against the trusted source
- [ ] You stopped at three — the rest of the backlog waits for Day 3's evidence

## Module 3 · Day 3: Finish the Loop, Prove It Stuck

### 3.1 · Finish the loop
Work the remaining Day-0 questions through the same cycle from Day 1: ask cold, score, diagnose, teach, verify. By now the ontology from Day 2 is attached, so expect more questions to come back correct on the first ask — log those as cold-correct wins; they're evidence the context compounds .

**Prompt for the learner to run:**
```
[Next question from your Day-0 list, pasted verbatim — same one-ask discipline]
```

### 3.2 · Prove it stuck — the consistency check
This is the step that earns the word "governed." Take 2–3 questions you fixed on Day 1 and re-ask them — fresh threads, no coaching, different phrasings :

**Prompt for the learner to run:**
```
[A question you fixed on Day 1 — rephrased two different ways, each in its own fresh thread]
```

> ✅ You'll see: the same governed answer every time, citing the same definition — proof the Day-1 fix was a durable piece of context, not thread luck. If a re-ask drifts, that's a real finding: the fix lived in a thread instead of the ontology — promote it now.

### 3.2b · Assemble the scorecard

**Prompt for the learner to run:**
```
Here's our improvement-loop log [paste the running table: question, cold answer, cold result, what we taught, warm answer, warm result, times]. Summarize: cold vs warm correct counts, average time-to-answer vs the original current-path times from our Day-0 list, which question types needed teaching vs worked cold, and the consistency-check results. Format as the scoring section of an evaluation readout.
```

> ✅ You'll see: the POC's core evidence, computed from the week's own artifacts — every before/after captured live as it happened. The argument isn't "the tool is impressive"; it's " the loop works: taught in minutes, correct on re-ask, consistent across phrasings. "

### 3.3 · The misses log becomes the backlog
For each remaining miss, diagnose and sort:

### 3.4 · The honesty principle
Carry the remaining misses into Friday's readout, labeled. An evaluation with documented misses and a working improvement loop is more credible than a flawless demo — every buyer has seen flawless demos, and knows what they're worth.

**Checkpoint before moving on:**
- [ ] Every Day-0 question has completed the loop — asked, scored, taught where needed, verified
- [ ] The consistency check passed: Day-1 fixes re-asked fresh, rephrased — same governed answer
- [ ] The loop scorecard (cold/taught/current-path times) is assembled
- [ ] At least one miss was fixed live and re-run to a hit
- [ ] Remaining misses are diagnosed, sorted, and headed for the readout — not hidden

## Module 4 · Day 4: Automate & Wow

### 4.1 · The playbook — replace a real report
Pick a report the team genuinely builds by hand today (the Day-0 conversation usually surfaced one). Build it as a playbook:

**Prompt for the learner to run:**
```
Create a playbook "[their report name]" that runs [their actual cadence]: [the report's actual contents — metrics, comparisons, breakdowns, using the governed definitions]. Match the structure of the current report. If the period's data is incomplete, say so rather than reporting partials as final. Deliver to [their actual channel/email].
```

> ✅ You'll see: their report, automated, on their definitions. Run it manually now and put the output next to the hand-built version — that side-by-side is a readout exhibit. (Prompt discipline and the 3-preview rule: Automation Deep-Dive .)

### 4.2 · The feed agent — watch their domain

**Prompt for the learner to run:**
```
Create a feed agent "[Domain] Watch" that checks [their data] daily and posts ONLY when something material changes: [2–3 thresholds from their domain — a metric moving >X%, a new top-N entity, an anomaly]. One paragraph with the evidence. Silent otherwise.
```

> ✅ You'll see: the agent deployed. It likely won't fire before Friday — say that in the readout and show the configuration instead; an exception-only watcher that stayed correctly silent for two days is the feature working.

### 4.3 · The dashboard

**Prompt for the learner to run:**
```
Build a dashboard for [the domain]: a KPI row using our three governed metrics with period-over-period deltas, the main trend chart, a breakdown table, and filters for [date range] and [their key dimension]. Default to [last closed period].
```

> ✅ You'll see: a live dashboard on governed definitions (depending on workspace configuration, an admin may need to enable Dashboards). Verify the headline KPI against the trusted source before showing anyone — Day 5 demos only verified numbers. (Craft: Dashboards & Reporting .)

### 4.4 · The role persona (if access control is in the evaluation)
With the restricted test login from Module 0:

**Prompt for the learner to run:**
```
What data sources can I access, and what do you know about [a metric/folder the restricted role shouldn't see]?
```

> ✅ You'll see: the restricted view — scoped connectors, and genuine unawareness of restricted context rather than a declined request. Then ask something the role should see and confirm it works. Both directions, witnessed by the security stakeholder if there is one. (Full model: Admin & Governance .)

### 4.5 · Stage Friday
End the day by assembling the demo path: the improvement-loop scorecard, one live warm question, the playbook side-by-side, the dashboard, the agent config, the persona flip. Dry-run it once — 15 minutes, no surprises.

**Checkpoint before moving on:**
- [ ] The playbook reproduces a report the team actually builds, side-by-side verified
- [ ] The agent is deployed with real thresholds — and its silence-by-design is understood
- [ ] The dashboard's headline KPI is verified against the trusted source
- [ ] (If in scope) The persona showed both directions: restricted-invisible and permitted-visible
- [ ] Friday's demo path is staged and dry-run

## Module 5 · Day 5: The Readout

### 5.1 · Score against Day 0 — first, and in their words
Open the readout with the success-criteria table from Module 0, scored row by row:

### 5.2 · The before/after table
The week's centerpiece exhibit, one row per question:

**Prompt for the learner to run:**
```
Assemble the readout pack: the success-criteria scorecard, the before/after question table, the cold-vs-warm summary from Day 3, the remaining misses with their diagnoses and what each needs, and the automation inventory from Day 4. Executive summary up front — findings, not feature descriptions. PDF.
```

> ✅ You'll see: the readout assembled from the week's own artifacts. The misses section stays in — labeled, diagnosed, with the fix path. Honest misses next to a working improvement loop is the credibility play; a misses-free readout reads like marketing.

### 5.3 · The 15-minute exec demo
Their data, their questions, this script:

### 5.4 · What production rollout looks like
Close with the sketch, not a vague "next steps":

**Checkpoint before moving on:**
- [ ] The scorecard was presented first, scored against the Day-0 criteria verbatim
- [ ] The before/after table and the labeled misses both made it into the pack
- [ ] The exec demo ran on their data, their questions, inside 15 minutes
- [ ] The room left with a decision (or a named blocker) and the four-track rollout sketch

