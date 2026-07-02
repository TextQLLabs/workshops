# Self-Service Analytics — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Self-Service Analytics" workshop with me in this thread, on the data connected here.
Run it interactively: look at my data first (2–3 lines), then go ONE module at a time. For each module,
DON'T run the prompt yourself — give ME the prompt to copy and run as my next message, tell me what to look
for, then wait. When I run it, coach me on the result and hand me the next one. Start with what you see,
then Module 0.

[paste the module list below]
```

## Module 0 · Setup & Orientation
*🎯 Goal: signed in, connected to a data source, and oriented — so every later module is pure analysis*

> **Before you start — the 2-minute gate** — Confirm these five things now, so you don't stall at minute ten: (1) your login works; (2) you can see New Thread ; (3) at least one connector appears under the "+" menu; (4) a test question ("how many rows in our main table?") returns an answer; (5) if any of these fail — stop and ask your workspace admin or TextQL contact before investing time. Everything past this point assumes a working, connected workspace.

> **Heads up** — Connectors and tools are chosen before the first message of a thread. To change them later, start a new thread.

> **No data connected yet?** — You need at least one connected data source for this workshop. If your workspace shows no connectors: ask your workspace admin or TextQL contact — they can connect a source or provision a demo workspace with sample data in minutes. (If you ARE the admin, the Connect Your Data workshop covers every source type step by step.)

**Prompt for the learner to run:**
```
What data sources do I have access to in this thread, and what kinds of questions are they good for? Give me 5 example questions I could ask.
```

> ✅ You'll see: Ana lists the attached connectors, summarizes what's in them, and suggests starter questions tailored to your actual data.

**Prompt for the learner to run:**
```
How many [customers / members / orders] do we have in total?
```

> ✅ You'll see: a simple count — both you and Ana should find this easy. If this fails, something's wrong with the connection: see Troubleshooting below.

**Prompt for the learner to run:**
```
What was [your main metric] last month, and how does it compare to the month before?
```

**Prompt for the learner to run:**
```
[A question your team genuinely disagrees about or that takes an analyst a day — e.g. "What's our true customer churn rate this quarter?"] Tell me which definition you used.
```

> ✅ You'll see: the hard one is your baseline . Note the number AND the definition Ana assumed. By the wrap-up — after you've learned to refine, verify, and govern definitions — re-ask it and compare. That before/after is the whole point of the workshop.

**Checkpoint before moving on:**
- [ ] You have a thread open with at least one connector attached
- [ ] Ana answered the hello prompt with a real description of your data
- [ ] You can see Threads, Dashboards, and Playbooks in the sidebar
- [ ] You ran the three baseline questions and noted the answers (especially the hard one's definition)

## Module 1 · Your First Question
*🎯 Goal: ask real business questions in plain English, refine them conversationally, and understand what Ana does on your behalf*

**Prompt for the learner to run:**
```
How many [orders / signups / tickets / sessions] did we have last month? How does that compare to the month before?
```

> ✅ You'll see: Ana find the right table on her own, write and run the query, and answer with the numbers plus the month-over-month change. Tool steps appear as collapsed cells — expandable, but optional.

**Prompt for the learner to run:**
```
Break that down by [region / product / channel / team]. Which one drove the change?
```

> ✅ You'll see: Ana reuse the context from the previous answer — no restating — and return a breakdown with the main driver called out.

**Prompt for the learner to run:**
```
Are we doing well this quarter?
```

> ✅ You'll see: one of two good behaviors — Ana either asks which metric you mean (revenue? volume? retention?), or states the assumption she's making before answering. Both are correct: ambiguity gets surfaced, not silently guessed.

**Prompt for the learner to run:**
```
What was our customer satisfaction score in 2015?
```

> ✅ You'll see: Ana typically says plainly what's missing and what would be needed, rather than inventing a number. If she does return an answer, that's your cue for the most important skill in this workshop: ask "what data did you use for that?" — verifying is Module 2's whole job.

**Checkpoint before moving on:**
- [ ] You got a correct answer to a concrete question about your data
- [ ] You refined it with a follow-up without restating the original question
- [ ] Ana clarified or stated an assumption on a vague question
- [ ] Ana declined to invent an answer when data was missing

## Module 2 · Trusting the Answer
*🎯 Goal: verify how an answer was produced — defend the number in a meeting, challenge it when something looks off*

**Prompt for the learner to run:**
```
Explain in plain English exactly how you calculated that number: which table, which filters, which time window, and what you counted. What assumptions did you make?
```

> ✅ You'll see: a step-by-step explanation a non-technical stakeholder could follow — table, filters, definitions, assumptions, caveats.

**Prompt for the learner to run:**
```
What was [metric you already know] for [period]? I have a reference number — I want to see if we match.
```

> ✅ You'll see: either a match (calibration confirmed) or a mismatch. A mismatch is useful — follow with "Why might your number differ from [reference]?" Common causes: time zones, filters (test accounts? refunds?), definitions. You just found a definitional gap; Module 6 fixes it for everyone.

**Prompt for the learner to run:**
```
For the metrics you just gave me, which came from governed ontology definitions and which did you compute ad hoc? What's the difference in how much I should trust each?
```

> ✅ You'll see: which numbers carry your org's stamp of approval and which are best-effort. Governed numbers are consistent for everyone who asks; ad-hoc ones are transparent but not yet standardized.

**Prompt for the learner to run:**
```
Give me a "methodology footnote" for that answer: source, time window, filters, whether the definition is governed, and data freshness.
```

> ✅ You'll see: all five trust dimensions in one footnote you can paste under any chart in a deck.

**Checkpoint before moving on:**
- [ ] You expanded a tool cell and saw the actual SQL behind an answer
- [ ] Ana explained a calculation in plain English on request
- [ ] You reconciled (or explained a difference vs.) a number you already knew
- [ ] You know how to ask whether a metric is governed

## Module 3 · Visualize & Go Deeper
*🎯 Goal: turn answers into charts, and go beyond "what happened" into segmentation, trends, and "why"*

**Prompt for the learner to run:**
```
Show me [metric] by month for the last 12 months as a chart.
```

> ✅ You'll see: an interactive chart rendered in the thread — hover for values; it's saved with the conversation.

**Prompt for the learner to run:**
```
Make that a stacked bar chart by [segment], add the trend as a line on top, and highlight the months where we declined.
```

> ✅ You'll see: the chart rebuilt to spec. Other useful asks: "log scale", "sort descending", "show only top 8 and group the rest as Other", "add a target line at [value]".

**Prompt for the learner to run:**
```
Segment [metric] by [customer tier / region / plan / cohort]. Which segments are growing, which are shrinking, and which one explains most of the overall change?
```

> ✅ You'll see: a segment-level view plus a narrative pointing at the segments that matter. This is the workhorse move of self-service analytics.

**Prompt for the learner to run:**
```
Plot [metric] weekly for the past year. Is there a trend after accounting for seasonality? Anything unusual — spikes, drops, level shifts — I should know about?
```

> ✅ You'll see: trend analysis with anomalies flagged and quantified, not just eyeballed.

**Prompt for the learner to run:**
```
[Metric] changed by [amount] in [period]. Decompose the change: how much came from each segment, and was it driven by volume, price/intensity, or mix? Rank the contributors.
```

> ✅ You'll see: a contribution breakdown — the kind of analysis that used to take an analyst a spreadsheet afternoon — in one turn.

**Prompt for the learner to run:**
```
Is there a relationship between [metric A] and [metric B] across [customers / regions / weeks]? Show a scatter and tell me how strong the relationship is — and warn me about anything that would make it misleading.
```

> ✅ You'll see: the relationship quantified, with honest caveats (correlation is not causation, outliers, confounders).

**Prompt for the learner to run:**
```
Based on the trend, project [metric] for the next quarter. Show the range of uncertainty, and list the assumptions baked into the projection.
```

> ✅ You'll see: a forward projection with confidence bands and explicit assumptions — clearly labeled as a projection.

> **Go deeper** — Charts, reports, and live dashboards as real products — publishing, filters, refresh, maintenance — get a full workshop: Dashboards & Reporting .

**Checkpoint before moving on:**
- [ ] You produced and restyled a chart with plain-English direction
- [ ] You ran a segmentation and identified the driving segment
- [ ] You decomposed a metric change into contributors
- [ ] You got a forecast with uncertainty and assumptions stated

## Module 4 · From Answers to Assets
*🎯 Goal: turn a one-off analysis into something durable — a shareable report, a live dashboard, or an exportable file*

> **Sharing > screenshotting** — A shared thread keeps the context and the "show your work" trail attached to the number. A screenshot strips all of that away. When a decision rides on the number, share the thread.

**Prompt for the learner to run:**
```
Turn this analysis into a polished report: an executive summary up top, then the key charts with one-paragraph takeaways each, then a methodology appendix. Make it something I can send to my VP.
```

> ✅ You'll see: a structured report assembled from the thread. Ask for a PDF or a slide deck version if you need a specific format.

**Prompt for the learner to run:**
```
Build a dashboard from this analysis with: [metric] by month, the segment breakdown, and the top-10 table. Add a filter so viewers can pick their own region and date range.
```

> ✅ You'll see: an interactive dashboard created and linked. It appears under Dashboards, viewers can use the filters, and the data refreshes from the source — no more screenshot-and-paste reporting.

**Prompt for the learner to run:**
```
On that dashboard: add a week-over-week comparison card at the top, move the table to the bottom, and default the date range to the last 90 days.
```

> ✅ You'll see: the dashboard updated in place. Only the creator can edit; everyone else gets view + filter.

**Prompt for the learner to run:**
```
Export the segment breakdown as a CSV I can download. Also give me the chart as a standalone image.
```

> ✅ You'll see: downloadable files attached to the thread — CSV for spreadsheets, images for decks.

**Checkpoint before moving on:**
- [ ] You shared a thread link
- [ ] You generated a report with summary + charts + methodology
- [ ] You created a dashboard with at least one filter
- [ ] You exported a CSV

## Module 5 · Automate It
*🎯 Goal: stop re-asking the same questions — schedule recurring analyses, and set up an agent that watches your domain proactively*

**Prompt for the learner to run:**
```
Create a playbook called "[Team] Weekly Snapshot" that runs every Monday at 9am: [metric 1] and [metric 2] for last week vs the week before, the segment breakdown, and anything unusual flagged at the top. Email it to me.
```

> ✅ You'll see: the playbook created and visible under Playbooks in the sidebar. Confirm the schedule and delivery before relying on it.

> **Delivery prerequisites** — Slack/Teams delivery requires the org-wide integration (admin, set up once) — no channel option means that's the missing piece. Email recipients must be org members ; external addresses are silently dropped.

**Prompt for the learner to run:**
```
Make that playbook's report style concise — headline numbers and flags only, no methodology section. Also send it to our team Slack channel [#channel-name].
```

> ✅ You'll see: delivery and style updated. Playbooks support email recipients, Slack channels, and executive / verbose / concise styles.

**Prompt for the learner to run:**
```
Create a feed agent that keeps an eye on [your domain — e.g., "our sales pipeline" / "product usage" / "support tickets"]. Every morning, look for meaningful changes, anomalies, or trends worth knowing about, and post only when there's something genuinely interesting. Build on what you found in previous runs.
```

> ✅ You'll see: the agent created. Its posts appear in the Feed, where teammates can comment — and the agent responds.

> **Go deeper** — Reliable prompts that survive autopilot, batch templates, delivery options, and operating your automations: the Automation Deep-Dive workshop.

**Prompt for the learner to run:**
```
Remind me next Friday at 3pm to review the Q2 dashboard before the leadership meeting.
```

> ✅ You'll see: a one-time future message scheduled to you.

**Checkpoint before moving on:**
- [ ] You created a playbook with a schedule and a delivery target
- [ ] You adjusted its style and/or added a Slack channel
- [ ] You created (or scoped out) a feed agent for a domain you care about
- [ ] You can explain playbook vs. feed agent in one sentence each

## Module 6 · Make It Smarter
*🎯 Goal: teach the platform your team's language, definitions, and preferences — so every future answer gets better. This is where self-service compounds.*

**Prompt for the learner to run:**
```
Our official definition of [metric] is: [definition — e.g., "active user = logged in AND took at least one action, excluding internal accounts"]. Save this to the ontology so it's used consistently from now on.
```

> ✅ You'll see: a change proposal (patch) created for your org's context library — like a pull request for business logic. Once an admin approves, everyone's answers use your definition. In most sessions no approver is present — that's normal. After approval lands (ask your admin), re-ask your Module 2 reconciliation question and watch it route through your definition.

> **Governed by design** — You propose, an owner approves, every change is versioned and revertible. That's why teaching the platform is safe.

**Prompt for the learner to run:**
```
Some vocabulary to remember: "NRR" means net revenue retention; "the big three" means our top 3 enterprise accounts: [names]; "EOQ crunch" means the last 2 weeks of a quarter. Save these to the ontology.
```

> ✅ You'll see: a patch proposing the glossary additions. After approval, asking "how did the big three trend through EOQ crunch?" just works.

**Prompt for the learner to run:**
```
Remember my preferences: I want concise answers with the headline number first; I work in [timezone]; when I say "my accounts" I mean accounts where I'm the owner; default time windows to the current quarter.
```

> ✅ You'll see: your personal context file created/updated. Future threads pick it up automatically. (Personal vs. role vs. org context — and full role personas — get their own workshop: The Context Stack .)

**Prompt for the learner to run:**
```
Save this as my "[name]" workflow: every month I (1) pull [metric] by segment, (2) compare to the plan numbers, (3) flag segments more than 10% off plan, (4) format as an email draft to [audience]. Next time I say "run my [name] workflow," do all of it.
```

> ✅ You'll see: the workflow documented in your context. One sentence now replaces a 30-minute ritual.

**Checkpoint before moving on:**
- [ ] You proposed at least one definition or vocabulary patch
- [ ] You set personal preferences
- [ ] You know org-level (reviewed) vs personal (immediate) context
- [ ] You know where self-service ends and the data team's ontology work begins

