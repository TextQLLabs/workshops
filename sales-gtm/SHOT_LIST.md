# Screenshot shot list

Drop PNGs into `assets/` with these exact filenames and they appear in the guide automatically
(no HTML edits needed — placeholders show "Screenshot coming soon" until the file exists).

Capture at ~1280-1440px browser width, light mode, demo data only — no customer data.

| # | File | Module | What to capture |
|---|---|---|---|
| 1 | `assets/gtm-m0-crm.png` | M0 | Ana describing the connected CRM objects / translation table |
| 2 | `assets/gtm-m1-pipeline.png` | M1 | a pipeline-by-stage answer with a chart |
| 3 | `assets/gtm-m2-dossier.png` | M2 | an account dossier combining CRM history and web research |
| 4 | `assets/gtm-m3-prep.png` | M3 | a meeting prep card for an upcoming external meeting |
| 5 | `assets/gtm-m5-cadence.png` | M5 | the weekly GTM playbook or its delivered report |

Adding more: copy a `<figure class="shot">` block in `index.html`, point it at a new
`assets/*.png`, and set `data-todo` to the capture description.
