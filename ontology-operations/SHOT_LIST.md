# Screenshot shot list

Drop PNGs into `assets/` with these exact filenames and they appear in the guide automatically
(no HTML edits needed — placeholders show "Screenshot coming soon" until the file exists).

Capture at ~1280-1440px browser width, light mode, demo data only — no customer data.

| # | File | Module | What to capture |
|---|---|---|---|
| 1 | `assets/oo-m1-git.png` | M1 | the ontology git sync / repo connection settings |
| 2 | `assets/oo-m2-access.png` | M2 | folder-level access control configuration |
| 3 | `assets/oo-m3-golden.png` | M3 | golden dataset / accuracy test results |
| 4 | `assets/oo-m4-patch.png` | M4 | a proposed ontology patch (diff) under review |
| 5 | `assets/oo-m5-scale.png` | M5 | the ontology graph or structure spanning multiple teams |

Adding more: copy a `<figure class="shot">` block in `index.html`, point it at a new
`assets/*.png`, and set `data-todo` to the capture description.
