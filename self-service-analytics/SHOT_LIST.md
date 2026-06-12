# Screenshot shot list — 9 of 10 captured (2026-06-12)

✅ Captured: all except `m6-patch.png` — **held back from the public site**: the capture showed a
real prospect path (`paypal_cto_intelligence/...`, "RealView" client reporting) plus internal
emails. Recapture in a neutral demo tenant (synthetic org path, no employee emails), or have
Claude redact the existing one.

Drop PNGs into `assets/` with these exact filenames and they appear in the guide automatically
(no HTML edits needed — placeholders show "Screenshot coming soon" until the file exists).

Capture at a comfortable browser width (~1280–1440px), light mode, with any real customer
data replaced by demo data. Crop to the relevant panel; the guide renders them full-width
with a click-to-zoom lightbox.

| # | File | Page | What to capture |
|---|---|---|---|
| 1 | `assets/m0-new-thread.png` | Module 0.1 | Workspace home with the **New Thread** button visible |
| 2 | `assets/m0-attach-connector.png` | Module 0.2 | The **"+" menu open** on a new thread — Connectors and Tools visible |
| 3 | `assets/m0-sidebar-tour.png` | Module 0.3 | Full app view showing sidebar: Threads, Dashboards, Playbooks, Connectors, Ontology, Context Library |
| 4 | `assets/m0-hello.png` | Module 0.4 | Ana's answer to the "what data do I have" prompt with example questions |
| 5 | `assets/m1-first-answer.png` | Module 1.1 | A first answer with the **collapsed tool cells** visible above it |
| 6 | `assets/m2-open-the-work.png` | Module 2.1 | An **expanded tool cell** showing the SQL behind an answer |
| 7 | `assets/m3-first-chart.png` | Module 3.1 | An interactive **chart rendered in the thread** |
| 8 | `assets/m4-dashboard.png` | Module 4.3 | A generated **dashboard with filters** visible |
| 9 | `assets/m5-playbook.png` | Module 5.1 | A created **playbook** in the Playbooks list with its schedule |
| 10 | `assets/m6-patch.png` | Module 6.1 | A proposed **ontology patch / change proposal** awaiting approval |

Adding more: copy a `<figure class="shot">` block in `index.html`, point it at a new
`assets/*.png`, and set `data-todo` to the capture description.
