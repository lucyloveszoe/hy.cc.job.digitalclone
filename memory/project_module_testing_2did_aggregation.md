---
name: project-module-testing-2did-aggregation
description: "Proposal to switch Module Testing's C-Aggregation (consolidation) logic from SubLot-based to 2DID-based"
metadata: 
  node_type: memory
  type: project
  modified: 2026-08-25T20:31:20.021Z
  originSessionId: d76cf8c9-d472-429b-98a8-48fc2c8e3550
---

Chung Sik proposed switching Module Testing's C-Aggregation (consolidation) enhancement logic from SubLot-based aggregation to 2DID-based aggregation, citing disadvantages of SubLot-based aggregation in special scenarios. Scope: applies to zDB Type 1 (PRDA) data processing, expected to change behavior in the Lot, TopParameter, Csummary, Cstatbin, Cdetail, Cdetailslim, and RejectPareto tables. No target date set as of ww34 (08/25/2026); status Initiated, owner not yet finalized in the report.

As of 08/25/2026, Yu Han is in further clarification with Chung Sik pending stakeholder answers to an open question ("Q1"). Kin Lum's internal rough estimate for the work is under 6 weeks.

**Why:** a core-table-level change to how Module test data gets consolidated, requested by PE (Chung Sik) to fix known gaps in the current SubLot-based approach.

**How to apply:** don't assume this is committed work yet — it's pending Chung Sik's stakeholder answer and doesn't have an owner or target date. Update once scoping clarification completes.
