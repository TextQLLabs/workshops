# Screenshot shot list

Drop PNGs into `assets/` with these exact filenames and they appear in the guide automatically
(no HTML edits needed — placeholders show "Screenshot coming soon" until the file exists).

Capture at ~1280-1440px browser width, light mode, demo data only — no customer data.

| # | File | Module | What to capture |
|---|---|---|---|
| 1 | `assets/ag-m0-console.png` | M0 | the admin settings surface / overview |
| 2 | `assets/ag-m1-sso.png` | M1 | SSO / SCIM identity settings |
| 3 | `assets/ag-m2-roles.png` | M2 | the roles & permissions configuration |
| 4 | `assets/ag-m3-connectors.png` | M3 | the connector governance / access page |
| 5 | `assets/ag-m5-audit.png` | M5 | the monitoring / observability dashboard |

Adding more: copy a `<figure class="shot">` block in `index.html`, point it at a new
`assets/*.png`, and set `data-todo` to the capture description.
