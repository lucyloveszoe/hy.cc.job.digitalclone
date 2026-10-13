---
name: project-diy-api-doc-review-2026-10
description: Review of the Die Inventory (DIY) Manual (upload/read/revert/stitch APIs): doc contradicts its own Bearer-token notice, no responses documented, email-as-secret, state-changing GET, tiered proposal + 14 owner questions
metadata:
  type: project
---

Reviewed 2026-10-12 from `cc.brcm/WSD.Artifact.Rig.SW.InfraAPIImprovement.Research/family/diy/` (`Die Inventory Manual.docx`, text-extracted). The Google Doc itself returned HTTP 401 and was not opened; the docx is assumed to be its export. Live API not called, token user guide and server/MCP code not seen. Deliverables there: `DIY_api_review_and_proposal.md` + `.pdf` (full evidence and reasoning) and `DIY_owner_brief.md` + `.pdf` (owner-facing: findings, 14 questions, D1-D10 decisions; built 2026-10-12, not yet sent). Proposal only, nothing decided or scheduled. Siblings: [[project-dpat-outlier-access-api-review-2026-10]], [[project-file-access-api-unification-2026-10]], [[project-wudas-api-doc-review-2026-10]].

**What the doc covers:** website on EC2 (`10.202.128.104/diysecure`); API Gateway `v2/diy-*` Lambdas (upload multipart with `secret`/`filetype` 0 AVI,1 TNR,2 DES; getwafers/getfiles/getwaferdetail with `csv=0/1`; GET revert; POST batch TnR revert; job history); stitching via WCF `zdbwcfv2/zdbService.svc/submitdiystitch` + `diystatus` and a "internal use" Lambda on stage `default`.

**Key findings (doc-only, inferences marked in the write-up):**
- The Mar 2025 notice requires Bearer tokens, but every API section still lists `secret: <broadcom email>` and no sample sends `Authorization`. Doc says old endpoints die 2025-04-30; memory says cutover moved to 05/18 (shutdown 05/19/2025) ([[project-okta-sso-jwt-architecture]]). Samples still use legacy `54.169.174.1/zdbwcf` over HTTP.
- No response schema for any endpoint; error list is 17 free-text strings; upload success returns no wid/fileid; revert is a state-changing GET with unclear "revert to what"; stitch result URL and `qtype=summary` undocumented; Sep 2026 C# worker behaviour (data set for non-uploaded wafer, Kam Heng confirmed useful) not in doc ([[project-2026-ww37-40-weekly-review]]).
- Possible bypass to verify: stitch Lambda on stage `default` vs authorizer added only on `v2`. Email in query string for job history (logged). Subcon robot callers (Inari/Chipbond token-refresh friction, Apr 2025) have no documented path.
- April 2025 Aurora CPU alarm came from unindexed `diystitchlog.taskid` ([[project-aws-resiliency]]): verify job-history query indexes.
- AI link: Foo-Kit's die-inventory MCP server (OneTrust #57517, approved 10/06/2026) and the Maverick DIY agent depend on this contract ([[project-maverick-agentic-platform]]); mutating tools (upload/revert) should not share a credential with read tools.

**Proposal tiers:** Tier 0 verify (`secret` honoured?, `default` stage auth?, legacy host alive?). Tier 1 docs only (auth rewrite, responses, error table, revert semantics, stitch docs, samples, remove internal host/IP). Tier 2 additive (ids on upload success, stable error codes, POST revert/job-history, cursor paging, discovery, audit log, one OpenAPI spec, robot-caller path). Tier 3 batch with AuthHub/NXG cycle (shared job envelope, close `default` stage, pre-signed upload only if files near 6-10 MB limits, token scopes read/upload/revert, idempotent transactional upload). Tier 4 usage-driven (caller inventory first).

**Decisions proposed (D1-D10, all recommendations, none decided; reasoning in the MD section 5):** D1 Tier 0 checks before docs (possible bypasses cost minutes). D2 `secret`: accept-and-ignore, announce removal after caller inventory (identity is in the token; abrupt removal breaks old subcon scripts). D3 revert: add POST, keep GET one deprecation window. D4 `default`-stage stitch Lambda: put behind the authorizer now, delete once only-website use is confirmed. D5 unattended subcon credential: decide with the short-life-token owners, default rotating service credential (ID.Pass design is shared; Inari manual refresh already failed, see [[project-okta-sso-jwt-architecture]]). D6 pre-signed S3 upload: defer until file sizes known (6-10 MB limits). D7 token scopes read/upload/revert, batch with AuthHub (agent tokens read-only by default). D8 error contract: additive stable codes now, shared CLEADA/WUDAS envelope next cycle. D9 one OpenAPI spec as source of truth, named owner and change log. D10 ownership: docs/checks with DIY owners, credential items with token owners, structural items next upgrade cycle.

**Priority owner questions (of 14):** Q1 does server still accept `secret` alone; Q2 does `default/diy-submitquery` require the authorizer; Q3 is legacy `54.169.174.1` host gone; Q4 response fields/CSV columns + sanitised samples; Q5 what revert reverts to and whether it is audited. Others cover batch-revert partial failure, lock lifetime/transactionality, file sizes, Inari/Chipbond auth today, stitch result URL, `qtype=summary`, indexes behind job history, MCP tool list, doc ownership.

**Next:** send `DIY_owner_brief.pdf` to the owners (Chao, Jiapeng, Ken, Kam Heng; owner confirmation pending), collect answers to Q1-Q5 first, then agree tier and D1-D10 and open Jira items. When answers arrive, update this file and the MD.

**Why:** DIY is the write path for NPI/CM wafer data and the next AI-agent tool surface, yet its doc is the least reconciled with the JWT migration of the family.

**How to apply:** treat as a backlog source; confirm tier with Yu Han before opening Jira items. Send the owner brief PDF once Yu Han approves. Not committed to git; live Windows memory still needs the sync.

**Cross-family umbrella (2026-10-12):** unify the contract first, then generate OpenAPI; see [[project-wsd-api-unification-2026-10]].
