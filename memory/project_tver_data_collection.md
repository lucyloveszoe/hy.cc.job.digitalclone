---
name: project-tver-data-collection
description: "TVER data file collection, parsing and query project for Inari — completed 03/28/2025"
metadata:
  node_type: memory
  type: project
  originSessionId: c155a9e1-a0c1-49e4-a420-87a1f4788eeb
  modified: 2026-08-24T03:27:47.968Z
---

New active-development project (Kin Lum/Choo-Yau), target date **03/28/2025**: enable TVER (calibration) data collection from Inari via Clotho test platform transfer to the CM zDB share folder, cloud-side parsing, and query through WUDAS.

**Progress (02/04–03/04/2025)**: SOW finalized with Chun Hao (02/04); raw data format finalized (02/25); collection plan set — collect raw `.tver` files (test-plan generated) as zip, transfer via zDB Transfer as new type 56, parse into Snowflake `COREDATA_56` via the zDB Feeder approach, with dedicated tables for `Tver_Callogger_report` and `Tver_CalData_report`. SRS drafted 03/04/2025, pending stakeholder review and pending "golden format" reference files; Clotho's ability to transfer to `.tver` via `ZDBCtrl.ctrl` configuration already verified working.

**SRS bought off (03/10/2025)**: stakeholders (Chun Hao, Chung Sik) reviewed and bought off the SRS. Still pending Say Aun to prepare golden raw files before further development can proceed.

**Target date slipped to 04/04/2025, golden files received, development started (03/18/2025)**: Say Aun has now shared the golden raw files (previously the blocker); Kin Lum is in development. Target date moved from 03/28/2025 to **04/04/2025**.

**Core pipeline complete, only WUDAS query integration remains (03/25/2025)**: Kin Lum completed the zDB transfer update (`.tver` collection, post-build auto-pack/CMAC-zip, deployed to INR P13), the Snowflake `COREDATA_56` table + dynamic tables, and the zDB Feeder (incl. unit tests + IaC/Jenkins deployment) — a trial run using 5 golden files successfully collected and parsed into Snowflake. Only the WUDAS API update (to support `tver` tables) and the CLEADA detail-stitching worker/unit test are still in progress, on track for the 04/04/2025 target.

**Completed ahead of schedule (04/01/2025)**: WUDAS API updated to support `tver` tables, CLEADA API config + detail-stitching worker/unit test completed, WUDAS schema doc and core-data SDD updated, Spotfire data function done, and the project formally released — all finished by **03/28/2025**, ahead of the already-once-slipped 04/04/2025 target. Project is now **Complete**.

**Why:** a discrete new data-pipeline commitment with a hard target date, structurally similar to other zDB/WUDAS-family data-type onboarding projects.

**How to apply:** closed out — no further action expected; reference if a similar CM calibration-data onboarding request comes up as a precedent for scope/sequencing.
