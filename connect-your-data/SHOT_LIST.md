# Screenshot shot list

Drop PNGs into `assets/` with these exact filenames and they appear in the guide automatically
(no HTML edits needed — placeholders show "Screenshot coming soon" until the file exists).

Capture at ~1280-1440px browser width, light mode, demo data only — no customer data.

| # | File | Module | What to capture |
|---|---|---|---|
| 1 | `assets/cyd-m1-credentials.png` | M1 | a connector credential form (showing secret handling) |
| 2 | `assets/cyd-m2-warehouse.png` | M2 | a warehouse connector setup form (e.g. Snowflake/BigQuery/Redshift) |
| 3 | `assets/cyd-m3-private.png` | M3 | the private-network connection options |
| 4 | `assets/cyd-m4-bi.png` | M4 | a BI tool connector page |
| 5 | `assets/cyd-m6-validate.png` | M6 | a validation thread (Ana confirming schema, rowcounts, freshness) |

Adding more: copy a `<figure class="shot">` block in `index.html`, point it at a new
`assets/*.png`, and set `data-todo` to the capture description.
