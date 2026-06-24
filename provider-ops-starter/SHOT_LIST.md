# Screenshot shot list

Drop PNGs into `assets/` with these exact filenames and they appear in the guide automatically.
Capture at ~1280-1440px, light mode, synthetic data only (the demo provider-ops set — never real patient PHI).

| # | File | Module | What to capture |
|---|---|---|---|
| 1 | `assets/po-m1-git.png` | M2 | The Git connector configured against the starter repo |
| 2 | `assets/po-m2-pr.png` | M3 | The schema-validation PR Ana opened (schema.tql diff) |

Worth adding: a specialty → service-line / DRG → MDC/weight rollup answer (M4), a governed ALOS or bed-occupancy answer with the basis + SQL shown (M5),
HIPAA small-cell suppression firing on a facility × service-line × age-band cut (M6). Copy a `<figure class="shot">` block to add slots.
