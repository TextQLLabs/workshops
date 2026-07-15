# Context Stack — Ana-Led Runner (FULL)

> The **full-instruction** version of this runner — every module's prompts, expected results, and checkpoints.
> Use this in tenants **without tight token limits** (or air-gapped/VPC: upload this file directly).
> Token-limited environment (e.g., Snowflake Cortex inference)? use the concise `ana-runner.md` instead.
> Facilitation is identical: **interactive — the learner runs each prompt, Ana coaches, one module at a time.**

## Step-0 prompt

```
Hey Ana — facilitate the "Context Stack" workshop with me in this thread, on the data connected here.
Pull the steps from https://textqllabs.github.io/workshops/context-stack/ (or use the module list below).
Run it interactively: look at what's connected first (2–3 lines), then go ONE module at a time. For each
module, DON'T run the prompt yourself — give ME the prompt to copy and run, tell me what to look for, then
wait and coach me on my result. Start with what you see, then Module 0.
```

## Module 0 · The Context Stack

### 0.1 · Three layers, one answer
When a user asks Ana a question, she composes context from three layers before answering:

### 0.2 · The governance distinction — learn it now, hear it repeated
The single most important operational fact about the stack:

### 0.3 · The routing table
For any "I want Ana to..." request, the layer is determined by who should be affected :

**Checkpoint before moving on:**
- [ ] You can name the three layers and what kind of knowledge belongs in each
- [ ] You can state the governance distinction in one sentence: personal is immediate, org/role is reviewed
- [ ] Given five "I want Ana to..." requests from your own org, you routed each to the right layer

## Module 1 · Prepare the Environment

### 1.1 · Why this comes first
The three-layer stack only works if a root-level routing file — by convention ANA.md — exists in the context library and is loaded into every chat. That file is what teaches Ana the conventions the other modules assume:

> **The most common setup failure** — Without ANA.md , Ana has no concept of "personal context." Asked to "remember my preferences," she falls back to writing an undifferentiated note into the shared library — visible to everyone, keyed to no one. If that happens, the routing setup isn't loaded. You're doing this module so it never does.

### 1.2 · The folder layout you're standing up
Here's the shape you'll end up with — just folders and files, versioned and reviewable like any repo. You won't hand-build it; Ana creates it in the next step. This is the map:

### 1.3 · Have Ana set up ANA.md — conversationally
Don't hand-author the routing file. Hand Ana the starter below and let her do what the whole workshop is about: she'll ask about anything in [brackets] she needs from you, then write ANA.md into the context library herself. Copy this whole block — the instruction plus the starter — and paste it into a new thread:

**Prompt for the learner to run:**
```
Help me set up context routing for this workspace. Below is a starter ANA.md. First, ask me about anything in [brackets] you need filled in — our organization name, fiscal calendar, the metrics and terms worth governing, and the teams or personas we have. Then create ANA.md at the root of our context library with my answers in place, and stub the users/ and behaviors/ folders. Here's the starter:

# ANA.md — Context Routing for [Your Organization]

## The three context layers
- ORG — everyone: this file + top-level doc folders (business rules, glossary, fiscal calendar, metric definitions).
- ROLE — a team/persona: behaviors/<persona>/org_context.md (allowed surfaces, clarification, response style, hard limits).
- PERSONAL — one individual: users/<email-local-part>/context.md (preferences, vocabulary, saved workflows).
The layers compose: a user gets the ORG definition, through their ROLE's behavior, in their PERSONALLY preferred detail.

## Governance distinction (the most important rule)
- PERSONAL changes are immediate — the user's call, no review. It affects only them.
- ORG and ROLE changes are proposed as patches, then reviewed and merged — like a pull request for business logic.

## Personal context — load at the start of every conversation
Before answering, read the current user's personal context:
1. Read the user's email; derive the directory as email.split('@')[0] (e.g. jane.doe@acme.com -> users/jane.doe/).
2. Read users/<local-part>/context.md. If it exists, apply the preferences silently. If not, offer to create one after the first task.
Read ONLY the active user's own file. Writing personal context is immediate — on "remember this," write to users/<their-local-part>/context.md and confirm the exact path. Never put business definitions here (those are ORG, via review).

## Role personas (load when a persona is active)
Each persona is behaviors/<persona>/org_context.md with four sections: 1. Allowed data surfaces · 2. Clarification behavior · 3. Response style · 4. Hard limits. Personas are provisioned by admins; persona files change only through reviewed patches.

## Org routing — where the answer lives (stub — add a row per definition)
| Trigger / question | Go to |
|---|---|
| "What does [active member / revenue / churn] mean?" | [definitions/<term>.md] |
| "What's our fiscal calendar?" | [definitions/fiscal_calendar.md] |
| Glossary / acronym lookup | [glossary.md] |
| "Which table is source of truth for [domain]?" | [data_quality.md] |
```

