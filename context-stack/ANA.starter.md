# ANA.md — Context Routing for [Your Organization]

<!-- ============================================================================
WHAT THIS FILE IS

This is the root routing file for your context library. It is loaded into EVERY
chat and tells Ana three things:

  1. The three context layers that exist (org / role / personal) and where each lives
  2. How changes to each layer happen (the governance distinction)
  3. Where to look for the right document when answering a question (routing tables)

Without this file, Ana has no concept of "personal context" or "role personas" and
will fall back to writing undifferentiated notes into the shared library. This file
is the substrate the entire Context Stack depends on.

HOW TO USE: replace every [bracketed] value with your own. Delete sections you do
not use yet (e.g. role personas) — but keep the layer definitions and the personal-
context read rule, which everything else relies on.
============================================================================ -->


## The three context layers

When a user asks a question, Ana composes context from three layers before answering:

| Layer | Scope | Lives in | Contains |
|---|---|---|---|
| **ORG** | Everyone in the org | `ANA.md` (this file) + top-level doc folders | Business rules, glossary, fiscal calendar, metric definitions, data-quality notes, shared policies/SOPs |
| **ROLE** | A team or persona | `behaviors/<persona>/org_context.md` | Allowed data surfaces, clarification behavior, response style, hard limits |
| **PERSONAL** | One individual | `users/<email-local-part>/context.md` | That person's preferences, vocabulary, saved workflows |

The layers compose: a user asking about `[revenue]` gets the **org** definition of revenue,
through their **role's** behavior, in their **personally** preferred level of detail.


## The governance distinction (the most important rule)

| Layer | How changes happen | Why |
|---|---|---|
| **Personal** | Immediately — the user's context, their call. No review. | It affects only that one user. |
| **Org and Role** | Proposed as patches, then reviewed and merged — like a pull request for business logic. | They change what everyone (or a whole team) is told; that power gets a gate. |

An individual cannot redefine a company metric by saving a personal preference, and a
team's behavior cannot drift without a reviewable change trail.


<!-- ============================================================================
PERSONAL CONTEXT — the read rule that makes personal context real.
Keep this section even if you delete everything else.
============================================================================ -->

## Personal context — load at the start of every conversation

**Before answering, read the current user's personal context:**

1. Read the user's email (e.g. from the session/user info).
2. Derive the directory: `email.split('@')[0]` (e.g. `jane.doe@acme.com` -> `users/jane.doe/`).
3. Read `users/<local-part>/context.md`.
   - **If it exists:** apply the preferences silently (tone, detail level, default time
     windows, vocabulary, saved workflows). Do not announce that you loaded it.
   - **If it does not exist:** note it internally and, after the user's first task,
     offer to create one.

**Read ONLY the active user's own personal file** to apply their preferences. Never read
another person's `users/<...>/context.md` to answer a question *about* them.

**Writing personal context is immediate** — when a user says "remember this" / "save this",
write it to `users/<their-local-part>/context.md` directly. No patch, no review. Confirm
the exact file path you wrote to.

**What goes in personal context (things true about the USER):**
- Communication style, preferred detail level, format ("headline number first")
- Timezone and default time windows ("default to [the current quarter] unless I say otherwise")
- Personal vocabulary / shorthand ("'my accounts' = [accounts I own]")
- Saved multi-step workflows ("'run my Monday prep' = [the ritual]")

**What NEVER goes in personal context (things true about the BUSINESS):**
- Metric or term definitions -> **Org** (a definition in personal context forks the truth)
- "Our team never shows SQL to stakeholders" -> **Role** (a persona behavior)
- "The `[X]` table is deprecated; use `[Y]`" -> **Org** (everyone querying it needs to know)

The test: *if a colleague asked the same question, should they get the same treatment?*
If yes, it belongs in the org or role layer, via the reviewed path — not in personal context.


<!-- ============================================================================
ROLE PERSONAS — delete this section until you have authored personas.
============================================================================ -->

## Role personas (behavioral context)

Each persona is a file at `behaviors/<persona>/org_context.md` with four sections:
**1. Allowed data surfaces · 2. Clarification behavior · 3. Response style · 4. Hard limits.**

Personas are **provisioned by admins** (mapped to roles / identity groups) — users do not
choose their own. Persona files change only through reviewed patches; widening an allowlist
is an access decision and should be reviewed by the relevant data owner.

| Persona | File | Summary |
|---|---|---|
| `[analyst]` | `behaviors/[analyst]/org_context.md` | `[broad scope, shows SQL, assumption-transparent]` |
| `[program_manager]` | `behaviors/[program_manager]/org_context.md` | `[restricted surfaces, plain language, declines off-scope with a redirect]` |
| `[executive]` | `behaviors/[executive]/org_context.md` | `[briefing tone, governed surfaces only, no SQL]` |

Behavioral context is the *semantic* layer of access control — it shapes which questions
get asked and how answers are framed. It does NOT replace data-layer enforcement (row-level
security, connector scope) or platform RBAC, which sit below it. Author persona files
assuming those enforcement layers exist.


<!-- ============================================================================
ORG ROUTING TABLES — point Ana at the right doc for a given question.
Add a row whenever you add a definition, policy, or data-quality note.
============================================================================ -->

## Org routing — where the answer lives

For any data or business question, find the trigger below and read the linked doc
BEFORE writing SQL or making a high-confidence claim. Do not guess at definitions.

| Trigger / question | Go to |
|---|---|
| "What does `[active member / revenue / churn]` mean?" | `[definitions/<term>.md]` |
| "What's our fiscal calendar?" / "What quarter is `[date]` in?" | `[definitions/fiscal_calendar.md]` |
| Glossary / acronym lookup (`[PMPM]`, `[panel]`, ...) | `[glossary.md]` |
| "Which table is the source of truth for `[domain]`?" | `[data_quality.md]` |
| `[Policy / SOP / onboarding question]` | `[policies/<doc>.md]` |
| `[A connector-specific quirk: "staging tables in [warehouse] are unreliable"]` | `[databases/<connector>/README.md]` (or connector-scoped context) |

> Connector-scoped context (loaded only when querying a specific source) is also
> supported for source-specific quirks. List those under each connector's folder.


## Library layout

| Path | Purpose |
|---|---|
| `ANA.md` | This file — root routing brain (org layer + conventions) |
| `users/<email-local-part>/context.md` | Personal context, one per user (immediate, unreviewed) |
| `behaviors/<persona>/org_context.md` | Role personas (reviewed) |
| `[definitions/]`, `[glossary.md]`, `[policies/]`, `[databases/]` | Org-wide docs (reviewed) |


## Verify the substrate is loaded

Two probes to confirm this file is active in a workspace:

1. Ask: *"What context conventions are loaded in this workspace?"*
   Ana should describe the three layers and the `users/` and `behaviors/` paths.
2. Ask: *"Save a test preference to my personal context and tell me the exact file path you wrote to."*
   Ana should name `users/<your-email-local-part>/context.md` — not a generic shared file.
   If it names a generic path or says it can only write to the shared library, this file
   is not loaded.
