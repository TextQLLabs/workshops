---
name: Building Data Apps
description: >-
  The static-first method for TextQL data apps on large warehouse tables — apps
  that open in seconds, filter instantly, and reconcile to the number. Use when
  building or fixing a data app, when an app is slow or times out on filters,
  or when duplicating a Power BI (.pbit) / Tableau (.twb) dashboard as a data app.
---

# Building Data Apps

Rules Ana applies whenever building, updating, or debugging a data app in this
workspace. The companion hands-on workshop: Build Data Apps with Ana
(https://textqllabs.github.io/workshops/data-apps/), Modules 5–6.

## The failure mode to design against — the "scan storm"

A naive app re-queries the warehouse on every interaction: one filter click fans
out into many full scans of a large fact table joined to several dimensions, and
nothing caches because filter combinations never repeat. The browser hangs. The
bottleneck is the scans, not the payload — raising a payload or row limit does
not help, and parallelizing the scans multiplies warehouse cost.

## Default architecture — static-first

1. **Pre-aggregate once, at refresh.** Push the GROUP BY into the warehouse a
   single time and materialize a small in-memory snapshot: a base metrics cube +
   breakdown tables + supporting series.
2. **Filter in the browser.** Every filter, sort, tab, and chart interaction runs
   against that snapshot — zero warehouse queries on routine use.
3. **Instant filters.** No "Apply" button, no full-screen spinner. Show the
   snapshot timestamp; if a refresh fails, keep serving the last good snapshot.
4. **Reserve live queries for genuinely un-cacheable detail** (distinct counts
   under a custom filter): one bounded, debounced query — never a fan-out.
5. **Tune the client side.** Update charts in place instead of rebuilding them;
   one optimized pass over the snapshot in the filter engine. Target: cached
   interactions land well within ~250 ms.

## Which serving strategy

- Aggregate fits in a shippable snapshot → ship it, filter in-browser (default).
- Aggregate too big to ship but far smaller than the raw fact → build a
  pre-aggregated serving table in the warehouse; the app queries that, never the
  raw fact.
- Genuinely on-demand detail → one bounded, debounced live query.
- Never: re-scan the raw fact per interaction, parallelize many full scans, or
  treat a payload limit as the fix.

## Correctness rules

- **De-duplicate every dimension before joining.** A dimension with more than one
  row per key silently doubles filtered totals — and the default view often skips
  the offending join, so it looks fine until someone filters.
- **Reconcile to the number before handing off:** the default view must reproduce
  the trusted figures exactly, and one spot-checked filtered slice must match a
  fresh warehouse query to the dollar.
- **Verify a filtered slice, never just the landing screen.**
- **State the tradeoffs — no silent caps.** If breakdown tiles slice only a subset
  of dimensions, or distinct counts use the single bounded query, say so in the
  app's notes.

## Duplicating a Power BI (.pbit) or Tableau (.twb) dashboard

1. Two paths in: upload the source file, or read a connected Power BI / Tableau
   source directly. Either way, attach 2–3 screenshots of the original — the file
   gives the logic, the screenshots give the look.
2. Map every measure to a governed ontology metric; flag any that don't map
   cleanly rather than re-implementing calculations ad hoc.
3. Build the static-first snapshot for the visuals actually shown, replicate the
   layout, then reconcile figure-for-figure against the source dashboard.
4. Keep the original tool running alongside until the numbers reconcile and the
   team trusts the app.

## Governance

Data apps run under each viewer's own permissions. On connections restricted to
governed queries (TQL-only), app data sources must be governed ontology query
files — build on governed surfaces from the start so a later lockdown never
breaks the app.