> ✅ You'll see: Ana walk the [bracketed] values with you — org name, fiscal calendar, the metrics worth governing, your teams — then write ANA.md to the library root and confirm the path, creating users/ and behaviors/ as she goes. ( ANA.md is an org-layer file, so if your governance requires it, route this first write through review — the same distinction from Module 0; either way you end with a real routing file, authored by asking.)

> **Prefer to author it by hand?** — You can also paste the starter straight into a new ANA.md in the file editor and fill the brackets yourself. Either path lands the same file. The fully-commented version — with the role-persona routing table and the access-control framing spelled out — is in the workshop repo: context-stack/ANA.starter.md .

### 1.4 · Connectors & tools — what's required vs. optional

> **You can do this whole workshop with zero connectors** — Typed preferences, personas, and definitions need only the writable library. Connectors matter only for the optional flourish of having Ana enrich a personal profile from real data (e.g., "look at my recent threads and propose my default time window"). If you have no connectors, skip those asides — every checkpoint still passes.

### 1.5 · Verify the routing setup is loaded
Two probes. Run them in a fresh thread after seeding ANA.md — don't move on until both pass.

**Prompt for the learner to run:**
```
What context conventions are loaded in this workspace?
```

> ✅ You'll see: Ana describe the three layers and name the users/ and behaviors/ paths. If she can't, ANA.md isn't being loaded into the chat — recheck step 1.3.

**Prompt for the learner to run:**
```
Save a test preference to my personal context and tell me the exact file path you wrote it to.
```

> ✅ You'll see: Ana name users/<your-email-local-part>/context.md — your own per-user file. If she names a generic or shared path, or says she can only write to the shared library, the routing setup isn't loaded — go back to step 1.3 and confirm ANA.md is at the library root and attached to the thread.

**Checkpoint before moving on:**
- [ ] ANA.md exists at the context-library root with your [org] values filled in
- [ ] The users/ and behaviors/ folders exist (even if empty)
- [ ] Probe 1: Ana described the three layers and the per-user / per-persona paths
- [ ] Probe 2: Ana wrote a test preference to users/<your-email>/context.md — not a generic shared file

## Module 2 · Personal Context

> **If a save lands in the wrong place** — If Ana says she can only save to the shared library, or writes to a generic path instead of users/<your-email>/context.md , the routing file from Module 1 isn't loaded. Stop and fix the routing setup — the ANA.md file from Module 1 isn't loaded — before continuing; every later module depends on it.

### 2.1 · Preferences

**Prompt for the learner to run:**
```
Remember my preferences: I want concise answers with the headline number first; I work in [timezone]; default time windows to [the current quarter] unless I say otherwise; when I say "my accounts" I mean [accounts where I'm the owner].
```

> ✅ You'll see: Ana confirm what she's saved to your personal context. This is the immediate layer — no review, no patch, effective now. Note what you just did not change: nothing about how Ana behaves for anyone else.

### 2.2 · Personal vocabulary

**Prompt for the learner to run:**
```
Personal vocabulary: "the weekly" means [my Monday metrics review]; "the big three" means [my three largest accounts: names]; "EOQ crunch" means the last two weeks of a fiscal quarter.
```

> ✅ You'll see: your shorthand saved. From now on, "how did the big three do during EOQ crunch?" just works — for you.

### 2.3 · A saved personal workflow
The highest-leverage personal item — a multi-step ritual reduced to one phrase:

