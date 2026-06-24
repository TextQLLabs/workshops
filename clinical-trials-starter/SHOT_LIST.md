# Screenshot shot list

Drop PNGs into `assets/` with these exact filenames and they appear in the guide automatically.
Capture at ~1280-1440px, light mode, demo data only (synthetic CTMS/EDC set — never real subject PII, never arm-level).

| # | File | Module | What to capture |
|---|---|---|---|
| 1 | `assets/ct-m1-git.png` | M1 | The Git connector configured against the starter repo |
| 2 | `assets/ct-m2-pr.png` | M2 | The schema-validation PR Ana opened (schema.tql diff) |

Worth adding: a CTCAE-grade / subject-status grouper answer (M3), a governed enrollment_vs_target answer with SQL (M4),
the blinding decline ("can't split by arm") firing (M5). Copy a `<figure class="shot">` block to add slots.
