# Admin & Governance — Ana-Led Runner (FULL)

> Full-instruction runner — every module's prompts, expected results, and checkpoints.
> Air-gapped/VPC: upload this file. Token-limited tenants: use the concise `ana-runner.md`.

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

### 0.1 · Confirm you're an admin
Open Settings from the left sidebar. As an admin you should see all of these sections; members see only a subset.

> **Note** — If a section is missing, you don't have the admin role — get it assigned before continuing. Also confirm Observability appears in the left sidebar (visible to admins by default; grantable to non-admin members via permission — Module 5).

### 0.2 · The two other admin surfaces

### 0.3 · Take a baseline snapshot

**Prompt for the learner to run:**
```
Give me an admin baseline of this organization: how many members and what roles they hold, which connectors exist and whether they are read-only or read-write, which tools are enabled org-wide, and which AI models are enabled. Format it as a table I can save.
```

> ✅ You'll see: A configuration snapshot. Save it — it's your before picture, and writing it down is the first governance habit.

### 0.4 · The mental model: four layers of control

**Checkpoint before moving on:**
- [ ] You can see all admin sections in Settings, plus Observability in the sidebar
- [ ] You know which module covers each section
- [ ] You saved a baseline snapshot of members, roles, connectors, tools, and models
- [ ] You can name the four layers of control in order

## Module 1 · Identity: SSO & SCIM

> **Note** — Do this module in a staging org first if you have one. Identity changes affect everyone's login.

### 1.1 · Why SSO first
Until SSO is on, users sign in with magic links or Google — fine for a pilot, not for governance. SSO gives you central credential policy (MFA, session length), instant deprovisioning, and a single audit point. Supported: Okta, Microsoft Entra ID (Azure AD), Ping Identity — anything OIDC-compliant.

### 1.2 · Configure OIDC

> **Where to click** — Settings → Configuration tab → Authentication section → OIDC Settings

### 1.3 · Verify SSO
Incognito → login page → your Display Name appears → authenticate via IdP → land in the workspace. Then confirm the trail:

> **Where to click** — Settings → Audit Log → filter Category = Authentication

> ✅ You'll see: "Created OIDC provider" and your test "Logged in" events, with actor, timestamp, IP, and auth method.

### 1.4 · Automate lifecycle with SCIM
SSO controls how people sign in; SCIM controls who exists . With SCIM 2.0 connected, your IdP creates, updates, and deactivates TextQL users automatically — and can push groups.

> **Where to click** — Settings → Configuration tab → SCIM Provisioning section → Create Token

### 1.5 · Test the joiner / leaver loop

**Prompt for the learner to run:**
```
From the audit log, list all member provisioning and deprovisioning events in the last 30 days. Flag any member created manually rather than via SCIM.
```

> ✅ You'll see: The lifecycle trail — and a flag on anyone bypassing the IdP, which after SCIM should be rare and deliberate.

### Troubleshooting

**Checkpoint before moving on:**
- [ ] OIDC login works for a test account, and your admin session never lapsed
- [ ] The Created OIDC provider event is in the audit log
- [ ] SCIM token created, configured in the IdP, and stored in your secrets manager
- [ ] A test joiner appeared and a test leaver was deactivated without touching TextQL

## Module 2 · Roles & Permissions

### 2.1 · The model in one minute

> **Where to click** — Settings → Roles → Manage Permissions . Assignment: Settings → Members → role dropdown. Changes apply immediately.

### 2.2 · Read the defaults before changing them
Do: open the member role's permissions and read what a default member actually gets:

**Prompt for the learner to run:**
```
Summarize the differences between the admin and member roles in this org as a table: resource, member access, admin access. Highlight anywhere the member role has write access.
```

### 2.3 · Design a custom role
The most-requested pattern from real support tickets: a viewer/analyst split and restricting who can publish connectors .

> **Note** — Custom roles also scope which connectors, context files, and models the role can access — that's how "Finance sees the finance warehouse, Sales sees the CRM mirror" is done. Connector scoping: Module 3. Model scoping: Module 4.

### 2.4 · Sharing semantics — the other half of access

