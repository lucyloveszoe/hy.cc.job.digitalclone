---
name: project-2026-ww37-40-weekly-review
description: Weekly-report archive for ww37-ww40 2026 (09/15-10/06) — small supports/incidents not covered by a dedicated initiative file
metadata:
  node_type: memory
  type: project
---

Source: Software Team Weekly Update Summary (WWL) reports dated 09/15, 09/22, 09/29, 10/06/2026. Big initiatives from these weeks were filed into their own files (Mythos, Git naming, pCD/Clotho, schema-driven migration, GenAI, Maverick, FCM, Amkor ATV, AWS IAM, WDAC, GCP APIs) — this file holds the rest. Report structure: Completed Projects / Completed-Closed Supports / Active Support (30%) / Active Development / In Queue (25+ topics at ww37, 34 at ww39, 35 at ww40).

**Incidents and supports closed**
- **Dieinventory Stitch worker upgrade** (Jiapeng/Chao): stitch-merge code moved from Python to pure C#; handles wafers not in the DB. Yu Han asked whether returning a data set for a not-uploaded wafer is still useful vs. returning an error code; Kam Heng confirmed it is. Deployed and bought off 09/15.
- **TUDAS snp stitching** one-off: an old test bench (FCM says it can't upgrade; shared with R&D, old TAP version) emits square xlsx files lacking the snp filename, so Theja was told to submit the stitching payload (cid + snp filename) directly via the TUDAS API.
- **INR P13 DPAT server slowdown (09/06/2026)**: no outage, but file moves (esp. Type-1) on the local disk slowed, delaying DPAT; cause unknown; Chung Kiat advised holding any SATA→SSD migration. Normal by 09/15, closed with plan to add monitoring (YK/Choo-Yau).
- **Inari SFTP server memory spike (09/19-09/20)**: SFTP server inaccessible ~09/19 15:00 UTC to ~09/20 01:50 UTC, DPAT lot-end reports delayed (a recurring DPAT problem, different failure pattern); Inari said the service stopped due to a memory spike; after one week of watching Inari's monitoring, closed 09/29.
- **CBMY (Chipbond Perai, Malaysia) FY26 supplier cyber-security assessment** observed/complete (see [[project-supplier-cybersecurity-audit]]).
- **SGN server OS upgrade (MS Server 2016→2022)**: all 5 servers done; intermittent MySQL connection failures after the upgrade (suspected TLS/SSL) took ~2 months (07/17-09/22) — retries, library changes, EC2 testing, GTO/network team and an AWS support ticket; AWS Singapore tunnel failover suspected; Dean updated the Broadcom gateway Phase-2 PFS/DH-group settings 09/14; stable by 09/22, closed.
- **RadarToJira VM**: removed the "Action1" RMM agent service after IT incident INC3153032 (Chao).
- **Serin/BCSG data upload** (Choo-Yau/YK): Raptor wanted one zip per lot; proposal to zip up to 100 lots per zip file; Rebecca changed her program 10/05, no change on SW side; closed 10/06.
- **Serin SDK client access expiration**: reconfigured with the customer (Arturo), completed.
- **NCDetail slow queries (WUDAS/CLEADA, Kin Lum, 10/01-10/06)**: Turker's small queries took 17-18 min because the worker did many small range downloads; Claude Code analyzed and redesigned it to download once (parallelized range-GET, skip BinaryRange for tiny requests): 17 min→3.5 s and 18 min→8 s, no side effects, completed 10/06.

**Active as of 10/06/2026**
- **INR "year 2036" datetime / missing raw files (Becketts) reprocessing** (Kin Lum): Inari fixed it for future lots; waiting on PE for next step on already-processed lots.
- **WUDAS Python package upgrade to Python 3.13** (Kin Lum): requested by Gerd Meisl (Python 3.10 EOL Oct 2026); Gerd confirmed 3.13 on 09/29; Kin Lum planning the upgrade.
- **Urgent PPYCC Financial Upgrade "Y048 – FBAR FTC ML (MassLoad) cost adder"** (Choo-Yau/Chao, target 10/15/2026): add an ML cost adder to the FTC FBAR wafer rate; touches BOM Input, Financial Rate Entry and Wafer P/N Setup UIs plus the Project_Input, Financial and Summary Excel reports. Started 09/28 as an urgent project; web UI (Choo-Yau) and reports (Chao) in progress.
- **JIRA "WSD SW Team Agile Support" category** (Yu Han, with Chung Sik and Aruna): premise "AI acceleration makes Agile possible"; requests route by topic to a domain owner (one owner per topic). Topic list grew to 49 topics with owners; Jira project WSDSWS created 10/06 (ww41 says 10/07 by Aruna — use 10/07) and Aruna is updating it.
- **AOD into WSD Snowflake Enterprise Access** (Chao): AOD chose Option 1 (09/28); a separate AOD sub-account (Enterprise edition, GCP us-central1) is being created, with usage showing under locator NM20055; pending AOD confirmation. 
- **Snowflake tech sync**: (I) Adaptive Compute (up to 30% cost reduction; needs migration from Type I to Type II warehouse, not a simple clone — pending bandwidth for pro/con), (II) Cortex Analyst/Cortex Code text-to-SQL (follow-up deep-dive agreed), (III) Dynamic Tables (Snowflake to rebuild one existing pipeline as a demo; 09/22 session).
- **Backend AVI Wafer Maps Collection and Access** (Ken/Kin Lum): request is for Production AVI .A files from CB/Inari (NPI .A files already arrive via Die Inventory); proposal is an enhanced zip-access API with on-the-fly .A conversion; 09/22 Chung Sik's side said direction is clear but no must-have timeline, so held in the queue.
- **On hold**: Extractor Traceability (needs a solid use case); Ken's Cloud-Native Scheduler ([[project-ken-cloud-native-scheduler-proposal]]); "change GU Cal File Parsing Logic"; GTO cert migration to Keyfactor ([[project-certificate-lifecycle-keyfactor]] — SW members' certificate-owner validation done 07/28); TDG's preliminary "PAD Dynamic Sampling Test" (book-keeping only; Yu Han's assumption: not urgent, minimal Clotho change, achieve it inside test-plan code).
- **Placeholders added to the queue**: NXG ATA Upgrade (incl. FCM custom computation) and Spotfire upgrade to LTS V14.6 due to end of support (Choo-Yau/Chao).

**Why:** keeps ongoing support/incident context and lessons (e.g. the NCDetail fix, the MySQL-after-OS-upgrade saga) available without bloating the big initiative files.
**How to apply:** use as the ww37-ww40 reference for status questions; for a topic with its own file, that file holds the latest status.
