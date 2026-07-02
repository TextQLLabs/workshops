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

**Prompt for the learner to run:**
```
Turn this analysis into a playbook called "[name]" that runs every [Monday at 9am]: same metrics, same breakdowns, comparing to the prior period. Email it to me.
```

> **Note** — A playbook that has been previewed once is a playbook that will eventually surprise you. Most issues are caught in preview.

**Checkpoint before moving on:**
- [ ] You created a playbook (ideally from a thread) with scope, format, sources, and schedule
- [ ] You ran all three previews, including one on unusual data
- [ ] It's deployed, and you know where execution history lives
- [ ] Calendar note set to check the first two scheduled runs

## Module 2 · Reliable Prompts

> **Note** — "This is a weekly executive summary of product usage for the leadership team. Surface the most important trends and anomalies from the past 7 days."

> **Note** — "1. Pull daily active users for the past 7 days from user_activity , excluding internal accounts. 2. Compare to the prior 7-day period, calculate WoW change. 3. Break down by plan tier. 4. Flag any day where DAU dropped >15% from the 7-day average."

> **Note** — "If any metric returns null or zero, note it explicitly rather than omitting it. If the current period is incomplete, use the most recent complete day and say so."

> **Note** — "Deliver: (1) one-paragraph executive summary with 2–3 takeaways, (2) a line chart of DAU for 14 days, (3) a table of DAU by plan tier with WoW change."

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

> **Note** — Slack/Teams delivery requires the org-wide integration (admin, once). No channel option = that's the missing piece.

**Checkpoint before moving on:**
- [ ] Your playbook delivers to the channel your audience actually checks
- [ ] You restated the full recipient list and know external emails are dropped
- [ ] You verified the delivered report renders, including charts
- [ ] You asked Ana a follow-up in the delivery thread and got an answer

## Module 4 · Templates

> **Note** — "Generate a health report for {{Account}} ({{Segment}}). Pull their last 90 days of usage, flag churn-risk signals, and address the summary to {{CSM}}."

**Prompt for the learner to run:**
```
Turn my "[name]" playbook into a template backed by the attached CSV: treat each column header as a {{variable}} and reference {{Account}}, {{Segment}}, and {{CSM}} in the prompt. Run it for the [Acme Corp] row only as a test batch and show me where the batch results are collected.
```

> ✅ You'll see: one report for the test row, versioned with the batch — confirm the variables resolved before running all rows.

> **Note** — "If {{Account}} returns no rows for the period, produce a one-line report saying so — do not invent activity, and do not fail the batch."

**Checkpoint before moving on:**
- [ ] You built a backing table and referenced its columns with {{ }} in a playbook
- [ ] You ran a batch on selected rows and found the results in one place
- [ ] Your prompt handles the empty-entity edge case
- [ ] You previewed the weirdest row first

## Module 5 · Feed Agents

> **Where to click** — Feed (sidebar) → Manage Agents → New Agent — modes: Describe (Ana builds it), Templates (pre-built by function), From Scratch

**Prompt for the learner to run:**
```
Create an agent that watches [your domain — e.g., weekly revenue and pipeline]. Every [weekday at 8am], check for meaningful changes vs. the trailing average, investigate anything anomalous one level deeper, and post to the Feed only when there's something genuinely worth knowing. If nothing notable happened, don't post.
```

> ✅ You'll see: The agent built. Verify the four blocks: trigger matches data freshness; connectors are exactly the needed sources; and Outputs has at least one Feed channel — an agent with no Feed channel is private (it won't post, though it can still reply when @mentioned).

> **Note** — Post when: [metric] deviates more than [X]% from the [4-week] average; a new [top-10 account] appears or disappears; a trend reverses. Don't post for: normal fluctuation, weekends/holidays explaining a dip, repeats of this week's findings. When in doubt, stay quiet — your credibility is the product.

**Checkpoint before moving on:**
- [ ] Your agent is deployed with all four blocks deliberately configured
- [ ] Its instructions include an explicit posting threshold and a stay-quiet clause
- [ ] You triggered a collaboration via @mention and got a follow-up
- [ ] You know when you'd add a Slack or webhook trigger

## Module 6 · Operate

**Prompt for the learner to run:**
```
List all playbooks and agents I own with their last run status and delivery target. For each, when was the output last meaningfully engaged with (Slack reactions/replies, feed comments)? Recommend what to pause.
```

**Checkpoint before moving on:**
- [ ] You scanned the Playbooks dashboard and your agents' recent activity
- [ ] You can map each failure symptom to its first check
- [ ] Every automation you own is genuinely owned (and you know the departed-owner risk)
- [ ] You ran the pruning prompt and acted on it