**Prompt for the learner to run:**
```
Save this as my "Monday prep" workflow: (1) pull [metric] for last week vs the prior week, (2) list [my accounts/deals] with no activity in 14+ days, (3) check [the team dashboard] for anything red, (4) format it all as a short brief I can read in two minutes. When I say "run my Monday prep," do all of it.
```

> ✅ You'll see: the workflow recorded. Next Monday: two words instead of twenty minutes.

### 2.4 · The persistence test
Saved context is only real if it survives the thread. Verify: start a NEW thread (context selections load fresh per thread), then ask:

**Prompt for the learner to run:**
```
What do you know about my preferences and vocabulary?
```

> ✅ You'll see: your items come back. Then:

**Prompt for the learner to run:**
```
Run my Monday prep.
```

> ✅ You'll see: the personal layer carrying across threads. If something didn't persist, it was answered conversationally but never saved — re-ask with an explicit "remember/save this."

### 2.5 · The hygiene rule — what NEVER goes in personal context
Personal context is for things that are true about you . The moment an item is true about the business , it belongs upstairs:

**Checkpoint before moving on:**
- [ ] Preferences, vocabulary, and one workflow are saved to your personal context
- [ ] A NEW thread recalled all three without re-explanation
- [ ] "Run my [workflow]" executed the full ritual from the saved definition
- [ ] You applied the hygiene test to your own saved items and moved (or flagged) anything that's actually business truth

## Module 3 · Org Context

### 3.1 · What lives at the org layer
Org context applies to every user, every thread : business rules ("revenue is recognized at shipment, not order"), the fiscal calendar, the glossary, data-quality knowledge ("[table] is the source of truth, not [other table]"), and the context library's shared documents (policies, metric docs, onboarding guides). It's the layer that makes answers consistent — the same question from two people routes through the same dictionary.

### 3.2 · Propose a definition patch
Pick a term your org actually argues about:

**Prompt for the learner to run:**
```
Propose an org context patch: our official definition of [active member] is [logged in AND completed at least one action in the trailing 30 days, excluding internal and test accounts]. Save it to the org context so every user gets this definition — and open it for review.
```

> ✅ You'll see: a patch created, not an immediate change — the governance distinction from Module 0 in action. The patch shows the proposed diff; a reviewer (an admin or designated owner) approves or declines it. Until approval, nothing changes for anyone.

### 3.3 · Propose a glossary addition

**Prompt for the learner to run:**
```
Propose a glossary patch with three entries: "[PMPM]" means per-member-per-month; "[panel]" means the set of members assigned to a care team; "[the morning report]" refers to the daily operations playbook output. Open for review.
```

> ✅ You'll see: vocabulary becoming shared infrastructure. After approval, anyone can say "PMPM by panel" and be understood — including new hires on day one.

### 3.4 · Contribute a document to the context library
Definitions are atoms; documents are molecules. Upload or point at a real document — a metrics policy, an SOP, a data dictionary:

**Prompt for the learner to run:**
```
Add this document to the org context library: [attach or name it]. Summarize what knowledge it contributes, propose where it should live in the library structure, and open the addition for review.
```

> ✅ You'll see: the document staged with a placement proposal. The context library is browsable by your colleagues and readable by Ana — one artifact serving both audiences.

