---
name: project-wafer-yield-by-radius-dc2pass
description: "Enable Wafer 'Yield by Radius' Processing+WUDAS and Process DC-2PASS — the 2024 'Dynamic Wafer Radius Processing' frozen backlog item, activated 2025 and shipped complete on target 06/13/2025"
metadata:
  node_type: memory
  type: project
  originSessionId: 59aa9cac-0722-4254-85a2-83d236cbea48
  modified: 2026-09-08T05:03:09.508Z
---

Kin Lum-owned development item, target date 06/13/2025. Goal: enable radius-basis wafer yield analysis for two use cases — (1) excursion investigation needing quick turnaround on a specific radius band (e.g. CMP non-planarity check on >80mm radius, plasma4 wafer-center 5mm yield), and (2) longer-trend radius-basis yield monitoring (e.g. circuit-open trend from 93-95mm radius). The item also absorbed a separate, originally distinct request — "Enable zDB Processing on DC-2PASS file (new Process DC-2PASS)," per Kuok Liang's SOW — bundled in because both needed the same wafer-processing pipeline work. The absorbed DC-2PASS sub-request ("Enable zDB Processing on DC-2PASS file") was itself formally closed 05/20/2025 — but this only closes the file-type-enablement sub-piece; the umbrella "Yield by Radius" item continued past that date still targeting 06/13/2025. As of ww22 (06/03/2025), still "In Progress"; a Sawnics parametric-data quality issue surfaced (some Sawnics radius values >100mm, physically implausible, root cause: 2WK5 GSG/PCM XY coordinates follow the Sawnics-internal SNX FAB process convention, not WSD's standard wafer-probing coordinate convention), with three candidate resolutions under consideration: skip radius calculation entirely for Sawnics data, calculate as-is and let PE ignore the bad values, or build a Sawnics-specific dedicated radius calculation.

**Completed on target, Sawnics question resolved (06/13/2025, reported ww24 06/17)**: Choon Siang (PE) confirmed the resolution — **skip radius calculation for ALL Sawnics data** (option b) rather than building a Sawnics-specific calculation or accepting garbage-in-garbage-out values. INR/CB radius-yield was bought off by Choon Siang 06/03/2025 ahead of the final close. Marked **Complete** exactly on its 06/13/2025 target. Treat this item, and its absorbed DC-2PASS sub-request, as fully closed — any future radius-yield question for Sawnics specifically should assume no radius data is calculated/available for that CM.

**This is the 2024 "Dynamic Wafer Radius Processing" backlog item finally moving.** [[project-2024-annual-review]] flagged "Dynamic Wafer Radius Processing" as one of three New-Requests-Investigation items that appeared every week in 2024 without ever advancing (frozen/low-priority backlog). This 2025 item is that same request, now with a real owner, SOW, and target date — the freeze lasted from its 2024 "(Initiated)" appearance until at least early 2025.

**Why:** confirms that "frozen backlog" status in these weekly reports is not necessarily permanent — worth periodically re-checking dormant items rather than assuming they stay frozen indefinitely.

**How to apply:** this item shipped on time and the Sawnics question is resolved (see above) — radius-yield reporting works for all CMs except Sawnics, where it's intentionally skipped, not a bug.