**Prompt for the learner to run:**
```
User [email] says they can't see [dashboard/playbook/thread name]. Walk the four layers: do they have a role with read on that resource type, is the item public or private, is it shared with them, and for feed posts — can they read all referenced chats? Tell me the single change that grants access with least privilege.
```

> ✅ You'll see: A layer-by-layer diagnosis ending in one least-privilege change. Save this prompt — it answers most access tickets.

### 2.5 · SCIM groups → roles
If you completed Module 1: push groups from your IdP and map them to TextQL roles. New hire joins the data-analysts IdP group → lands in the right TextQL role on day one, no admin touch.

### Troubleshooting

**Checkpoint before moving on:**
- [ ] You can recite the 4-permission pattern and both design implications
- [ ] You read the member defaults and made a deliberate decision about Connector write
- [ ] You created analyst-restricted, assigned a test user, and verified their access as them
- [ ] You can explain the feed inheritance rule and debug a private-dashboard access question

## Module 3 · Connector Governance

### 3.1 · Inventory what exists

> **Where to click** — Connectors (sidebar) — note each connector's type, creator, public/private, and read-only vs read-write mode

**Prompt for the learner to run:**
```
List every connector in this org with its type, visibility (public/private), and whether it is read-only or read-write. Flag anything read-write, and anything public that connects to a production database.
```

> ✅ You'll see: Your data attack surface in one table. Read-write connectors deserve explicit justification — Ana can run INSERT/UPDATE/DDL through them.

### 3.2 · The credential decision

> **Note** — Governance principle for shared accounts: the service account's warehouse role is your ceiling — grant the narrowest role that serves the use case, never a superuser. With per-member auth, the warehouse's own grants do the row-level work.

### 3.3 · Scope connectors by role
The recurring enterprise ask: Finance sees the finance warehouse; Sales sees the CRM mirror; neither sees the other. Custom roles (Module 2) scope which connectors a role can access. Combine with: private connectors , default connector by role , and keeping member Connector permission at read so the catalog stays admin-curated.

**Prompt for the learner to run:**
```
What data sources do I have access to in this chat?
```

> ✅ You'll see: Only the scoped connectors. If they see more, check in order: role connector list, connector visibility, second role assignment.

### 3.4 · Guardrails on what queries can do
Read-only mode, org limits (Specs), and ontology-layer RLS — plus the query-path controls in 3.4b, which make the RLS layer enforceable.

### 3.4b · Close the raw-SQL back door — three dials
Row-level security lives in ontology query files; anyone who could run plain SQL on the same connection could historically go around them. Three independent, audit-logged, opt-in controls now close that path: **TQL-only per connection** (connection settings — blocks plain SQL on that connection in every surface while governed queries keep working), the **raw-sql role permission** (revocable independently of read/write), and an **org-wide raw-SQL switch**. Any one dial blocks the query; governed ontology queries are never affected.

**Prompt for the learner to run:**
```
Which of our connections hold row-level-sensitive data (PHI, PII, financials with entity restrictions)? For each, tell me whether it is marked TQL-only, and which roles currently hold the raw-sql permission. I want the list of gaps where a user could still reach that data with ad-hoc SQL.
```

> ✅ You'll see: the bypass surface, connection by connection. Target posture: sensitive connections TQL-only, raw-sql revoked from business roles, org switch per your governance board.

> ⚠️ **Prerequisite — the ontology comes first (coach this hard):** TQL-only removes the ungoverned path; it doesn't create a governed one. A connection with no ontology query files that gets locked is a connection *nobody can query*. Sequence: build the governed surfaces (Build Your Ontology) → verify coverage with the data team (Ontology Operations §2.5) → then lock. Same logic for revoking raw-sql from a role that relies on it daily.

### 3.5 · Credential lifecycle

**Prompt for the learner to run:**
```
Which dashboards and playbooks depend on the [name] connector? I'm planning to decommission it.
```

> ✅ You'll see: The dependency list — migrate or archive these, announce, then delete in a maintenance window.

### Troubleshooting

