# TextQL Workshops

Hands-on, self-paced workshop guides for the TextQL platform — AWS Workshop Studio–style sites a customer can walk through alone, or that an FDE/AE can use to lead a guided session.

Each workshop is a **single self-contained `index.html`** — no build step, no dependencies. Open it from a double-click, host it anywhere, or serve the whole repo via GitHub Pages.

## Catalog

| Workshop | Audience | Time | Status |
|---|---|---|---|
| [Self-Service Analytics for Business Users](self-service-analytics/) | Business users — fully non-technical | 60–90 min | ✅ Ready |
| Build Your Ontology, End to End | Data teams | Half day | 🔜 Planned — see [ontology-workshop-guide](https://github.com/TextQLLabs/ontology-workshop-guide) |

The repo root [`index.html`](index.html) is a catalog landing page listing all workshops.

## Using a workshop

- **Send to a customer:** share the hosted link (or the `index.html` file itself — it works offline). Everything is account-agnostic and safe to share.
- **Run a guided session:** the facilitator shares their screen on the guide while attendees follow hands-on in their own TextQL workspace.
- Learner progress (checkpoint checkboxes) persists in the browser via localStorage.

## Adding a new workshop

1. Copy `_template/` to a new kebab-case folder (e.g. `data-team-onboarding/`).
2. Follow the `HOW TO USE THIS TEMPLATE` comment at the top of the content — set the title, a unique `WS_KEY`, and replace the placeholder `<section class="page">` blocks.
3. Add a card to the catalog in the root `index.html` and a row to the table above.
4. Keep it **account-agnostic**: no customer names, no real data, synthetic examples only. Run the sanitizer before publishing if content derives from a real engagement.

The full authoring pattern (writing rules, components, structure) lives in the internal `building-workshops` skill.

## Conventions

- One folder per workshop, one self-contained `index.html` per workshop.
- Customer-facing only — facilitator playbooks and account context live in separate private `*-workshop-playbook` repos, never here.
- Non-technical voice for business-user workshops: no jargon, every step says exactly what to type, every prompt has a "What you'll see" follow-up.
