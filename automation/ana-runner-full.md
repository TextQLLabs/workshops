# Automation — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Automation" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/automation/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Automation Map

### 0.1 · Three primitives

### 0.2 · Match frequency to data
The universal rule: match trigger frequency to how often the underlying data changes . A daily playbook on a weekly-loading warehouse sends six identical reports; an hourly agent on daily data posts noise.

### 0.3 · Take inventory

**Prompt for the learner to run:**
```
List the playbooks and feed agents that already exist in this org: name, owner, schedule, and delivery target. Flag anything that looks duplicated or that hasn't run successfully recently.
```

> ✅ You'll see: The current automation estate. Duplicated weekly reports are how teams end up with two "official" numbers.

**Checkpoint before moving on:**
- [ ] You can name the three primitives and the question shape each fits
- [ ] You know the dashboard boundary (explore yourself vs delivered to you)
- [ ] You inventoried existing automations before building new ones

## Module 1 · Your First Playbook

### 1.1 · Two ways to create

**Prompt for the learner to run:**
```
Turn this analysis into a playbook called "[name]" that runs every [Monday at 9am]: same metrics, same breakdowns, comparing to the prior period. Email it to me.
```

### 1.2 · The four configuration pieces

### 1.3 · The 3-preview rule

> **Note** — A playbook that has been previewed once is a playbook that will eventually surprise you. Most issues are caught in preview.

### 1.4 · Deploy and monitor
Click Deploy . The Playbooks dashboard shows execution history for the whole team (creator filter for yours). Check the first two scheduled runs — that's where environment differences (timezones, load timing) show up.

**Checkpoint before moving on:**
- [ ] You created a playbook (ideally from a thread) with scope, format, sources, and schedule
- [ ] You ran all three previews, including one on unusual data
- [ ] It's deployed, and you know where execution history lives
- [ ] Calendar note set to check the first two scheduled runs

## Module 2 · Reliable Prompts

### 2.1 · Why playbook prompts are different
In chat, Ana asks for clarification and you course-correct live. In a playbook, the prompt carries the full weight alone . Before writing, answer: objective and audience? which tables? what time period — and how is "today" handled? what output? what edge cases?

### 2.2 · The four-part structure
1 · Objective — what this is for, who reads it:

> **Note** — "This is a weekly executive summary of product usage for the leadership team. Surface the most important trends and anomalies from the past 7 days."

> **Note** — "1. Pull daily active users for the past 7 days from user_activity , excluding internal accounts. 2. Compare to the prior 7-day period, calculate WoW change. 3. Break down by plan tier. 4. Flag any day where DAU dropped >15% from the 7-day average."

> **Note** — "If any metric returns null or zero, note it explicitly rather than omitting it. If the current period is incomplete, use the most recent complete day and say so."

> **Note** — "Deliver: (1) one-paragraph executive summary with 2–3 takeaways, (2) a line chart of DAU for 14 days, (3) a table of DAU by plan tier with WoW change."

### 2.3 · Date handling — the #1 failure source

### 2.4 · Pin the format with a sample
The single most effective trick: include a sample report in the prompt — exact structure, headings, phrasing — clearly marked as a sample with old data. Runs converge on the template instead of reinventing the layout weekly.

### 2.5 · Upgrade your Module 1 playbook

**Prompt for the learner to run:**
```
Rewrite my "[name]" playbook prompt into the four-part structure: objective, explicit steps with table names, edge-case handling for nulls and incomplete periods, and an output format section with a sample report. Show me the before and after.
```

> ✅ You'll see: A disciplined rewrite. Re-run the 3-preview rule on it before redeploying.

**Checkpoint before moving on:**
- [ ] Your playbook prompt has all four parts, with explicit tables and filters
- [ ] Dates are relative, anchored, and timezone-explicit
- [ ] A sample report pins the output format
- [ ] The rewritten prompt re-passed all three previews

## Module 3 · Delivery

### 3.1 · The options

> **Note** — Slack/Teams delivery requires the org-wide integration (admin, once). No channel option = that's the missing piece.

### 3.2 · Pick by audience behavior

### 3.3 · Style and recipients

### 3.4 · Test the full loop
Do: trigger a manual run with delivery configured. Verify the report arrived, charts render (auth-protected images failing in email is a known gotcha — prefer Slack or PDF attachment if images break), and a follow-up question in the Slack thread gets answered.

**Checkpoint before moving on:**
- [ ] Your playbook delivers to the channel your audience actually checks
- [ ] You restated the full recipient list and know external emails are dropped
- [ ] You verified the delivered report renders, including charts
- [ ] You asked Ana a follow-up in the delivery thread and got an answer

## Module 4 · Templates

### 4.1 · The idea
Templates connect a playbook to a backing table : column headers become variables, each row becomes one execution context. Use cases: weekly sales analysis per product, customer health per enterprise account, KPIs per store location.

### 4.2 · Build one
1 — backing table (CSV; headers are variable names):

> **Note** — "Generate a health report for {{Account}} ({{Segment}}). Pull their last 90 days of usage, flag churn-risk signals, and address the summary to {{CSM}}."

