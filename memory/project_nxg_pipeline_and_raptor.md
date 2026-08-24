---
name: project-nxg-pipeline-and-raptor
description: NXG WSD Cloud Data Pipeline simplification effort and the Raptor data integration decision
metadata: 
  node_type: memory
  type: project
  originSessionId: b57dbea7-e0ff-4ed2-a55d-4625560a26cf
  modified: 2026-08-24T06:29:26.980Z
---

**NXG WSD Cloud Data Pipeline**: acknowledged technical debt — a 5-year-old, 5+ layer legacy pipeline stack. Goal is to simplify to 3 layers via IaC/containerization. On hold, gated on the Snowflake/GenQS/FeederDB migration completing first.

**Raptor data integration**: confirmed Snowflake only holds metadata (not raw data) for this source, so a direct Snowflake pipeline/API integration was ruled infeasible. Decision: integrate via Raptor's "Serin push" method instead. SOW review with Raptor completed 04/07/2026; Serin onboarding (secret key exchange, server config) completed by 04/07/2026; production upload path completed 04/09/2026. The 2025 HB PAD trial ran late April/early May 2026.

**Origin traced to "Raptor Data Project Support" (12/16/2025)**: YK/Choo-Yau began investigating Serin tools specifically to transfer 2DID and LVM Yield (buy-off) data to Raptor — this is the earliest recorded step toward the Serin-push integration above, a full year before the 2026 SOW/onboarding work. Early Q&A already established the architecture that the 2026 work executed on: WSD's databases (zDB, gDB, plus smaller R&D/NPI/admin DBs) are hosted in a fully secured AWS Singapore Private VPC, accessible only from the BRCM network; PPYCC (Projection/Planning/Yield/CRRC/Cost) is deployed at the BRCM SG data center with its backbone in gDB. WSD can support pushing to Raptor's SFTP, letting Raptor pull from BRCM's SFTP, or publishing a whitelisted Web API for Raptor's Serin clients — **BRCM's stated preference is "pushing to Raptor SFTP,"** consistent with the eventual "Serin push" decision recorded above.

**2DID data landscape mapped (12/23/2025)**: WSD's internal 2DID-relevant processing spans three tracks — (I) **Type 58**, created by the Tape & Reel vendor, already collected/parsed into zDB per Kok Kit's team's request; (II) **raw result files** from RF1/NF/RF2/AOI/TOP/BSI processes, each mapping to one Snowflake row with a full 2DID list per file — Snowflake can search "flattened" JSON directly (unlike Mongo), so a specific-2DID lookup across all processes is technically possible, but **no full-lifecycle (PCB/Wafer/Lot) 2DID+ReelID stitching solution exists** because no one has requested it yet; (III) **gDB**, which collects genealogy/process/yield/failure reports from all CMs and feeds the MPE reports. YK is drafting a V1 Spotfire-Automation + Serin-integration flow for the upcoming Raptor discussion.

**Raptor Phase 1 marked COMPLETE 05/28/2026.** Baku Products full production implementation started the week of 06/08/2026 (first lots shipping).

**FeederDB → Snowflake migration (2024, the blocker referenced above) — executed across 2024 in 3 phases**: Phase 1 (Wafer types 13/15/19/41, target 04/30/2024), Phase 2 (BE Module type 1), Phase 3 (misc types 2/14/25/26/27/39/49/50/51, target 01/10/2025). By year-end 2024 the migration had reached Phase 3 deployment across most CM sites (INR, AKR, CB, SEO). KP approved a 2024 Snowflake Capacity Contract expansion to $100K to fund this (01/12/2024).

**Key architecture decision surfaced mid-migration (Sep-Oct 2024)**: Snowflake's **Hybrid Table** type was tried to fix a table-level-write-lock concurrency issue on the "ProcLog" tracking table (high-frequency concurrent Lambda writes were being queued/dropped past a 20-request cap). Hybrid Tables fixed the write-concurrency problem (row-level not table-level locking) but hit a **show-stopper read-performance issue**: multi-field WHERE queries on a Hybrid Table took >5 minutes (vs. ~1s for Snowflake's Standard table type) — closed as unusable. Final solution (decided 10/08/2024, Chao's proposal): keep ProcLog as a **Standard** Snowflake table for reads, and add an **Aurora DB cache layer** in front of it via a new "Proclog API" (hosted on both AppRunner and EC2, split by caller domain to work around AppRunner's "Too Many Requests" throttling for non-AWS-domain callers) — write/read-write clients hit the Aurora cache API, read-only clients query Snowflake Standard directly.

**Why:** both are durable architecture decisions affecting how WSD's cloud data pipeline evolves, not one-off status items. The Hybrid-vs-Standard-plus-cache decision is a concrete instance of the team's benchmark-before-deciding principle (see [[feedback-engineering-principles]]) and is directly relevant to any future Snowflake concurrent-write design question.

**How to apply:** update the NXG section once the Snowflake/GenQS/FeederDB migration unblocks the simplification work — as of year-end 2024 that migration is essentially done (Phase 3 in deployment), so the NXG 3-layer simplification may now be unblocked; verify current status before assuming so. The Raptor Phase 1 integration is done — future updates here should track the Baku Products production rollout, not the integration method (that's settled). Related 2024 Clotho-platform-generation investigation (DEC2/Kafka/Magnus, distinct from this FeederDB work) is tracked in [[project-clotho-nxg-magnus-roadmap]].
