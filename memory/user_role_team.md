---
name: user-role-team
description: "Yu Han's role as WSD Software Team manager, reporting line, team roster, and platforms owned"
metadata: 
  node_type: memory
  type: user
  originSessionId: b57dbea7-e0ff-4ed2-a55d-4625560a26cf
  modified: 2026-08-22T00:51:05.255Z
---

Yu Han manages the WSD (Broadcom) Software Team, spanning sites including SG, Penang, Seoul, and MUC. He reports up to "KP," who approves budget items, policy changes, and architecture direction. He sends a biweekly/weekly "WWL" status report to KP and the team.

Recurring named team members/contributors across weekly reports: Kin Lum, YK, Choo-Yau, Tingyu, Chao, Jiapeng, Ken, Foo-Kit, Brian, Kuok Liang, JJ. Other recurring stakeholders: Chung Sik, Mitchell, David (PE/stakeholders). This core roster (Kin Lum, YK, Choo-Yau, Tingyu, Chao, Jiapeng, plus Da Wang) is confirmed stable back to at least 2023 — see [[project-2023-annual-review]]. "Ken" = **Ken Sun**, who joined WSD SW team 11/06/2023 and completed his onboarding/ramp-up (including a 4-week weekly 1:1 with Yu Han) by 01/08/2024, using the Non-FBAR Wafer Sampling @ Chipbond project as his training vehicle.

Beyond people management, Yu Han personally owns and drives several cross-cutting technical decisions rather than delegating them fully: the WSD/AWS Resiliency initiative ([[project-aws-resiliency]]), WSD GenAI governance and OneTrust tool approvals ([[project-genai-program]]), and key architecture calls on zDB/WUDAS/TUDAS and pCD/Clotho ([[project-pcd-clotho-v7]]).

Team-owned platforms/systems: pCD/Clotho (wafer test execution/revision control), zDB, WUDAS/TUDAS (data pipelines), TPA (artifact system), gDB (genealogy DB), FCM Analyzer, PPYCC, MLOps/Atropos, GD-Breakdown DB, and WSD's GenAI agents (Clotho/MARA/PRA on Vectara).

Yu Han is WSD's named OneTrust requester/approver for AI tool use-case approvals (Cursor, Gemini/Canvas, GCP API projects) — this is the operational side of his Broadcom WSD AI Champion role.

**GenAI program ownership narrowed (May 2026)**: per Josh Kim's request, Yu Han transferred WSD GenAI "Non-AI-Coding" POC items #4–#10 to John Kim (who also became WSD EDA domain master, taking over the CEG EDA GenAI POC in ww11). Yu Han retains Production items #1 (Vectara Agents Rebuild) and #2 (G-AI Portal Phase II) only until the current dev/release cycle completes, then hands off #1 too. As of 05/21/2026 the split is tracked as "BLUE" track (Yu Han: Vectara-relevant dev, POC #7, Production #1–2) vs. "YELLOW" track (John Kim: OneTrust review, POC #6/8/9/10/11). See [[project-genai-program]].

Other recurring duties: Yu Han was assigned by KP as WSD SW team representative for a supplier cybersecurity audit (Inari Technologies, covering business continuity/encryption/datacenter security/IAM); he personally built and owns the "WSD GCP-API Umbrella" GCP API-gateway project (service account `wsd.gcpapi.hub`, completed 05/26/2026); and he prepares the Summary + "AI Coding for WSD" sections of the quarterly QBR (recurring quarterly duty, seen for Q2'FY26 and Q3'FY26). As of ww16 (04/21/2026) he was still completing required GTO GenAI training himself, after his team had already completed ACE training.

**New duties picked up June-Aug 2026**:
- Personally led the Mythos Git security-scanning effort for SW-owned repos to completion (see [[project-mythos-security-scanning]]), and temporarily covered a teammate's ownership of the non-SW-team Mythos scan for about 9 days.
- Initiated and drives "Git Repo Standardization/Optimization" — moving GitHub org ownership to a newly appointed "GitHub master" role and enforcing naming/branch/ownership standards, triggered by governance gaps the Mythos scan exposed in non-SW-owned repos.
- Leads WSD's response to GTO's "Maverick" multi-agent orchestration framework evaluation (see [[project-maverick-agentic-platform]]).
- John Kim (who already owns the GenAI "YELLOW" track) is separately now driving the next round of WSD Data Center expansion — collecting power/bandwidth requirements for consolidating old on-prem hardware into VMs.
