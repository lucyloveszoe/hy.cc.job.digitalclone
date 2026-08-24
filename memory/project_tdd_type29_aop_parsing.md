---
name: project-tdd-type29-aop-parsing
description: "Parsing Tester TDD (Test Time Data) files as new zDB Collection Type 29, to fix WSD's slow/manual annual AOP test-time budgeting process"
metadata: 
  node_type: memory
  type: project
  originSessionId: c155a9e1-a0c1-49e4-a420-87a1f4788eeb
  modified: 2026-08-24T06:31:01.307Z
---

WSD management faces long turnaround times for annual AOP (budget) planning because detailed test-time breakdowns aren't standardized or parsed into a database — existing local test-time logs are uploaded to zDB but not parsed, forcing PEs to manually compile test-time data from a limited tester population for CAPEX/expense forecasting. TDG started standardizing the TDD (Test Time Data) format for 2026 products/test processes, targeting EVT readiness (Jan 2026).

**Why:** this is a recurring annual planning pain point (JJ/WSD management), not a one-off request — TDG's format-standardization work created the opening to finally parse this data into a queryable form (new zDB Collection Type 29) instead of PEs compiling it by hand each year.

**Scope (per Ronald's SOW)**: enable data upload/parsing of TDD test-time log data (RF1 and NFR "golden" TTD files), and expand WUDAS query access to the uploaded data — requires both ETL and a dashboard access layer.

**Timeline**: SOW shared and reviewed against TDD content with PE (Kin Lum/Ronald), updated based on Tzer Ming's input, RF1/NFR golden TTD files provided 12/02/2025; pending RF2 golden files. Coverage split: Kin Lum/Tingyu own data parsing + Cleada/WUDAS access, Choo-Yau/YK provide Spotfire data-function support if needed. Target: start development Dec 2025, complete by end of Jan 2026 (to align with TDG's EVT-readiness date). Note from JJ: FY26 AOP itself is already over, so this isn't urgent for FY26 — it's being built ahead of the next test-time-breakdown-by-platform review, expected Q3 FY26 (May-Jun 2026).

**How to apply:** update as RF2 golden files land and development starts; don't treat this as urgent-priority despite the Jan 2026 target, since JJ's own framing is "preparing ahead," not fire-fighting a current AOP cycle.

**Development started, target date set to 02/10/2026 (12/23/2025)**: Jiapeng defined the TTD parsed-file format and core DB design (Complete); now building the new feeder for TTD files (In Progress).

**Development continues (12/30/2025)**: new feeder for TTD files, pre-development work on a TTD summary table, and updating the feeder's meta output/core DB columns to match the summary-table design — all In Progress, no blockers reported.
