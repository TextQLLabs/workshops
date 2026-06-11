# Screenshot shot list

Drop PNGs into `assets/` with these exact filenames and they appear in the guide automatically
(no HTML edits needed — placeholders show "Screenshot coming soon" until the file exists).

Capture at ~1280-1440px browser width, light mode, demo data only — no customer data.

| # | File | Module | What to capture |
|---|---|---|---|
| 1 | `assets/dr-m0-outputs.png` | M0 | the Dashboards area in the sidebar showing existing dashboards/reports |
| 2 | `assets/dr-m1-chart-restyle.png` | M1 | a restyled chart (top-N + Other, sorted, target line/annotations) |
| 3 | `assets/dr-m2-report.png` | M2 | a generated report with executive summary and charts |
| 4 | `assets/dr-m3-dashboard.png` | M3 | the dashboard built in this module with filters visible |
| 5 | `assets/dr-m4-share.png` | M4 | the publish/share view of a dashboard |

Adding more: copy a `<figure class="shot">` block in `index.html`, point it at a new
`assets/*.png`, and set `data-todo` to the capture description.
