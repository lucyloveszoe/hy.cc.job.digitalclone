---
name: project-2026-ww41-weekly-review
description: Weekly-report archive for ww41 2026 (10/13) — what closed, what changed vs ww40, supports/items not covered by a dedicated initiative file
metadata:
  node_type: memory
  type: project
---

Source: Software Team Weekly Update Summary (WWL) dated 10/13/2026 (ww41), diffed against the ww40 report. Continues [[project-2026-ww37-40-weekly-review]]. Initiatives with their own files got a short dated note there (schema-driven migration, pCD/Clotho V7, GenAI, Maverick, Mythos); this file holds the rest. Report structure unchanged: Completed Projects / Completed-Closed Supports / Active Support (30%) / Active Development / Production & IT Support / In Queue (the header says 35 queue topics, the section title says x27 — source inconsistency, not reconciled).

**Closed this week**
- **AI-Friendly Schema-Driven Migration (I) Feeder** — moved to Completed Projects, "Completed @ 10/13/2026" (original target 10/20). The 10/13 note still says "monitor 1 more week without issue, then mark as completion", so completion is stated alongside a final watch week. See [[project-ai-friendly-schema-driven-migration]].
- **Urgent PPYCC Financial Upgrade Y048 (FTC FBAR ML cost adder)** — web UI (Choo-Yau) and Excel reports (Chao) done 10/08; Kok Kit confirmed buy-off 10/09; complete (target was 10/15).
- **INR Becketts "year 2036" datetime / missing raw files** (Kin Lum) — 10/07 Chun Hao officially asked for a reset-and-reload ("option #2": Inari places corrected raw files in a request folder on the zDB share); Kin Lum cleaned up the bad data, recollected and re-parsed; Chun Hao bought off 10/12. Both sides agreed this is not continuing support; Inari is adding preventive action.
- **ATA Utilization Report** (Choo-Yau/YK) — V2 separates ATA and Spotfire stats, loads real DB data, has a monthly distribution; ATA workload is "very unbalanced". Ken integrated it and scheduled it every Wednesday 9:00 AM. Known gap: the current custom auto-scaling EC2 setup can't give per-EC2 CPU or memory history. This report is the precondition for NXG ATA (project to be renamed, e.g. WCCF) — goals: support Python and C# (container?) and add FCM custom computation (outlier removal etc.) via TUDAS; Chao started FCM custom-computation requirements negotiation 10/02.
- **AOD into WSD Snowflake Enterprise Access** (Chao) — AOD confirmed access OK 10/07; complete. AOD sub-account spend appears under locator NM20055.

**New / changed items**
- **SW Team Tech Direction Discussion with Infra APIs Consolidation** (SW team, In Progress) — new Active Support entry: a Google Sheet holds the tech direction and a summary of the **21 SW infra API families**. This is the umbrella context behind the per-family API reviews ([[project-wsd-api-unification-2026-10]]).
- **Prisma (GTO request "WSD - Prisma - critical & High alerts")** — 10/13 progress: Ken completed "IAM access key rotation hygiene" (pending update to GTO); Kin Lum confirmed the Aurora DB security groups only allow specific inbound IPs (pending sync with GTO for the link to raise an exception); Kin Lum is checking/updating owned EC2 instances for IMDSv2 (in progress); SW-team internal tracking sheet is based on a Claude Code summary of the 427 alerts. Defender-agent exemption for ECS clusters was approved 09/15. Background: [[project-aws-resiliency]].
- **Mythos non-SW GitHub (10/13)**: still 139 repos — 92 Yellow (exception to skip requested), 19 Red (delete), 27 Green (Round 2 scan done), 1 pending Round 2 scan (was "pending owner response" on 10/06). GitLab FtC still skipped per FtC management. See [[project-mythos-security-scanning]].
- **Secure Vectara/GTO Production Environment**: **target date moved 10/06 → 10/27/2026**. Auto-Grader migrating from LM Studio @ ETX to Maverick with a Claude model (Vectara itself recommends the Maverick grader); Seoul's lee.yu-seon is being onboarded to the Vectara RAG-agent workforce for the Seoul Issue Tracking system; still checking a better generating model via Vertex AI. Brain's how-to summary: build an ingestion process (Vectara container or drag-and-drop), build an agent profile (prompt, tooling; script or web app), then hand the agent id to Tingyu to post to the G-AI portal. See [[project-genai-program]].
- **WSD SW Team Jira Analyzer** (Maverick item 4, renamed from "Jira Workload Dashboard"): the SW team's workload lives in two Jira groups — WSDSWS (support, 49 topics with owners) and the "WSD SWD Plan" portfolio (major projects); the planned Maverick agent analyzes both to generate team updates and workload analysis. See [[project-maverick-agentic-platform]].
- **JIRA "WSD SW Team Agile Support"** (Yu Han): WSDSWS project created 10/07 by Aruna with owner-assignment automation by Support Type; the 10/13 plan is a 2-week trial run before marking complete.
- **WUDAS Python package → 3.13** (Kin Lum): upgrade in progress (requested by Gerd Meisl; 3.13 confirmed 09/29).
- **WUDAS non-FBAR wafer pass-bin criteria** (Ken): deployed with the new Zdb.Feeder schema; pending Min Yoke's buy-off (no change vs 10/06).
- **WSD Open Computation Framework POC** (Jiapeng/Chao, no target date): 3 vehicles — NXG ATA upgrade, FCM custom computation, generic reporting; container-based (ECS/Lambda/EKS?), language-independent, full CI/CD where a Git check-in triggers deployment with selectable resource specs (e.g. memory). 10/13: Jiapeng's and Chao's design docs (in the TudasEnhancePlugin `doc` folder of WSD.TapToZdb) are being merged into one design doc. Related: [[project-fcm-analyzer-program]].
- **Production & IT Support section (this report)**: gDB Web API new tables `Wafer_probe_tnr_header` and `Wafer_probe_tnr_bin` requested by YC (done); new .ppk key for the Amkor Vietnam gDB SFTP cm user (done); SFTP uploader now accepts .txt and .csv for type 4 (FBAR_FG) (done); MS patches for SGN servers (done) — Choo-Yau/YK.

**Not filed (flag for Yu Han)**
- The report contains a **plaintext service auth-token value** for the Die Inventory MCP container's env var, and **Zoom recording passcodes** (also present in earlier reports). Deliberately not recorded in memory. Consider scrubbing them from the WWL doc itself.
- **Codex/ChatGPT GTO-funded budget**: ww40 recorded $150; the ww41 report says **$550** "for 2026 Oct spending". Could be a typo or a real increase — confirm before relying on either figure.

**Why:** keeps weekly status deltas (what actually closed, slipped, or changed) available without bloating the big initiative files.
**How to apply:** use as the ww41 reference; for a topic with its own file, that file holds the latest status. For ww42, diff against this report's state.