**Checkpoint before moving on:**
- [ ] Full connector inventory with mode and visibility; every read-write connector justified
- [ ] You can articulate shared vs per-member credentials for your top two connectors
- [ ] A test user in a scoped role sees only their connectors — verified in a real chat
- [ ] You know the three guardrail layers and which problem each solves

## Module 4 · Capabilities & Models

### 4.1 · Tool access control

> **Where to click** — Settings → Capabilities

### 4.2 · Model governance

> **Where to click** — Settings → Models — three tabs: Model Catalog, Role Access, Analytics

**Prompt for the learner to run:**
```
From model analytics: which models were used most in the last 30 days, by which roles, and what share of total ACU spend does each model represent? Does anything suggest the wrong default for a role?
```

### 4.3 · Limits (Specs)

> **Where to click** — Settings → Specs — read-only; changes go through TextQL support

### 4.4 · Spend awareness

> **New — Chargeback (Beta)** — Administrators can now split the organization's usage bill across internal teams : configurable bill lines, auto-updating rosters, what-if previews, and CSV export — past billing periods stay frozen and reproducible. If internal cost allocation is the blocker to onboarding more teams, this is the feature to turn on.

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

### 5.1 · The audit log

> **Where to click** — Settings → Audit Log — filters: free-text + category + action + date range (Today / 7 / 30 / 90 days)

> **Note** — Audit logs and product metrics can stream to Datadog, Splunk, Grafana, Prometheus, or S3 via OpenTelemetry (docs → Observability Export). If you run a SIEM, wire this — don't make the TextQL UI your only copy.

### 5.2 · Observability — quality monitoring

> **New — the People view** — Observability now includes a People view : active-people trends, an engagement spectrum, and access-method breakdowns, with custom date-range filtering — your adoption dashboard, next to the quality signals below.

> **Where to click** — Observability (left sidebar)

> 📌 **No longer admin-only:** Observability access can be granted to non-admin members holding the required permission — data-team leads can run the triage loop without the admin role. Grant deliberately: quality signals reveal what users ask about.

### 5.3 · Spend monitoring
Weekly review: total ACUs vs plan, top consumers, per-model split, anomalies (one user 10x-ing, a runaway playbook). Lenses from Module 4.4.

### 5.4 · Automate the routine

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

### 6.1 · Reviewing context & ontology patches
When members teach the platform ("our definition of active user is..."), it lands as a patch — a PR-style proposed change to org context. Members propose; you (or designated owners) approve. Every change is versioned and revertible.

> **Where to click** — Settings → Notifications → Ontology → enable Patch submitted (route to Slack if connected). Also watch Sync failures — a failed git sync means Ana reads stale context; treat as an incident.

### 6.2 · Access requests

**Prompt for the learner to run:**
```
List private items with access requests pending for more than a week, with each item's owner — those are stuck queues I should nudge.
```

### 6.3 · Onboarding & offboarding, end to end
Onboarding (mostly automatic): IdP group → SCIM provisions → group mapping sets role → role scopes connectors, models, defaults. Manual steps: verify the role, send the self-service workshop .

**Prompt for the learner to run:**
```
[Departed user email] has left. List everything they own: dashboards, playbooks (note which are actively scheduled), connectors, datasets, and any pending ontology patches. Propose a reassignment plan.
```

> ✅ You'll see: A complete asset inventory with a reassignment plan — run this for every departure, real or simulated.

### 6.4 · Write the runbook
Governance in one admin's head is a bus-factor of one. Use the Admin Runbook template on this workshop's Reference page : decision log, credential inventory and rotation schedule, patch review owners, on/offboarding checklists, escalation paths, and the weekly/monthly cadence. Fill it in now while Modules 1–5 are fresh.

### You're done
You now run identity through your IdP, authorization through least-privilege roles, data scope through governed connectors, behavior through capability and model controls — and you watch all of it through audit, observability, and spend monitoring, with the routine automated and the rest written down.

**Checkpoint before moving on:**
- [ ] Patch notifications route to designated reviewers; review bar applied at least once
- [ ] Access-request flow tested end to end with a test user
- [ ] You ran the offboarding prompt and have a reassignment plan
- [ ] ADMIN_RUNBOOK.md is filled in and stored where your team can find it

