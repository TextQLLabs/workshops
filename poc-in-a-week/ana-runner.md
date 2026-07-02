# POC in a Week — Ana-Led Runner (v2: gated &amp; resumable)

> A **multi-session, state-aware** runner — the v2 pattern. Unlike the sit-down workshops, POC-in-a-week
> spans several days and has setup steps you do **in the console** (connect data, seed the ontology). So Ana
> **gates on prerequisites**, hands you the out-of-thread action and **waits**, **re-inspects** state on
> resume, and **persists progress** so a new thread picks up where you left off. Delivery: inline / URL.
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste at the START of each session/day)

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

## Instructions to Ana (the v2 HOW)

1. **Re-inspect on start.** Profile connectors, ontology contents, and playbooks/feeds. Infer the current Day from state — never assume or restart.
2. **Gate on preconditions.** Before each Day's work, check its prereq (below). If it's missing, hand the **out-of-thread action** ("In Connectors → Add → your warehouse; come back and say *connected*") and **wait**. Re-check on return. **Never fake completion.**
3. **One step at a time; hand prompts to the learner** to copy and run (same as the sit-down runners). Coach on the result.
4. **Persist progress.** When a Day completes, write "POC Day N complete + what we did" to `users/<email>/context.md` so a fresh thread resumes.
5. **Adapt to the connected data; never fabricate.**

## The week — each Day is a gate + the work

| Day | Ana checks (precondition) | What happens | Advance when |
|---|---|---|---|
| **0 · Before Day 1** | — | Define success criteria + the 2–3 questions that would prove value; confirm data access is coming | criteria written down |
| **1 · Connect &amp; Validate** | a warehouse connector attached? | If not → hand the console action &amp; wait. Once attached, validate with a few golden questions on real data | Ana returns real answers on your data |
| **2 · Seed the Ontology** | data validated | Define your top metrics/terms as governed context (propose patches); reconcile a metric defined two ways | first governed metric is live |
| **3 · Finish the Loop, Prove It Stuck** | ≥1 governed metric | Ask the hard question cold vs governed; confirm the number is consistent + traceable; the "N+1 document" update | same number twice, traceable |
| **4 · Automate &amp; Wow** | governed answers working | Build a **scheduled playbook** + a **feed agent** — these run *after* you leave the thread (the "runs later" piece) | playbook scheduled + feed live |
| **5 · The Readout** | playbook/feed exist | Ana assembles the **scored readout** — criteria hit? value shown? — into a report you can send | readout report generated |

## Notes

- Days 1–2 have real console / out-of-thread work — that's the **gate-and-return**: Ana hands you the action and waits, then re-checks. Exactly your "connect data, then come back" flow.
- Day 4's feed/playbook is the thing that **runs after** the session — the workshop literally produces something that persists.
- **Resume any day in a new thread**: paste Step-0; Ana re-inspects + continues from your saved progress.
- This is the reusable **v2 pattern** — connect-your-data, admin-governance, and ontology-operations should adopt the same gates + resume.