### 3.5 · The review loop, from the reviewer's side
If you hold review rights, open the pending patches (the Reviews surface in the Ontology/context area) and apply the three-question bar from the Admin & Governance workshop: Correct? Conflicting with an existing definition? Scoped right (org-wide truth vs one team's shorthand)? Approve, or decline with a reason — declines without reasons teach proposers to stop proposing.

> **Note** — Reviewer notifications (patch submitted/approved/denied) are configurable under Settings → Notifications → Ontology — set them up now if you're a reviewer, or this loop silently stalls.

> **Write for the search box** — Future threads find org context by searching , not browsing. So: searchable headings and filenames, a short README in each folder saying what lives there and which files are canonical, and — deliberately — the phrases people will actually ask for repeated in the prose (metric names, synonyms, team and role names). Exact-match search is often a future agent's fastest retrieval path.

**Checkpoint before moving on:**
- [ ] A definition patch, a glossary patch, and a document addition are proposed (not directly applied)
- [ ] You watched a patch go through review — approved or declined-with-reason
- [ ] You can explain why the same question from two users now returns the same definition
- [ ] Reviewers have notifications routed somewhere they actually look

## Module 4 · Author Role Profiles

### 4.1 · The model: behavior is files
From the Role-Based Access chapter of Build Your Ontology, End to End : the ontology is files, and the behavioral context is also files — a behaviors/ folder where each persona is a directory with an org_context.md :

### 4.2 · Persona one: the restricted business persona
A [program manager in care management / clinical operations] — domain-scoped, non-technical, in a setting where seeing the wrong data is a compliance event, not an inconvenience:

### 4.3 · Persona two: the analyst
Same organization, same ontology — opposite contract:

### 4.4 · Author them for real
Create the two folders and files in your context/ontology repo (in-app editor or git, per your setup — Methods in the Build Your Ontology workshop), adapting the brackets to your domains. Then submit both as patches for review — persona files are role-layer context; they take the governed path, always.

**Prompt for the learner to run:**
```
Create behaviors/[program_manager]/org_context.md and behaviors/[analyst]/org_context.md with the two persona files above, brackets adapted to our domains, and open both as patches for review — do not apply them directly.
```

> ✅ You'll see: Two pending patches in Reviews — the personas exist only after approval, like any role-layer change.

### 4.4 · Give every persona file a routing header
Beyond the four behavior sections, open each persona file with a short routing header so future chats (and new admins) can find and use it:

**Prompt for the learner to run:**
```
Add a routing header to the top of each persona file: (1) Search terms - the phrases this role actually uses, including synonyms; (2) Audience - who this persona serves and who reviews it; (3) Start with - links to the canonical org definitions this role uses most; (4) one line stating what this file does NOT do. For example: "Do not redefine [core metric] here - this file routes [role] questions to the canonical org definition."
```

> **Route, don't redefine** — A persona file routes a role's questions to the one governed definition — it never copies or redefines it. The moment a metric is restated inside a persona, you have a fork waiting to disagree. Scope, voice, and vocabulary live here; truth lives at the org layer .

**Checkpoint before moving on:**
- [ ] Both persona files exist with all four sections, submitted as reviewed patches
- [ ] The restricted persona's clarification menu draws only from its allowlist
- [ ] Its hard-limits section declines with a redirect and without enumerating what's hidden
- [ ] The analyst persona flags ad-hoc work and never treats breadth as an access bypass
- [ ] Each persona file opens with a routing header (search terms · audience · start-with links · what it does NOT do)

## Module 5 · The Same-Question Test

> **Note** — Setup: your two Module 4 personas approved and provisioned to two test identities (or two sessions an admin can switch — Module 6 covers assignment mechanics). Have both sessions open side by side.

### 5.1 · The side-by-side
Ask the identical broad question in both sessions:

**Prompt for the learner to run:**
```
How are our [members/patients] doing on [hospital utilization] and cost?
```

> ✅ You'll see: Ana recognizes the breadth and clarifies with a menu of allowed options only ("through care gaps, utilization, or outreach — which would help most?"). After you pick: plain language, summary first, no SQL anywhere , sources described in business terms.

> ✅ You'll see: Ana proceeds with stated assumptions ("assuming utilization means admissions + ED visits over the trailing 12 months..."), answers in full technical detail, shows the SQL with a clause-by-clause explanation, and flags anything computed beyond the governed surfaces.

### 5.2 · The off-scope decline — and the file behind it
In the restricted session, ask for something real but outside the allowlist:

**Prompt for the learner to run:**
```
Show me the [unit-cost breakdown by provider contract / financial detail] for last quarter.
```

> ✅ You'll see: a warm decline with a redirect — "that falls outside this workspace; contact [the analytics team]. Within this workspace I can help with..." — and no hint of what other surfaces exist.

### 5.3 · Change behavior through governance
The program-management team now legitimately needs [readmission detail]. Wrong move: someone edits the file directly and behavior drifts. Right move:

> ✅ You'll see: behavior change with a paper trail — who requested it, who approved it, when , and exactly what changed. Compare that against the alternative ("an admin toggled something in a console six months ago, nobody remembers why") and you have the whole argument for context-as-files.

### 5.4 · Write the test down
The same-question test isn't just a demo — it's your regression suite for personas. Record the three probes (the broad question, the off-scope request, the newly-allowed request) and their expected behaviors; re-run them whenever a persona file changes. Module 7 makes this a quarterly habit.

**Checkpoint before moving on:**
- [ ] The same broad question produced clarify-vs-proceed, no-SQL-vs-SQL, summary-vs-assumptions — captured side by side
- [ ] The off-scope decline matched section 4 of the persona file, word for behavior
- [ ] The allowlist change went through propose → review → approve, and the re-ask confirmed it
- [ ] The three-probe test is written down as the persona's regression suite

## Module 6 · Provisioning & Identity

### 6.1 · Users don't choose their persona
The rule that makes personas meaningful: behavioral context is provisioned at the org/session level by an admin — end users don't select their own. The same way a database role is granted, not chosen. A persona a user could swap off is a suggestion, not a control.

### 6.2 · Map personas to identity groups
Hand-assigning roles works at ten users and fails at a hundred. The scaled pattern ties personas to your identity provider :

### 6.3 · The semantic layer of RBAC
Where behavioral context sits in the stack — the framing worth memorizing:

### 6.4 · Fail closed
The default posture for restricted personas, in both design and provisioning:

**Checkpoint before moving on:**
- [ ] Personas in your org are admin-provisioned — you verified a test user cannot swap their own
- [ ] The IdP-group → role → persona chain is mapped (or planned) for at least your two Module 4 personas
- [ ] You can draw the three-layer RBAC table from memory and say what each layer catches that the others miss
- [ ] Restricted personas fail closed: allowlist-based files, restrictive provisioning default, asymmetric review

## Module 7 · Operate the Stack

### 7.1 · Persona files are governed artifacts
Everything the Ontology Operations workshop establishes for ontology files applies to behaviors/ verbatim: version control (in-app history or git sync), changes via reviewed patches, attributable diffs, revertibility. Operational specifics for personas:

### 7.2 · New persona, or extend an existing one?
The boundary rule: one persona per team, use case, or compliance boundary.

### 7.3 · Audit: who changed what
The questions an auditor (or an incident) will ask, and where the answers live:

### 7.4 · The quarterly persona review
Thirty minutes per quarter, per persona, with the domain owner:

### 7.5 · The failure modes table

### 🎓 You're done
You can route any behavior request to its layer, build personal context that persists, curate org truth through review, author personas from the four-section anatomy, prove them with the same-question test, provision them through identity fail-closed, and operate the whole stack with the governance it deserves.

### 7.6 · The usage-driven gap review
The quarterly review catches drift; a recurring usage review catches gaps while they're small. Mine real activity for the personas' blind spots:

**Prompt for the learner to run:**
```
Create a playbook "Weekly Context Gap Review" that runs Monday mornings: review the last 14 days of usage - repeated questions, mid-thread corrections, failed context lookups, and questions users rephrased until they worked. Group them by business concept, flag concepts hit by multiple users or roles, and propose small reviewable patches: a persona-file addition, an org glossary entry, or a routing-README update. Draft the patches for review - do not apply them - and list unresolved gaps as work items with suggested owners.
```

> ✅ You'll see: a standing loop where Ana drafts, named owners approve, learnings land back in the routing docs — and context gaps become tracked work items instead of buried chat history. (Same pattern as Ontology Operations Module 4.)

**Checkpoint before moving on:**
- [ ] Persona changes route through review, with domain owners on scope changes, and the probe suite runs after every patch
- [ ] You applied the new-vs-extend rule to a real pending request
- [ ] Every audit-table row has a live answer in your setup
- [ ] The quarterly review is calendared with named owners — and the failure-modes table is in your team's runbook
- [ ] The weekly usage-driven gap review playbook exists — Ana drafts, owners approve, gaps become work items

