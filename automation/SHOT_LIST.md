# Screenshot shot list

Drop PNGs into `assets/` with these exact filenames and they appear in the guide automatically
(no HTML edits needed — placeholders show "Screenshot coming soon" until the file exists).

Capture at ~1280-1440px browser width, light mode, demo data only — no customer data.

| # | File | Module | What to capture |
|---|---|---|---|
| 1 | `assets/au-m1-create.png` | M1 | the Playbooks page with Create Playbook / a playbook config screen |
| 2 | `assets/au-m2-fourpart.png` | M2 | a playbook prompt using the four-part structure |
| 3 | `assets/au-m3-slack.png` | M3 | a delivered report in Slack with a follow-up question answered in-thread |
| 4 | `assets/au-m4-template.png` | M4 | a template backing table and a prompt using {{variables}} |
| 5 | `assets/au-m5-agent.png` | M5 | an agent config showing triggers, connectors, instructions, outputs |

Adding more: copy a `<figure class="shot">` block in `index.html`, point it at a new
`assets/*.png`, and set `data-todo` to the capture description.
