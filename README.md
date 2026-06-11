# TextQL Workshops

Hands-on workshop guides for the TextQL platform — self-paced sites a customer can walk through alone, or that an FDE/AE can use to lead a guided session.

Each workshop is a **self-contained `index.html`** (plus a local `assets/` folder for screenshots) — no build step, no dependencies. Open it from a double-click, host it anywhere, or serve the whole repo via GitHub Pages.

## Catalog

| Workshop | Audience | Time | Status |
|---|---|---|---|
| [Self-Service Analytics](self-service-analytics/) | Business users — no SQL, no data background | ~2 hours | ✅ Ready (screenshots pending — see [SHOT_LIST](self-service-analytics/SHOT_LIST.md)) |
| [Dashboards & Reporting](dashboards-reporting/) | Anyone who turns analysis into something others consume | ~2 hours | ✅ Ready |
| [Automation Deep-Dive](automation/) | Analysts/operators automating recurring analysis | ~2 hours | ✅ Ready |
| [TextQL for Sales & GTM](sales-gtm/) | Sellers, sales leaders, RevOps, CS | ~90 min | ✅ Ready |
| [Connect Your Data](connect-your-data/) | Data engineers, platform admins | ~2.5 hours | ✅ Ready |
| [Ontology Operations](ontology-operations/) | Data teams running an ontology in production | ~2 hours | ✅ Ready |
| [Admin & Governance](admin-governance/) | Workspace administrators | ~2.5 hours | ✅ Ready |
| [Developer Workshop — API & MCP](developer/) | Developers and technical integrators | ~2 hours | ✅ Ready |
| Build Your Ontology, End to End | Data teams | Half day | 🔜 Planned — see [ontology-workshop-guide](https://github.com/TextQLLabs/ontology-workshop-guide) |

The repo root [`index.html`](index.html) is a catalog landing page listing all workshops.

## Using a workshop

- **Send to a customer:** share the hosted link, or the workshop folder (the HTML works offline; screenshots load from its `assets/`). Everything is account-agnostic and safe to share.
- **Run a guided session:** the facilitator shares their screen on the guide while attendees follow hands-on in their own TextQL workspace.
- Learner progress (checkpoints + progress bar) persists in the browser via localStorage.

## Adding a new workshop

1. Copy `_template/` to a new kebab-case folder (e.g. `data-team-onboarding/`).
2. Follow the `HOW TO USE THIS TEMPLATE` comment inside — set the title, a unique `WS_KEY`, and replace the placeholder `<section class="page">` blocks.
3. Add screenshots to the workshop's `assets/` and keep a `SHOT_LIST.md` (placeholders render as "coming soon" until the file exists).
4. Add a card to the catalog in the root `index.html` and a row to the table above.
5. Keep it **account-agnostic**: no customer names, no real data, synthetic examples only. Run the sanitizer before publishing if content derives from a real engagement.

The full authoring pattern (writing rules, components, structure) lives in the internal `building-workshops` skill.

## Conventions

- One folder per workshop; one self-contained `index.html` + `assets/` per workshop. The HTML is the single source of truth — no parallel markdown copies to drift.
- Customer-facing only — facilitator guides, playbooks, and account context live internally, never here.
- Non-technical voice for business-user workshops: every step gives the exact prompt to type, followed by a "You'll see" panel; modules end with outcome-based checkpoints.
