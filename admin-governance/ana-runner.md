# Admin Governance — Ana-Led Runner

> Pairs with `../ana-workshop-facilitator.md` (the generic HOW). This is the **module list** (the WHAT) for the Admin Governance workshop.
> Delivery: **inline** (paste it) or just give Ana the workshop URL — she fetches it. Facilitation: **in-thread, interactive — the learner runs each prompt, Ana coaches.**
> ⚙️ Setup/operational workshop — some steps are console/config, not data queries; Ana guides rather than runs them.

## Step-0 prompt (paste this, or use the workshop URL)

```
Hey Ana — facilitate the "Admin Governance" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/admin-governance/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Methodology (what this teaches)

A hands-on workshop for workspace administrators: identity and SSO, roles and permissions, connector governance, capability and model controls, monitoring, and the day-to-day operations of running TextQL safely at scale.

## Modules — Ana adapts each `[bracket]` to the connected data, then hands the prompt to the learner to run

| # | Module | Prompt for the learner to run (resolve the brackets) |
|---|---|---|
| 0 | The Admin Surface | Give me an admin baseline of this organization: how many members and what roles they hold, which connectors exist and whether they are read-only or read-write, which tools are enabled org-wide, and which AI models are enabled. Format it as a table I can save. |
| 1 | Identity: SSO & SCIM | From the audit log, list all member provisioning and deprovisioning events in the last 30 days. Flag any member created manually rather than via SCIM. |
| 2 | Roles & Permissions | Summarize the differences between the admin and member roles in this org as a table: resource, member access, admin access. Highlight anywhere the member role has write access. |
| 3 | Connector Governance | List every connector in this org with its type, visibility (public/private), and whether it is read-only or read-write. Flag anything read-write, and anything public that connects to a production database. |
| 4 | Capabilities & Models | From model analytics: which models were used most in the last 30 days, by which roles, and what share of total ACU spend does each model represent? Does anything suggest the wrong default for a role? |
| 5 | Monitoring & Audit | Create a playbook called "Admin Weekly Review" that runs every Monday at 8am: (1) ACU consumption last week vs the week before, top 10 users; (2) audit log summary — new members, role/permission changes, failed logins; (3) observability summary — run volume, warn rate vs prior week, top 3 warning types. Flag anything anomalous at the top. Email it to me. |
| 6 | Governance Operations | List private items with access requests pending for more than a week, with each item's owner — those are stuck queues I should nudge. |

## When done

Recap in 3 bullets. Offer to save anything the learner *built* as a **Playbook** for reuse. Do **not** write the workshop into the governed ontology.
