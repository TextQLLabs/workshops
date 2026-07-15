# Sales Gtm — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Sales Gtm" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/sales-gtm/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · Your CRM, Mapped

### 0.1 · The read-path decision

> **Note** — The mature pattern: read from the mirror, write through the API . Either alone works for this workshop — write steps just need the API path.

### 0.2 · Build your translation table

**Prompt for the learner to run:**
```
Describe the CRM data you can see: what are the objects for companies/accounts, deals/opportunities, contacts, and activities? For deals: what are the stage values in order, and which fields hold amount, close date, and owner? Give me 5 example questions this data can answer.
```

> ✅ You'll see: Your CRM's actual shape. Write the mapping down — every later prompt uses the generic terms.

### 0.3 · Sanity-check the numbers

**Prompt for the learner to run:**
```
What is the total open pipeline right now — count and value by stage? I'll check this against [the CRM's own pipeline view / last week's forecast deck].
```

> ✅ You'll see: The headline numbers. Reconcile before proceeding — mismatches mean sync lag, excluded record types, or differing "open" definitions. Every later module depends on this trust.

**Checkpoint before moving on:**
- [ ] You know your read path (API, mirror, or both) and its implications
- [ ] Your translation table is filled in
- [ ] Ana's pipeline total reconciles with the CRM's own view — or you know exactly why it differs

## Module 1 · Pipeline Questions

### 1.1 · The state of the pipeline

**Prompt for the learner to run:**
```
Show open pipeline by stage: count, total value, and average deal size per stage. Then compare to 30 days ago — where did pipeline grow or shrink?
```

> ✅ You'll see: The funnel and its movement — the 30-day comparison turns a status into a story.

### 1.2 · What's moving — and what isn't

**Prompt for the learner to run:**
```
Which deals changed stage in the last 14 days — list them with from→to, owner, and value. Separately, which open deals haven't had any stage change in 60+ days?
```

> ✅ You'll see: Momentum and stagnation side by side. The stagnant list feeds Module 4.

### 1.3 · This quarter's reality

**Prompt for the learner to run:**
```
For deals with close dates this quarter: total value by stage, split by [owner/team]. Which deals carry the most value, and which of those have close dates in the next 2 weeks but are still in early stages?
```

> ✅ You'll see: The forecast-call view — including the early-stage-but-closing-soon deals that always need scrubbing.

### 1.4 · Win/loss patterns

**Prompt for the learner to run:**
```
For deals closed in the last 2 quarters: win rate by [segment/source/size band], average days-to-close for won vs lost, and at which stage lost deals most often died. Anything notable about what we win vs lose?
```

> ✅ You'll see: Conversion patterns that usually surprise someone — the foundation for coaching and forecast realism.

### 1.5 · My book (for individual sellers)

**Prompt for the learner to run:**
```
For deals I own: rank by value, flag anything with no activity in 14+ days, anything with a close date in the past, and anything in [late stage] missing [next step / champion / decision date]. What should I touch today?
```

> ✅ You'll see: A personal action list. Saved and re-run each Monday, this is many sellers' single biggest win — Module 5 schedules it.

**Checkpoint before moving on:**
- [ ] You ran the funnel + 30-day movement view
- [ ] You have a stagnant-deals list saved for Module 4
- [ ] You produced the forecast-call view for this quarter
- [ ] (Sellers) Your my-book prompt returns a real action list

## Module 2 · Account Research

### 2.1 · The account dossier

**Prompt for the learner to run:**
```
Build a dossier on [account]: everything in our CRM (deals past and present, contacts, recent activities, notes), plus — using web search — recent news, leadership changes, and anything suggesting budget or priority shifts. End with three angles for our next conversation.
```

> ✅ You'll see: Internal history fused with external context — the prompt to run before any first call with a known account.

### 2.2 · Whitespace in your book

**Prompt for the learner to run:**
```
Across my accounts: which have [product/tier] but not [other product]? Which look like our best customers (by [size/industry/usage]) but have unusually small deals? Rank the expansion whitespace.
```

> ✅ You'll see: The upsell map. With product usage in the warehouse alongside the CRM mirror, this is where the cross-source join earns its keep.

### 2.3 · Signals worth acting on

**Prompt for the learner to run:**
```
For my open deals and target accounts, check for signals: web-search news (funding, leadership, layoffs, product launches) in the last 30 days, plus internal signals — new contacts added, usage spikes or drops [if available]. Which three accounts have the strongest reason for outreach this week?
```

> ✅ You'll see: A prioritized outreach list with reasons attached.

### 2.4 · Write-back: capture what you learned — optional
If your CRM is connected via API (the write path):

**Prompt for the learner to run:**
```
Add a note to [account] in the CRM summarizing this research: the three conversation angles, key signals found, and the date. Don't modify any other fields.
```

> ✅ You'll see: The research persisted where the team works.

> **Note** — Write safety: be explicit about what may change ("don't modify any other fields"). For bulk updates: one record first, verify, then the rest.

**Checkpoint before moving on:**
- [ ] You built a dossier combining CRM history with live web research
- [ ] You ranked expansion whitespace across your book
- [ ] You have three signal-backed outreach targets for this week
- [ ] (API path) You wrote a research note back to the CRM safely

## Module 3 · Meetings

> **Note** — Value scales with what's connected: calendar + meeting recordings (e.g., Grain) make this automatic; CRM-only still works with manual inputs.

### 3.1 · Prep for today

**Prompt for the learner to run:**
```
Look at my calendar for [today/tomorrow]. For each external meeting: which account is it (match attendee domains to CRM accounts), what's the deal state, what happened in our last interactions, and — one web search each — anything new about the company? One prep card per meeting.
```

