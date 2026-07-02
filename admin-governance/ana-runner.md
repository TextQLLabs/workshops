# Admin & Governance — Ana-Led Runner (v2: gated)

> A **gated** runner (v2). This workshop has steps you do in the **console/CLI**, so Ana inspects your
> workspace state, **gates on each prerequisite**, hands you the setup action and **waits**, then re-checks
> — and resumes if you step away. Pairs with `../ana-workshop-facilitator.md`. Delivery: inline / URL.
> **Full version:** `ana-runner-full.md` (all prompts + expected results) — use it when the tenant has no tight token limit; this concise file is for token-limited environments.

## Step-0 prompt (paste this)

```
Hey Ana — facilitate the "Admin & Governance" workshop with me on this workspace. It has steps I do in the
console/CLI, so: first inspect the current state (connectors / roles / context / git, as relevant) and
tell me where we're starting. Then go ONE step at a time — for analysis steps, hand me the prompt to
run; for console/setup steps, tell me exactly what to do, then WAIT until I say "done" and re-check
before moving on (don't assume a setup step is done). If I leave and come back, re-inspect and resume.
Start by telling me what you see.
```

## Methodology (what this teaches)

A hands-on workshop for workspace administrators: identity and SSO, roles and permissions, connector governance, capability and model controls, monitoring, and the day-to-day operations of running TextQL safely at scale.

## Instructions to Ana (the v2 HOW)

1. **Inspect current state first** — connectors / roles / context / git, as relevant; tell me where we start.
2. **Gate on each prerequisite.** For a console/setup step, hand me the exact action and **WAIT**; re-check on return; **never fake completion**.
3. **One step at a time.** Analysis steps → hand me the prompt + what to look for. Setup steps → the console action, then wait.
4. **Persist progress** so a new thread resumes where we left off.
5. **Adapt to my actual workspace; never fabricate.**

## Modules — Ana gates on setup steps, hands analysis prompts to the learner

| # | Module | Step — Ana hands you the prompt, or the console action |
|---|---|---|
| 0 | The Admin Surface | Give me an admin baseline of this organization: how many members and what roles they hold, which connectors exist and whether they are read-only or read-write, which tools are enabled org-wide, and which AI models are enabled. Format it as a table I can save. |
| 1 | Identity: SSO & SCIM | From the audit log, list all member provisioning and deprovisioning events in the last 30 days. Flag any member created manually rather than via SCIM. |
| 2 | Roles & Permissions | Summarize the differences between the admin and member roles in this org as a table: resource, member access, admin access. Highlight anywhere the member role has write access. |
| 3 | Connector Governance | List every connector in this org with its type, visibility (public/private), and whether it is read-only or read-write. Flag anything read-write, and anything public that connects to a production database. |
| 4 | Capabilities & Models | From model analytics: which models were used most in the last 30 days, by which roles, and what share of total ACU spend does each model represent? Does anything suggest the wrong default for a role? |
| 5 | Monitoring & Audit | Create a playbook called "Admin Weekly Review" that runs every Monday at 8am: (1) ACU consumption last week vs the week before, top 10 users; (2) audit log summary — new members, role/permission changes, failed logins; (3) observability summary — run volume, warn rate vs prior week, top 3 warning types. Flag anything anomalous at the top. Email it to me. |
| 6 | Governance Operations | List private items with access requests pending for more than a week, with each item's owner — those are stuck queues I should nudge. |

## When done

Recap in 3 bullets. Offer to save anything built as a **Playbook**. Do **not** write the workshop into the governed ontology.
