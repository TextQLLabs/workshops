---
name: Save to Ontology
description: >-
  Opinionated rules for saving, organizing, and cleaning the ontology.
  Use before making any patch. Make a patch when asked ("save this",
  "remember this", "fix up the ontology", "too many folders") — or
  proactively, when a chat produces durable knowledge worth keeping (a
  definition, a join path, a reusable query, a dead end).
---

# Save to Ontology

The ontology only stays useful if every save lands in a predictable place,
reflects reality, and can be found again by a future chat that does not
know the filename. Left to defaults, repeated "save this" requests pile up
near-duplicate folders and stale notes until nobody can navigate it. This
skill carries the opinions so the user never has to think about where a
thing goes.

The rules below are the default law — follow them on every save. They are
also meant to be localized: this skill is the single home for the layout
and conventions, so when your organization's real structure diverges
(new domains, deeper trees, different identity axes), the change is an
edit to this file and to the short root routing document (`ANA.md` or
equivalent) that carries the essentials into chats that never invoke this
skill. Create that routing document if it is missing.

## The ontology is a live snapshot

Reference knowledge describes the connectors, schemas, and business logic
**as they are now** — not as they used to be. When something changes, edit
the file to match reality and delete what is no longer true: no tombstones,
no "deprecated" / "legacy" labels, no backdated migration history inside
reference docs. This is safe because every change ships as a reviewed
patch — version history and auditability live in the patch record, not in
the documents. If your organization needs an explicit change narrative
(regulated environments often do), keep it in dated findings files, never
in reference docs.

## Optimize for future discoverability

A save is not done when the file exists — it is done when a future chat
can find it without knowing the filename.

- Put important guidance behind **routing documents** reachable from
  auto-attached context: the root routing doc, the ontology README, or a
  domain/team README. A good path is broad → narrow: root doc → domain
  README → specific file.
