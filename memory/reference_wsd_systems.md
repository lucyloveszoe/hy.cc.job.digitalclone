---
name: reference-wsd-systems
description: "Where WSD software team info/status/policy lives — JIRA, roadmap sheet, GitHub org, GenAI KB articles"
metadata: 
  node_type: memory
  type: reference
  originSessionId: b57dbea7-e0ff-4ed2-a55d-4625560a26cf
  modified: 2026-08-25T20:30:31.330Z
---

- **JIRA**: WSD SW team active pool with status & backlog — jira.broadcom.net PortfolioPlanView (id=59, sid=61, vid=164).
- **SW Team 3-Year Road Map**: a Google Sheet — marked **Obsolete** as of ww30 (07/28/2026); don't treat it as current, and don't have a confirmed replacement link yet.
- **GitHub org**: `wsd-broadcom` — hosts GenAI POC, pCD/Clotho, TPA, WUDAS, Wafer, and MLOps repos (e.g. `WSD.GenAI.POC`, `WSD.KB.Chatbot`, `WSD.TF`).
- **WSD Atropos portal**: MLOps portal, accessed via Broadcom OKTA.
- **GenAI KB articles** (Broadcom GTO knowledge base): Usage/Policy (KB0023277), OneTrust Questionnaire (KB0023618), Cursor Onboarding (KB0023366), Cursor Pricing/FAQ (KB0023715).
- **Cursor tracking sheets**: license control sheet, "Best Practices" sheet, "Interested Case Tracking" sheet.
- **WSD-GENAI-GENERIC**: Google Group / PDL for WSD's GenAI program (~150-166 members early 2026, grown to 456 members by ww34/Aug 2026). Chat: "WSD ClaudeCode/Cursor/Gemini User Group" Google Chat space.
- **SW Team Projects Site**: `https://atropos.s3.ap-southeast-1.amazonaws.com/wsd.sw/wsd-sw-projects.html` — lists all SW team projects since 2010, tag-based, rebuilt for UX in ww17 (Apr 2026).
- **2026 weekly status reports (WWL) archive**: stored in a Box folder (link given in the reports themselves) — the canonical archive of the same reports this project's `/log` skill processes.
- **`WSD.AspNetCoreReact`**: GitHub template repo for the ASP.NET Core (.NET 10) + React.js portal-rebuild standard — see [[project-ai-frontend-rebuild]].
- **GenAI/Cursor/Claude KB articles**: Usage/Policy (KB0023277), OneTrust Questionnaire (KB0023618), Cursor Onboarding (KB0023366), Cursor Pricing/FAQ (KB0023715/KB0023386), Claude Code FAQs (KB0024051), On-Prem GTO Model Inferencing (Google Doc, no stable link captured).
- **WSD's own GitHub Enterprise** (separate from GTO's) plus **GTO JFrog** for ML dataset storage.
- **Mythos scan findings**: Box folders `wsd-swteam-repos-mythos-findings` and `wsd-non-swteam-repos-mythos-findings`.
- **Vectara support**: broadcom-gto-support@vectara.com; internal PDL wsd-genaipoc-private-aiagent.pdl@broadcom.com.
- **GTO-approved direct-LLM models** (for on-prem/self-hosted use): mistralai/devstral-small-2505, Llama-4-Scout-17B, gpt-oss-120b, gemma-4-31b.
- **Maverick documentation**: maverick-docs.epc-vks.lvn.broadcom.net. **Atropos portal**: reachable via Okta search "WSD Atropos."
- **WUDAS docs**: DocFx site `http://wsdsgsvc03.sgn.broadcom.net:8080/WSD.Wudas.DocFx/index.html` (HTTP only), schema PDF `ZdbDataSchema_Rev2.16_20260828.pdf` on Box, Google Site `sites.google.com/broadcom.com/wudas` — see [[project-wudas-api-doc-review-2026-10]].
- **OEE Report Processing API**: doc on Box (`broadcom.ent.box.com/file/862901991610`), endpoint `http://10.202.128.103/oeeweb/oeelambdav2` (HTTP, filetype 34/35/36) — see [[project-oee-report-api-review-2026-10]].
- **CLEADA docs**: DocFx site `http://wsdsgsvc03.sgn.broadcom.net:8080/WSD.Cleada.DocFx/` (HTTP only; pages: JobSubmit, JobStatus, JobCancel, Response Code, On-Demand Glacier, DPAT Specification, DPAT Outlier Detail), Google Site `sites.google.com/broadcom.com/cleadawsd` — see [[project-dpat-outlier-access-api-review-2026-10]].

- **Die Inventory (DIY) docs**: Google Doc `docs.google.com/document/d/1Y7TOC8YbEuTM4CX6M6HtrIHMWfcQhhUWU97GUSiAaz4` (private, needs login), website `http://10.202.128.104/diysecure/diy/index.html`, API Gateway `https://8ov8whc99j.execute-api.ap-southeast-1.amazonaws.com/v2/diy-*`, stitch `http://10.202.128.101/zdbwcfv2/zdbService.svc/submitdiystitch` — see [[project-diy-api-doc-review-2026-10]].
**How to apply:** point to these when Yu Han asks "where do I find X" about his team's status/backlog/policy info, or when drafting anything that should link to the canonical source instead of restating it.
- **gDB Web Service docs**: DocFx `http://wsdsgsvc03.sgn.broadcom.net:8080/GenealogyDB.DocFx/` (62 tables incl. 15 PPYCC `npip_*`; data endpoint `http://10.202.128.107/v2/genealogyapi/ProcessQuery`; proxy config `https://atropos.s3-ap-southeast-1.amazonaws.com/gdbwebservice.html`); the Box PDF "Genealogy DB Web Service User Guide 1.10 (20231018)" is stale (token-in-body, 37 tables). Review in [[project-gdb-ws-api-doc-review-2026-10]].
- **API unification synthesis**: `cc.brcm/WSD.Artifact.Rig.SW.InfraAPIImprovement.Research/unification.md` (+ PDF) ties the eight family reviews together — see [[project-wsd-api-unification-2026-10]].
