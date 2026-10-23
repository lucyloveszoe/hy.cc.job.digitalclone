---
name: project-tudas-api-doc-review-2026-10
description: Review of TUDAS Phase 2 API Introduction (docx, last rev 3/17/2026) + Web Portal User Manual (PDF, 07/10/2025) with improvement proposal; what the docs contain and where they conflict with memory
metadata:
  type: project
---

Reviewed 2026-10-12 from `cc.brcm/WSD.Artifact.Rig.SW.InfraAPIImprovement.Research/family/tudas/` (text only; screenshots in the PDF and 2 images in the docx not inspected; live API not called). Proposal only, nothing here is decided or scheduled. Deliverable: `TUDAS_API_Review_and_Improvement_Proposal.md` plus a PDF of it (same folder; PDF verified word-for-word identical to the MD on 2026-10-12) for the TUDAS owners (Chao, Jiapeng, Tingyu, Ken) and Yu Han. Finding IDs below refer to that document.

**What the docs say (facts, verified from source):**
- API Introduction (Chao/Tingyu, rev log 05/02/2025 to 3/17/2026): base URLs `https://tudas.broadcom.net` (product) and `http://10.202.128.107/tudas/` (Spotfire Data Function, plain HTTP, hardcoded IP). Auth = AuthHub group membership (access requests go to Ken Sun by email) plus an id.pass JWT from the Atropos site, 6-month expiry, sent as `Authorization: Bearer`. Sample code uses `verify=False`.
- Endpoints: `getallpcname`, `querytaprawfile`, `querytaprawfilepaginated`, `querytaprawfilebycid`, `getsnpinfo`, `gettaprawfile`, `getalltapheaders`, `gettapresultstitch`, `gettapdetailrows`, `gettapsnpstitch`, `getsupplementalfiles`, `gettapsupplementalzip`, `gettapworstcase`, `gettelecasterwc` (adds `spec_func`, only `add`/`inverse_add`), `gettapworstcasesnp`, `checktaskstatus`, `tap/queryprogresspaginated`. Six heavy endpoints are async: return `statusURL`, client polls `checktaskstatus` (empty body = not ready, URL in body = done).
- Web Portal manual (Ken Sun, 05/14/2025, updated 07/10/2025, 15 pp): File Search/Download + tags, UC1 Result Stitch, UC2 SnP Stitch, UC3 Supplemental Zip, UC4 Worst Case Study, Query Pool. Does not cover SNP worst case, Telecaster WC, or `spec_func`.

**Conflicts with existing memory:**
- Use-case numbering: manual = UC1 Result Stitch / UC2 SnP / UC3 Supplemental / UC4 Worst Case; [[project-rd-bench-tap-testing-tudas-origin]] final roster = UC1 SNP / UC2 Worst Case / UC3 Supplemental / UC4 Result Stitch. Pick one numbering.
- Manual predates the 04/21/2026 portal rebuild to ASP.NET Core + React ([[project-ai-frontend-rebuild]]); its screenshots are likely stale.
- API doc auth wording is still pre-AuthHub; SLING-5 migration closed 10/21/2025 ([[project-okta-sso-jwt-architecture]]).

**Findings (IDs as in the proposal doc):** D1-D10 doc errors (incl. D7 GET-with-body on `tap/queryprogresspaginated`, D8 `cid` string vs list, D9 `retcol`/`snp_freq` ambiguity, D10 three drifting copies); S1-S5 security/auth (plain HTTP + IP, `verify=False`, unexplained "safelock" token, single-person access email, no token renewal/revocation doc); A1-A10 API design (only 200/400/500, empty-body polling, single-value meta filters, silent 200-row cap on `querytaprawfile`, page-number pagination, naming drift, no OpenAPI/versioning/limits, FCM logic in `gettelecasterwc`, absolute URLs, `checktaskstatus`/`gettaprawfile` unsupported by Data Function); M1-M5 manual gaps; 4.5 Penang silent-gap health monitor ([[project-tudas-oom-sqlite]], [[project-rd-bench-tap-testing-tudas-origin]]).

**Proposal tiers:**
- **Tier 1, docs, no risk (P1.1-P1.6):** fix D1-D9 (confirm D1, D5 against code first); one source of truth with version + change log; one UC numbering; AuthHub auth section, explain safelock token, replace `verify=False` with corporate-CA note; team PDL instead of personal email; re-shoot manual after rebuild and add access/token/limits/troubleshooting and shared-tag-link/download-expiry notes.
- **Tier 2, additive, small backend work (P2.1-P2.6):** OpenAPI + `/v1` (old paths aliased); 202 + Location, 401/403/404/429, one error body; `checktaskstatus` JSON (state/progress/error/downloadUrls/expiresAt); Data Function over HTTPS + DNS; data-freshness endpoint (cheap first step of the health monitor); list-valued meta filters, cursor pagination, `truncated` flag.
- **Tier 3, bundle into NXG TUDAS upgrade, may break (P3.1-P3.5):** unify naming/shapes; generic spec-expression layer replacing `gettelecasterwc`/`spec_func` list, aligned with the WSD Open Computation Framework POC (no target date per [[project-2026-ww41-weekly-review]]; the earlier "10/06/2026" was unsourced; related custom-computation work in [[project-fcm-analyzer-program]]); relative status/download URLs for multi-region ([[project-aws-resiliency]]); SNP/Telecaster worst case into Portal if FCM Analyzer is sunset; keep versioned contract stable while per-file SQLite lands. Batching follows [[feedback-engineering-principles]].
- **Suggested order:** all Tier 1; then P2.5 and P2.2/P2.3 first in Tier 2; rest of Tier 2; Tier 3 at the NXG upgrade. No effort estimates (code not reviewed).

**Open decisions requested from owner (unanswered):** (1) D1 `add(-10)` sign: doc or implementation wrong; (2) D5 `Set¡` vs `Set.`: real or artifact; (3) approve Tier 1 and name owner; (4) pick source of truth and UC numbering; (5) approve P2.5 first and whether it replaces the unstaffed health-monitor plan; (6) confirm Tier 3 in NXG scope; (7) share current numbers vs adoption target.

**Risks:** findings are doc-only, verify against live API; changing HTTP codes can break clients that treat non-200 as retry (ship under `/v1`, announce); HTTPS Data Function may need a cert Spotfire trusts (ask Spotfire admins); token renewal/revocation process to confirm with Atropos owner.

**Why:** adoption target (>40 active users, >120k task requests/month by 06/01/2026, set 11/2025) depends on API/doc usability; current status vs that target not recorded here.

**How to apply:** treat as a backlog source, not commitments; confirm with Yu Han which tier to pursue before opening Jira items.

**Cross-family umbrella (2026-10-12):** unify the contract first, then generate OpenAPI; see [[project-wsd-api-unification-2026-10]].