- Use **searchable filenames and headings** containing the terms people
  actually ask with ("customer retention", "provider cost", "usage
  connector"). Duplicating routing terms in README prose is deliberate
  and good — exact-match search is often the fastest retrieval path.
- Every folder keeps a short `README.md`: what lives here, when to use
  it, which files are canonical, and related folders.

## The canonical layout (starter — localize during onboarding)

The top level is organized by **what a thing is**, and knowledge is saved
**next to what it describes** — never in a generic docs pile.

- `databases/<source>/` — everything known about one connected data
  source: schema and table notes, join paths, metric definitions computed
  from it, reusable `.tql` queries, and known dead ends (bad joins, stale
  tables). One folder per database/connector. Most durable data knowledge
  lives here.
- Business-domain folders (e.g. `finance/`, `clinical/`, `sales/`) —
  cross-source business knowledge that no single database owns. Only a
  handful of these should ever exist.
- `roles/<role-name>/` and `teams/<team-name>/` — the identity layer
  (see below): routing READMEs, example questions, and persona context
  for a role or team. These folders **route**; they never redefine.
- `users/<person>/` — per-user personal context, one folder per person
  keyed by email local part (`users/jane.doe/context.md`).
- `skills/<name>/` — one folder per skill, holding its files, queries,
  and notes. The folder name is the skill's trigger.
- `apis/` — scripts and docs for external APIs and integrations.
- `agents/` — product-managed (machine-keyed names). Never reorganize
  or hand-edit it.

Point-in-time analyses and reports get **date-stamped files inside the
domain or database folder they are about**
(`finance/2026-07-14-q2-flux-drivers.md`) — an old analysis is never
edited to "update" it; write a new dated file.

## The identity layer: roles, teams, people

People ask questions through organizational lenses. When material
naturally belongs to a role, team, or person, make that identity visible
in the path — but identity folders are **routing, not duplication**:

- Each identity folder's README names the audience, the recurring
  questions, and links to the canonical domain/database files that answer
  them ("Start with `../../finance/revenue.tql` for bookings"). It never
  restates a metric definition.
- Placement heuristic, in order:
  - **Personal** (`users/`) — how one specific person works: preferred
    formats, default filters, recurring reports. If a colleague asking
    the same question should get the same answer, it is NOT personal.
  - **Role/team** (`roles/`, `teams/`) — how a population works: the
    questions a CFO's office asks, the voice an executive summary should
    take, which domains a team starts from.
  - **Org** (database/domain folders) — the facts themselves. Definitions,
    join paths, and queries always live here.
- If two people share an email local part, their `users/` folders use the
  full email address; a full-email folder always beats a bare one.
- Never read one person's `users/` folder to answer questions about them
  — only to apply that user's own preferences.

## Saving rules

Apply these on every save, without being asked:

1. **Search before saving.** List and search the tree first. If a file on
   the topic already exists, edit it — never create a near-duplicate, a
   copy, or a `-v2`/`-final`/`-updated` variant.
2. **Save knowledge next to what it describes.** Facts about one data
   source go in `databases/<source>/`; cross-source business facts go in
   the domain folder; role-specific routing goes in the identity folder;
   notes about an artifact go in that artifact's folder. Nothing goes in
   a generic docs pile.
3. **Reference knowledge is edited in place; findings are append-only.**
   Durable knowledge (definitions, join paths, rules) is updated in its
   existing file as the truth changes — including deleting what is now
   false. Point-in-time findings get a new dated file. Never mix the two
   in one file.
4. **Keep the tree as shallow as the organization allows.** Default to
   `type/name/file` (two levels). Larger organizations and richer domains
   may legitimately need three or four (`databases/<warehouse>/<schema>/
   queries/`, `domains/finance/semantic/`) — go deeper only when every
   level is a real axis (source, schema, artifact kind), never for
   taxonomy's own sake. If most folders at a level contain one child,
   the level is fake — flatten it.
5. **Names are lowercase and searchable**, words separated with `-` or
   `_` to match sibling folders: `revenue-recognition.md`, never
   `Revenue Recognition (final).md`. This applies to non-text assets too:
   `revenue-lineage.png`, not `diagram-final.png`.
6. **One definition per term.** A metric or business term is defined
   exactly once — in the database or domain folder that owns it. Queries,
   analyses, and identity folders reference it, never restate it. When a
   term is ambiguous (booked vs recognized revenue), that definition wins
   — and answers state which definition was used.
7. **Link, don't copy — and protect downstream references.** Connect
   files with relative links instead of restating content; one file owns
   a fact, everything else points to it. Before moving, renaming, or
   deleting ANY file: (a) fix every markdown link that pointed to it, and
   (b) check for **non-markdown consumers** — dashboards and data apps
   that reference `.tql` files by path, playbooks that cite library
   paths, auto-attach configuration, and agent prompts. A rename that
   breaks a production dashboard's data source is worse than the mess it
   cleaned up. Update those references in the same patch or leave the
   path alone.
8. **Save queries as reusable, runnable `.tql` — never one-off SQL.**
   A `.tql` file models a reusable semantic surface, not one rigid
   report. The rule of thumb: **if the same business question could be
   asked with a different entity, filter, grouping, or time window, those
   choices become typed parameters.** A question about one provider is
   saved as a query parameterized over provider — so every provider gets
   the same governed calculation. Prefer `metrics` / `dimensions` /
   `filters` parameter sets with authored options over hard-coded SELECT
   lists; document each parameter with `--` comments; split shared
   entities and joins into reusable object modules when several queries
   need them. Execute the file before saving — an untested query is a
   liability, not knowledge. Docs reference the file rather than inlining
   a copy.

   *Inline TQL primer — the mechanics:*
   - Declare typed inputs with `params { ... }`: `String`, `Bool`,
     `Date`, `Timestamp`, `Set<...>`, `List<FilterInput>`.
   - Use `Set<"revenue" | "order_count">` for the approved labels a
     caller may pick (metric names, dimension names).
   - `matchSet` maps each caller-selected label to an authored SQL
     fragment — the caller chooses a label, the `.tql` owns the SQL.
   - `filterWhere` with authored filter definitions keeps filter keys,
     column expressions, and operators explicit.
   - `concatSep`, `wrap`, and `isEmpty` assemble SELECT / WHERE /
     GROUP BY / JOIN fragments only when they're present.
   - Never hand-quote interpolations — write `country = ${country}`,
     not `country = '${country}'`.
   - Split shared entities and joins into reusable object modules
     (`objects/customer.tql`) when several queries need them.
9. **Certify the canonical (golden) files.** When a file has been tested,
   reviewed, and accepted as the source of truth, mark it certified
   (golden) through the platform's certification flow and say so in the
   owning README. When multiple files touch the same topic, the golden
   file wins — non-golden files must defer to or link into it. Only
   certify what has actually been executed/reviewed; a golden file that
   is wrong is worse than no file. Changes to a golden file deserve the
   owner's review, not a silent edit.
10. **Structural changes update this skill in the same patch.** New
    top-level folders are rare: artifact-type folders come from the
    product, and a new business domain earns a top-level folder only when
    several files across sources demand it. A patch that adds a folder or
    domain without updating the layout above (and the folder's
    `README.md`) is incomplete.

## Granularity

- One file answers one question. Split files that cover unrelated topics;
  merge files that cover the same one.
- New knowledge starts as a section in the closest existing file. It
  graduates to its own file only when it outgrows the parent or gets
  referenced on its own.
- Create a domain subfolder only once several files share the domain —
  never for a single file.
- An analysis is one file per run, not one file per finding or chart.

## More than text and TQL

The ontology can hold any artifact that helps future chats understand,
generate, or verify work: entity-relationship and lineage diagrams,
brand stylesheets for generated reports, spreadsheet and slide
generators, example outputs. Keep generated examples next to their
generator with a README explaining how to regenerate them and what
sources they assume. The same rules apply: searchable names, saved next
to what they describe, referenced not copied.

## Feedback loops

An ontology should learn from real usage. Where the platform exposes
usage data, set up a recurring playbook or agent that looks for repeated
questions, hand-written SQL, and failed retrievals — and proposes small,
reviewable ontology patches (a new metric label in an existing `.tql`, a
new filterable, a routing README entry). Keep approval explicit for
business definitions and access-related content, and route what was
learned back into the READMEs so the next chat finds it.

## Cleanup mode — "fix up the ontology"

When the user asks to clean up, reorganize, or de-duplicate, run this as
a project, not a single save:

0. **Pre-flight: inventory downstream references.** Before proposing any
   move, list every `.tql` referenced by a dashboard, data app, or
   playbook, every path cited in auto-attach configuration, and every
   path cited in agent prompts. These paths are either frozen or updated
   in the same patch that moves them — never orphaned.
1. **Audit.** Walk the entire tree. Build a table: every current folder →
   which canonical folder/domain it maps to, plus near-duplicate files,
   rule violations (unsearchable names, undated analyses, definitions
   restated in multiple places, stale reference notes, dangling links,
   one-file folders, fake depth levels), and uncertified files that are
   acting as sources of truth. For an already-organized workspace this
   step alone may be the deliverable — a conformance report is a valid
   outcome; do not manufacture churn.
2. **Propose.** Show the user the target structure and the full
   old-path → new-path mapping, including which files will be merged,
   what each merge keeps, and which downstream references will be
   updated. Plain language — they should be able to approve it without
   knowing the rules.
3. **Confirm before touching anything.** Never delete or merge content
   without explicit approval of the plan. When two files conflict, keep
   both bodies in the merged file under a "needs reconciliation" heading
   rather than silently dropping one.
4. **Execute in staged patches.** Do the work in the sandbox and persist
   it via patches — patches are the only way changes outlive this chat.
   Stage it: (a) moves/renames into the canonical layout plus the
   downstream-reference updates they force, (b) merges of duplicates,
   (c) the conventions update (step 5). Never test-skip a `.tql`: if a
   move touches a `.tql` file, execute it from the sandbox before
   patching it.
5. **Persist the conventions.** Finish by rewriting the layout section of
   this skill to match what the organization actually keeps (real domain
   names, real identity folders, real examples from their tree), and by
   updating the short root routing document — attached to all chats — so
   the essentials (the top-level map, save-next-to-what-it-describes,
   the live-snapshot rule, search-before-saving, the downstream-reference
   warning) reach chats that never invoke this skill.

## Save checklist

- Can a future chat discover this from a routing doc or search term?
- Did I search for an existing file before creating one?
- Is it next to what it describes — org fact, role routing, or personal?
- If it is a query: parameterized for the family of questions, executed,
  and saved as `.tql`?
- Does exactly one file define each term it touches?
- Did I break any dashboard, data app, playbook, or link?
- Should this file be certified golden — or defer to one that is?
