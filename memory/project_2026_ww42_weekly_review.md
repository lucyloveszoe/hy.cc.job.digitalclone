---
name: project-2026-ww42-weekly-review
description: "Weekly-report archive for ww42 2026 (10/20) — what closed or changed vs ww41, supports/items not covered by a dedicated initiative file"
metadata:
  node_type: memory
  type: project
  originSessionId: 2ce3f337-bd6b-41b5-9047-7ec57ff19a41
  modified: 2026-10-23T00:10:32.714Z
---

Source: Software Team Weekly Update Summary (WWL) dated 10/20/2026 (ww42), diffed against [[project-2026-ww41-weekly-review]]. Most of the report is carry-over; many entries are re-dated 10/19-10/20 without new content (e.g. the Prisma block is word-for-word the ww41 status). Initiatives with their own files got a short dated note there (GenAI, Maverick, Mythos, schema-driven migration, pCD/Clotho V7). Structure unchanged: Completed Supports / Active Supports / Active Developments / In Queue (header says 35 queue topics, section title says x27 — same source inconsistency as ww41).

**Closed this week (moved to Completed Supports)**
- **Mythos Finding Fix w/ GitHub (Non-SW) & GitLab (FtC)** (Chao/Ken) — marked Complete. Final non-SW GitHub tally 10/20: 139 repos = 92 Yellow (exception to skip requested) + 19 Red (delete) + **28 Green (Round 2 scan complete; was 27)**; the "pending" repo from ww41 is gone. GitLab FtC still skipped per FtC management. Mythos report to GTO still shown "In Progress" (cumulative updates 08/27, 10/06). Reminders: GTO covered WSD's Mythos scan cost only until 10/01/2026; for future non-SW-owned repo scan requests, raise tickets via the JIRA SW Support group *before* WSD onboards AIS. See [[project-mythos-security-scanning]].
- **WUDAS non-FBAR wafer pass-bin criteria** (Ken, per Min Yoke) — Complete; 10/20 line lists "Min Yoke buy-off" (ww41 said pending). Values: t25 ncf2 {0,1}→{1,81-89}; t51 nfwps {1}→{1,81-89}; ncf3/awscaoi/awscdc unchanged. Details already in [[project-ai-friendly-schema-driven-migration]].

**Changes in Active Support**
- **JIRA "WSD Software Support" (WSDSWS) agile project** (Yu Han): 10/20 — trial run to confirm everything works before marking complete. Open issues: WWL→JIRA sync to initialize JIRA still being worked; waiting on GTO to fix the "READ-ONLY" state of the "WSD Software Support Plan". Companion project WSDSWD ("WSD Software Development", with its own dashboard and plan view) holds major projects. Rationale line in the report: "AI acceleration makes Agile possible".
- **WUDAS Python package → 3.13** (Kin Lum): 10/19 upgrade in progress (no change otherwise; Python 3.10 EOL is Oct 2026).
- **Prisma (GTO request)**: no change since ww41 (IAM key rotation done pending GTO update; Aurora SG exception link pending; IMDSv2 check in progress). See [[project-aws-resiliency]].
- **Tech Direction / Infra APIs Consolidation** (21 families): unchanged. See [[project-wsd-api-unification-2026-10]].

**Active Development changes**
- **AI-Friendly Schema-Driven Migration (II) Transfer & CI/CD**: 10/20 internal sync decided to **hold until pCD-V official release** (ww41 had just moved it into the active queue). (III) CLEADA Stitcher still "Initiated", last status 08/04.
- **Spotfire upgrade to LTS V14.6 (end of support)** (Choo-Yau/Chao): 10/19 — now an active development item (ww41 only had it as a queue placeholder); starting investigation to propose a schedule with workload estimate; target TBD.
- **Golden-Dictionary NXG-WUDAS** (Kin Lum/Chao): status text — Chao evaluating Snowflake against the full 14B rows / 120 columns; decided to drop Parquet and External Table options and load csv straight into Snowflake (requires re-parsing); warehouse sizing for best ROI still to decide. (Not dated this week; may be older text.)
- **WSD Open Computation Framework POC**: 10/19 internal kick-off sync held (recording exists). Otherwise as ww41.
- **pCD-V**: target still 12/04/2026; Kin Lum's workload breakdown/schedule says initial sharing starts either the 11/20 week or the 12/07 week. See [[project-pcd-clotho-v7]].
- **Secure Vectara/GTO Production Environment**: still 10/27/2026, no change.

**Not filed (flag for Yu Han)**
- The report again contains a **plaintext service auth-token value** for the Die Inventory MCP container env var, and **Zoom recording passcodes** (several). Deliberately not recorded; same flag as ww41 — consider scrubbing them from the WWL doc.
- **Codex/ChatGPT budget**: ww42 says "$550/user" for OpenAI (covered by GTO) in the spending summary and "$550 GTO funded budget applies to both Codex and ChatGPT for 2026 Oct spending" in the Codex section; ww40 had recorded $150. "Per user" wording hints it is a per-user cap, but unconfirmed — don't rely on either figure.

**Why:** keeps weekly status deltas (what actually closed, slipped, or changed) available without bloating the big initiative files.
**How to apply:** use as the ww42 reference; for a topic with its own file, that file holds the latest status. For ww43, diff against this report's state.
