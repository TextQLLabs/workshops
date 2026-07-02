# Admin & Governance — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Admin & Governance" workshop with me on this workspace. It has steps I do in the
console/CLI, so: first inspect the current state (connectors / roles / context / git, as relevant) and
tell me where we're starting. Then go ONE step at a time — for analysis steps, hand me the prompt to
run; for console/setup steps, tell me exactly what to do, then WAIT until I say "done" and re-check
before moving on (don't assume a setup step is done). If I leave and come back, re-inspect and resume.
Start by telling me what you see.
```

## Module 0 · The Admin Surface

> **Note** — If a section is missing, you don't have the admin role — get it assigned before continuing. Also confirm Observability appears in the left sidebar (admin-only; Module 5).

**Prompt for the learner to run:**
```
Give me an admin baseline of this organization: how many members and what roles they hold, which connectors exist and whether they are read-only or read-write, which tools are enabled org-wide, and which AI models are enabled. Format it as a table I can save.
```

> ✅ You'll see: A configuration snapshot. Save it — it's your before picture, and writing it down is the first governance habit.

**Checkpoint before moving on:**
- [ ] You can see all admin sections in Settings, plus Observability in the sidebar
- [ ] You know which module covers each section
- [ ] You saved a baseline snapshot of members, roles, connectors, tools, and models
- [ ] You can name the four layers of control in order

## Module 1 · Identity: SSO & SCIM

> **Note** — Do this module in a staging org first if you have one. Identity changes affect everyone's login.

> **Where to click** — Settings → Configuration tab → Authentication section → OIDC Settings

> **Where to click** — Settings → Audit Log → filter Category = Authentication

> ✅ You'll see: "Created OIDC provider" and your test "Logged in" events, with actor, timestamp, IP, and auth method.

> **Where to click** — Settings → Configuration tab → SCIM Provisioning section → Create Token

**Prompt for the learner to run:**
```
From the audit log, list all member provisioning and deprovisioning events in the last 30 days. Flag any member created manually rather than via SCIM.
```

> ✅ You'll see: The lifecycle trail — and a flag on anyone bypassing the IdP, which after SCIM should be rare and deliberate.

**Checkpoint before moving on:**
- [ ] OIDC login works for a test account, and your admin session never lapsed
- [ ] The Created OIDC provider event is in the audit log
- [ ] SCIM token created, configured in the IdP, and stored in your secrets manager
- [ ] A test joiner appeared and a test leaver was deactivated without touching TextQL

## Module 2 · Roles & Permissions

> **Where to click** — Settings → Roles → Manage Permissions . Assignment: Settings → Members → role dropdown. Changes apply immediately.

**Prompt for the learner to run:**
```
Summarize the differences between the admin and member roles in this org as a table: resource, member access, admin access. Highlight anywhere the member role has write access.
```

> **Note** — Custom roles also scope which connectors, context files, and models the role can access — that's how "Finance sees the finance warehouse, Sales sees the CRM mirror" is done. Connector scoping: Module 3. Model scoping: Module 4.

**Prompt for the learner to run:**
```
User [email] says they can't see [dashboard/playbook/thread name]. Walk the four layers: do they have a role with read on that resource type, is the item public or private, is it shared with them, and for feed posts — can they read all referenced chats? Tell me the single change that grants access with least privilege.
```

> ✅ You'll see: A layer-by-layer diagnosis ending in one least-privilege change. Save this prompt — it answers most access tickets.

**Checkpoint before moving on:**
- [ ] You can recite the 4-permission pattern and both design implications
- [ ] You read the member defaults and made a deliberate decision about Connector write
- [ ] You created analyst-restricted, assigned a test user, and verified their access as them
- [ ] You can explain the feed inheritance rule and debug a private-dashboard access question

## Module 3 · Connector Governance

> **Where to click** — Connectors (sidebar) — note each connector's type, creator, public/private, and read-only vs read-write mode

**Prompt for the learner to run:**
```
List every connector in this org with its type, visibility (public/private), and whether it is read-only or read-write. Flag anything read-write, and anything public that connects to a production database.
```

> ✅ You'll see: Your data attack surface in one table. Read-write connectors deserve explicit justification — Ana can run INSERT/UPDATE/DDL through them.

> **Note** — Governance principle for shared accounts: the service account's warehouse role is your ceiling — grant the narrowest role that serves the use case, never a superuser. With per-member auth, the warehouse's own grants do the row-level work.

**Prompt for the learner to run:**
```
What data sources do I have access to in this chat?
```

> ✅ You'll see: Only the scoped connectors. If they see more, check in order: role connector list, connector visibility, second role assignment.

**Prompt for the learner to run:**
```
Which dashboards and playbooks depend on the [name] connector? I'm planning to decommission it.
```

> ✅ You'll see: The dependency list — migrate or archive these, announce, then delete in a maintenance window.

**Checkpoint before moving on:**
- [ ] Full connector inventory with mode and visibility; every read-write connector justified
- [ ] You can articulate shared vs per-member credentials for your top two connectors
- [ ] A test user in a scoped role sees only their connectors — verified in a real chat
- [ ] You know the three guardrail layers and which problem each solves

## Module 4 · Capabilities & Models

> **Where to click** — Settings → Capabilities

> **Where to click** — Settings → Models — three tabs: Model Catalog, Role Access, Analytics

**Prompt for the learner to run:**
```
From model analytics: which models were used most in the last 30 days, by which roles, and what share of total ACU spend does each model represent? Does anything suggest the wrong default for a role?
```

> **Where to click** — Settings → Specs — read-only; changes go through TextQL support

**Prompt for the learner to run:**
```
Show ACU consumption by week for the last 8 weeks, broken down by user. Who are the top 10 consumers, and is any single user trending sharply up?
```

> ✅ You'll see: Your spend early-warning system — Module 5 turns it into a scheduled playbook.

**Checkpoint before moving on:**
- [ ] Every Capabilities toggle reflects a decision you can defend, not an inherited default
- [ ] Model catalog matches your AI policy; premium models mapped to roles that need them
- [ ] You know the default limits well enough to triage a dead query without escalating
- [ ] You ran an ACU breakdown and know your top consumers

## Module 5 · Monitoring & Audit

> **Where to click** — Settings → Audit Log — filters: free-text + category + action + date range (Today / 7 / 30 / 90 days)

> **Note** — Audit logs and product metrics can stream to Datadog, Splunk, Grafana, Prometheus, or S3 via OpenTelemetry (docs → Observability Export). If you run a SIEM, wire this — don't make the TextQL UI your only copy.

> **Where to click** — Observability (left sidebar, admin-only)

**Prompt for the learner to run:**
```
Create a playbook called "Admin Weekly Review" that runs every Monday at 8am: (1) ACU consumption last week vs the week before, top 10 users; (2) audit log summary — new members, role/permission changes, failed logins; (3) observability summary — run volume, warn rate vs prior week, top 3 warning types. Flag anything anomalous at the top. Email it to me.
```

> ✅ You'll see: The playbook under Playbooks in the sidebar. Confirm schedule + delivery, then let Monday come to you. Optionally add a feed agent for continuous anomaly watching.

**Checkpoint before moving on:**
- [ ] You ran all three audit drills and can produce one user's full access history on demand
- [ ] You know the weekly observability triage loop and the four root-cause owners
- [ ] OpenTelemetry export is wired to your SIEM, or consciously deferred
- [ ] The Admin Weekly Review playbook exists, is scheduled, and delivers to you

## Module 6 · Governance Operations

> **Where to click** — Settings → Notifications → Ontology → enable Patch submitted (route to Slack if connected). Also watch Sync failures — a failed git sync means Ana reads stale context; treat as an incident.

**Prompt for the learner to run:**
```
List private items with access requests pending for more than a week, with each item's owner — those are stuck queues I should nudge.
```

**Prompt for the learner to run:**
```
[Departed user email] has left. List everything they own: dashboards, playbooks (note which are actively scheduled), connectors, datasets, and any pending ontology patches. Propose a reassignment plan.
```

> ✅ You'll see: A complete asset inventory with a reassignment plan — run this for every departure, real or simulated.

**Checkpoint before moving on:**
- [ ] Patch notifications route to designated reviewers; review bar applied at least once
- [ ] Access-request flow tested end to end with a test user
- [ ] You ran the offboarding prompt and have a reassignment plan
- [ ] ADMIN_RUNBOOK.md is filled in and stored where your team can find it

