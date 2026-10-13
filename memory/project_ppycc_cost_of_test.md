---
name: project-ppycc-cost-of-test
description: "PPYCC Cost of Test (CoT) module — SW-team-built cost-input database/portal extending PPYCC, Phase 1 (Handler & Tester Cost Input) completed 06/07/2024"
metadata:
  node_type: memory
  type: project
  modified: 2026-09-08T01:28:17.445Z
  originSessionId: 077ee51e-a0d3-4dac-9391-fb44fc17b43c
---

A new module (owner: Choo-Yau) built on top of PPYCC (Broadcom's Projection/Planning/Yield/CRRC/Cost system — see [[project-nxg-pipeline-and-raptor]] for PPYCC's own architecture) per an SOW from Kok Kit. Goal: establish a PPYCC "Cost of Test" (CoT) input database covering test yield, test time by platform, R&M, consumables, equipment cost, and inventory — replacing manual Excel updates and standardizing the format across San Jose, Seoul, and Penang. Git repo: `wsd-broadcom/WSD.NPIPlanningPortal`. Design principle set 01/16/2024 (per KP sync): follow a "data only" style — CoT collects data into a single source of truth, but deliberately does NOT auto-generate Excel macros (GO's simulation formulas change too often to standardize); instead either offer a UI to keep formulas updated, or give GO raw data to use with their own macros. Reuses PPYCC's existing UI/user-management architecture from the earlier PPYCC Financial and Custom Sampling work.

**Actual origin ~1 month earlier than previously recorded (12/19-12/25/2023)**: Kok Kit's CoT proposal (with a Google Slides deck) and the "Objectives"/"Proposals" framing already recorded above were both present in the initial Dec 2023 WWL discussion — including the "data only, no auto-generated formulas" reasoning already attributed to a 01/16/2024 KP sync. The project was explicitly sequenced to start only after NXG WUDAS and the Spotfire RTS/Optimization/Upgrade work, which explains the ~1-month gap between this Dec 2023 kickoff conversation and the 01/16/2024 date already on file.

**Two-phase scope**: Phase 1 = Handler & Tester Cost Input database; Phase 2 = Project Input database (test time, yield, utilization, etc.) plus Test OSP/Capex summary.

**Phase 1 completed 06/07/2024** (target was 06/25/2024, finished early) — both internal review and external buy-off done. Sub-items: product/test-process/test-CM presetup, hardware presetup, supplier presetup, CM cost-per-hour charges, LTV & weekly max capacity by quarter, and hardware/cost-entry CSV upload, plus an SDD doc with Selenium unit tests. Phase 2 SOW was confirmed ready by Kok Kit around 04/15/2024, clearing the way for Phase 1 to kick off — Phase 2 itself was not yet started as of this batch (through ww25/06/19/2024).

**Why:** a genuine SW-team-owned build project distinct from PPYCC's other WSD-adjacent context (Raptor Serin-push integration, gDB backbone) already tracked elsewhere — this is WSD extending PPYCC's own cost-tracking capability, not consuming it.

**How to apply:** if Yu Han asks about PPYCC "Cost of Test," CoT, or Phase 2 (Project Input database) status, check for updates beyond ww25/06/19/2024 (Phase 2 not yet started as of that date) — this file only covers through mid-2024.

**See also (2026-10-12)**: PPYCC data reachable through gDB WS is the 15 `npip_*` tables; the CoT input DB is not in that list (verify consumption path): [[reference-ppycc-npip-gdb-subset]].