**Prompt for the learner to run:**
```
Turn my "[name]" playbook into a template backed by the attached CSV: treat each column header as a {{variable}} and reference {{Account}}, {{Segment}}, and {{CSM}} in the prompt. Run it for the [Acme Corp] row only as a test batch and show me where the batch results are collected.
```

> ✅ You'll see: one report for the test row, versioned with the batch — confirm the variables resolved before running all rows.

### 4.3 · The discipline transfers — times N
Everything from Module 2 applies more: the prompt runs N times unattended. Add the template-specific edge case:

> **Note** — "If {{Account}} returns no rows for the period, produce a one-line report saying so — do not invent activity, and do not fail the batch."

### 4.4 · When templates beat the alternatives

### Worked template: the Weekly Ontology Gap Review
The highest-leverage recurring playbook most teams never build: one that makes your ontology learn from usage . It mines real activity for missing semantics and drafts the fixes — a named owner just reviews.

**Prompt for the learner to run:**
```
Create a playbook "Weekly Ontology Gap Review" that runs Monday 9am: review the last 14 days of usage - repeated questions, manual SQL users wrote or copied, failed ontology lookups, and mid-thread corrections. Group them by business concept (not exact wording), flag concepts hit by multiple users or roles, and for each propose ONE small reviewable change: a new metric label or dimension in an existing surface, a glossary entry, or a routing-README update. Draft the patches for review - never apply them - and end with unresolved gaps as work items with suggested owners. Post the summary to [#channel].
```

> ✅ You'll see: a standing loop — Ana drafts, owners approve, the ontology gets cheaper and sharper every week, and gaps become tracked work items instead of buried chat history.

**Checkpoint before moving on:**
- [ ] You built a backing table and referenced its columns with {{ }} in a playbook
- [ ] You ran a batch on selected rows and found the results in one place
- [ ] Your prompt handles the empty-entity edge case
- [ ] You previewed the weirdest row first

## Module 5 · Feed Agents

### 5.1 · What an agent is
A specialist analyst watching one slice of your business. On each trigger it opens a behind-the-scenes thread with Ana, runs its instructions with the same tools as a chat (SQL, Python, web search, ontology), and publishes to its outputs.

### 5.2 · Create one

> **Where to click** — Feed (sidebar) → Manage Agents → New Agent — modes: Describe (Ana builds it), Templates (pre-built by function), From Scratch

**Prompt for the learner to run:**
```
Create an agent that watches [your domain — e.g., weekly revenue and pipeline]. Every [weekday at 8am], check for meaningful changes vs. the trailing average, investigate anything anomalous one level deeper, and post to the Feed only when there's something genuinely worth knowing. If nothing notable happened, don't post.
```

> ✅ You'll see: The agent built. Verify the four blocks: trigger matches data freshness; connectors are exactly the needed sources; and Outputs has at least one Feed channel — an agent with no Feed channel is private (it won't post, though it can still reply when @mentioned).

### 5.3 · The agent's editorial bar
The hardest part isn't the analysis — it's the posting threshold . Spell it out:

> **Note** — Post when: [metric] deviates more than [X]% from the [4-week] average; a new [top-10 account] appears or disappears; a trend reverses. Don't post for: normal fluctuation, weekends/holidays explaining a dip, repeats of this week's findings. When in doubt, stay quiet — your credibility is the product.

### 5.4 · Collaboration via @mentions
When a post or comment @mentions another agent , that agent is triggered to read the post and follow up with its own analysis. Revenue agent spots an anomaly → cost agent gets mentioned → cost agent checks whether cloud spend spiked the same week. No pre-configured pipeline.

### 5.5 · Beyond the schedule

### 5.6 · Playbook or agent? The final word

**Checkpoint before moving on:**
- [ ] Your agent is deployed with all four blocks deliberately configured
- [ ] Its instructions include an explicit posting threshold and a stay-quiet clause
- [ ] You triggered a collaboration via @mention and got a follow-up
- [ ] You know when you'd add a Slack or webhook trigger

## Module 6 · Operate

### 6.1 · Monitor

### 6.2 · Debug failures in order

### 6.3 · Ownership and lifecycle

**Prompt for the learner to run:**
```
List all playbooks and agents I own with their last run status and delivery target. For each, when was the output last meaningfully engaged with (Slack reactions/replies, feed comments)? Recommend what to pause.
```

### 6.4 · The compounding setup
Governed metrics (ontology) feed playbooks and agents (this workshop), whose findings link to dashboards (Dashboards workshop) for exploration, with admins watching quality org-wide (Admin workshop). Each automation should slot into that loop — not exist as an island.

### You're done
You can automate recurring reports with playbooks that survive autopilot, batch across entities with templates, deploy agents with editorial judgment, and operate the estate responsibly. See the Playbook Prompt Library (the final section of this workshop) for six battle-tested playbook skeletons.

**Checkpoint before moving on:**
- [ ] You scanned the Playbooks dashboard and your agents' recent activity
- [ ] You can map each failure symptom to its first check
- [ ] Every automation you own is genuinely owned (and you know the departed-owner risk)
- [ ] You ran the pruning prompt and acted on it

