# Executives — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Executives" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/executives/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · Orientation

### 0.1 · What this is
Ana is an analyst you talk to. She's connected to your company's actual systems — warehouse, CRM, finance — writes and runs the queries herself, and shows her work. You ask in plain English; the SQL happens underneath, inspectable any time.

### 0.2 · The discovery prompt

**Prompt for the learner to run:**
```
What data can you see, and what are five questions I should ask it?
```

> ✅ You'll see: your connected sources and a question menu calibrated to your actual business — not a demo script.

**Checkpoint before moving on:**
- [ ] The data Ana listed matches the systems your company runs on

## Module 1 · Ask Your Team's Questions

### 1.1 · The number

**Prompt for the learner to run:**
```
Revenue this quarter versus plan?
```

> ✅ You'll see: the number, the plan, the gap — in seconds, not in next Tuesday's deck.

### 1.2 · The why
The follow-up that normally costs a week of analyst time:

**Prompt for the learner to run:**
```
What's driving the miss?
```

> ✅ You'll see: the gap decomposed — which segment, which product, volume or price — with the biggest contributor named. No new meeting was scheduled to produce this.

### 1.3 · Decision support

**Prompt for the learner to run:**
```
I'm deciding [whether to expand the EMEA team]. What should I look at first?
```

> ✅ You'll see: the relevant evidence assembled — trends, comparisons, capacity signals — framed for the decision, not just dumped. You direct the follow-ups from here, conversationally.

> **Note** — Here's the real exercise: think of the question you didn't ask in the last board meeting because the answer would have taken analytics a week. Ask it now.

**Checkpoint before moving on:**
- [ ] You got a number, a why, and decision support without leaving the conversation
- [ ] You asked the question you skipped in the last board meeting

## Module 2 · Trust in Five Minutes

### 2.1 · Show the work
Take any answer from Module 1:

**Prompt for the learner to run:**
```
How did you calculate that?
```

> ✅ You'll see: the sources, the filters, the time window, the assumptions — in plain English. Underneath, the actual SQL is attached to the answer and inspectable by anyone you forward it to. Every number here carries its own audit trail.

### 2.2 · Is that the official number?

**Prompt for the learner to run:**
```
Is that the same revenue definition Finance uses?
```

> ✅ You'll see: whether the answer routed through your company's governed definition — the single, Finance-approved formula stored centrally — or was computed ad hoc. Governed numbers are the same for everyone who asks: you, your CFO, the board deck, the Monday report. That's the mechanism, not a promise.

### 2.3 · The honest no

**Prompt for the learner to run:**
```
What was our customer NPS in 2019?
```

> ✅ You'll see: if the data isn't there, Ana says so — what's missing and what it would take — rather than producing a confident invention. An analyst who says "I don't know" when she doesn't is the one you can trust when she says she does.

**Checkpoint before moving on:**
- [ ] You saw the full derivation of a number you'd quote
- [ ] You know whether your headline metric is governed — and asked who owns the definition if not

## Module 3 · Your Morning Briefing

### 3.1 · The daily digest
Set this up yourself, or watch the facilitator do it — it's one prompt either way:

**Prompt for the learner to run:**
```
Every weekday at 7am, email me: revenue pace vs plan, yesterday's [bookings/signups], cash position, and anything unusual — flagged first. Five lines, then detail.
```

> ✅ You'll see: a scheduled playbook created. Tomorrow at 7am it arrives; the morning after, too. The "anything unusual, flagged first" line means the days that matter read differently from the days that don't.

### 3.2 · The exception watcher
The digest reports daily; the watcher speaks only on exceptions:

**Prompt for the learner to run:**
```
Watch [revenue pace, pipeline, cash] daily. Alert me only when something moves more than [10%] off trend or plan. One sentence plus the chart. Otherwise, silence.
```

> ✅ You'll see: a feed agent created. Most days it says nothing — that's the design. When it does speak, it's worth reading, which is the opposite of every dashboard email you currently delete.

### 3.3 · Follow up where you are
Briefings delivered to Slack or Teams (depending on your workspace configuration) can be interrogated right in the thread — reply "why?" to the morning number and the analysis continues there. Your inbox stops being a dead end.

**Checkpoint before moving on:**
- [ ] The 7am digest exists and you know what arrives tomorrow
- [ ] The watcher is set with a threshold you chose — and you understand why silence is the feature

## Module 4 · The Board View

### 4.1 · The prompt

**Prompt for the learner to run:**
```
Board view: headline revenue vs plan, the 12-month trend chart, and three takeaways. Paste-ready.
```

> ✅ You'll see: the number, the chart, three findings written as findings — ready for the deck or the email to the board chair. Behind it: governed definitions (Module 2), full audit trail, and the same figures everyone else in the company gets when they ask.

### 4.2 · The executive sponsor ask
This platform compounds through one mechanism: governed definitions — and deciding what gets governed is an executive call, not a technical one. So, the ask:

**Checkpoint before moving on:**
- [ ] The board view is in your hands, paste-ready
- [ ] Your three never-disagree numbers are named and sent to your data team