> ✅ You'll see: Prep cards for the day. Attendee-domain-to-account matching makes this run without naming the accounts.

### 3.2 · Deep prep for the big one

**Prompt for the learner to run:**
```
Deep prep for my meeting with [account] on [date]: full deal history and stakeholder map from the CRM; summaries of previous meeting recordings if available; their likely objections based on the deal's stage and history; and the three things we must accomplish in this meeting for the deal to advance.
```

> ✅ You'll see: A brief that would have taken an hour. The must-accomplish framing turns research into an agenda.

### 3.3 · After the call: capture and act

**Prompt for the learner to run:**
```
From my [account] call today: summarize what was discussed, list commitments made (ours and theirs) with owners, flag any new stakeholders mentioned, and draft a follow-up email confirming next steps.
```

> ✅ You'll see: The post-call package. Without recordings, paste raw notes and ask for the same structure. Pair with a CRM write-back note so the deal record stays current without anyone "doing hygiene."

### 3.4 · The weekly retro

**Prompt for the learner to run:**
```
Across my external meetings this week: which accounts did I touch, what commitments are now outstanding (mine and theirs), and which deals had meetings but no stage movement or follow-up logged?
```

> ✅ You'll see: The loop closed — meetings turned into accountability.

**Checkpoint before moving on:**
- [ ] You generated prep cards for a real day of meetings
- [ ] You ran one deep-prep brief with a must-accomplish list
- [ ] You produced a post-call package with a draft follow-up
- [ ] You know what the weekly retro catches (meetings without follow-through)

## Module 4 · Hygiene & Coaching

### 4.1 · The hygiene sweep

**Prompt for the learner to run:**
```
Audit all open deals for hygiene: missing or past close dates, missing amounts, missing [next step / champion fields], no activity in 21+ days, and stage older than [your stage-SLA]. Group by owner, worst first.
```

> ✅ You'll see: The scrub list, pre-sorted for the pipeline review. This exact prompt becomes a scheduled playbook in Module 5.

### 4.2 · Fix-forward, not blame

**Prompt for the learner to run:**
```
For [owner]'s flagged deals: draft the specific update needed for each — a realistic close date based on stage history, the missing field, or a recommended disqualification if dead. Make it a checklist they can clear in 15 minutes.
```

> ✅ You'll see: Hygiene converted to a 15-minute task with proposed answers, not homework.

### 4.3 · Coaching from the data

**Prompt for the learner to run:**
```
Compare [rep] to team medians over the last 2 quarters: win rate, average deal size, days-in-stage by stage, activities per open deal, and where their lost deals die. What are their two biggest gaps, and what does the top performer do differently at exactly those points?
```

> ✅ You'll see: A coaching card grounded in pattern, not anecdote — actionable rather than evaluative.

### 4.4 · Make definitions permanent

**Prompt for the learner to run:**
```
Save to the ontology: our official definitions — "open pipeline" means [stages X–Y, excluding Z record types]; "stalled" means [no stage change in 60 days AND no activity in 21]; "commit" means [definition]. Propose the patch for admin review.
```

> ✅ You'll see: A change proposal that, once approved, makes every seller's, leader's, and playbook's numbers agree.

**Checkpoint before moving on:**
- [ ] You ran the hygiene sweep grouped by owner
- [ ] You converted one owner's list into a fix-forward checklist
- [ ] You produced one coaching card from win/loss patterns
- [ ] Your team's pipeline definitions are proposed as an ontology patch

## Module 5 · The Cadence, Automated

> **Note** — Mechanics (3-preview rule, four-part prompts, delivery, editorial bars) live in the Automation Deep-Dive workshop — this module applies them to GTM.

### 5.1 · The Monday morning playbook (per seller)

**Prompt for the learner to run:**
```
Create a playbook "My Book Monday" that runs Mondays at 7am: my open deals ranked by value; anything with no activity in 14+ days; past-due close dates; deals in [late stage] missing [key fields]; and my three most-urgent actions. Concise style. Email it to me.
```

### 5.2 · The pipeline review pack (per team)

**Prompt for the learner to run:**
```
Create a playbook "Pipeline Review Pack" that runs [Wednesday 7am, before the 9am review]: funnel by stage vs last week; deals that moved; the hygiene sweep grouped by owner; this quarter's committed deals with risk flags; win/loss trend. Post it to [#sales-leadership] so we can ask follow-ups in thread.
```

> ✅ You'll see: The review meeting now starts with everyone having interrogated the same numbers in-thread.

### 5.3 · The deal-watch agent

**Prompt for the learner to run:**
```
Create a feed agent "Deal Watch" that checks the pipeline every weekday at 8am and posts ONLY when: a deal over [$X] changes stage (either direction); a committed deal's close date slips; a deal over [$X] goes quiet for 14 days; or total pipeline moves more than [Y]% in a week. Include the deal, the change, and one sentence of why it matters. If nothing qualifies, stay silent.
```

> ✅ You'll see: The editorial bar does the work — a deal-watch agent that posts everything is notifications; one that posts rarely is intelligence.

### 5.4 · The renewal/health watch (CS variant)

**Prompt for the learner to run:**
```
Create a feed agent "Renewal Radar" that runs weekly: accounts with renewals in the next 90 days, each scored on [usage trend, support volume, engagement recency]. Post only accounts that turned red or newly green. Relay to [#customer-success].
```

### 5.5 · Close the loop

**Checkpoint before moving on:**
- [ ] Your Monday playbook is deployed (and 3-previewed)
- [ ] The review pack posts to the channel where the meeting lives
- [ ] Deal Watch has a real editorial bar and stays silent on quiet days
- [ ] You can sketch the full cadence table for your team

