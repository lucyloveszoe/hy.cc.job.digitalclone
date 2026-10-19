---
name: project-safelock-api-review-2026-10
description: Review of the one-page Safelock API doc (S3 download proxy, Range, bucket/prefix sheet) plus a combined secured-solution design with the short-lived token endpoint: Tier 0-4 plan, Q1-Q15, D1-D8 (MD+PDF built, not sent)
metadata:
  type: project
---

Reviewed 2026-10-13 from `cc.brcm/WSD.Artifact.Rig.SW.InfraAPIImprovement.Research/family/safelock/Safelock API doc.docx` (one page, undated, no owner). Docs only: Safelock, `WebProxy/routingtable.js`, bucket policies and the policy Google Sheet were NOT seen/opened. Deliverables in the same folder: `Safelock_API_review_and_proposal.md` + `.pdf` (5 pp, headless Chrome; **not sent**; addressed to "Safelock owners (Kin Lum, Chao, Jiapeng per notes; confirm)", cc Tingyu). Proposal only, nothing decided.

**What the doc defines:** `https://safelock.wsd.lvn.broadcom.net/s3/<bucket>/<s3key>` translates a bucket URL to a private-IP route (routing table in `WebProxy/routingtable.js`); partial download by `Range: bytes=0-60` header or `?range=0-60&file=headersonly.txt`; allowed buckets/prefixes live in a Google Sheet. Nothing on auth, errors, limits, version, owner.

**Findings (check first):** no auth described (AuthHub/ID.Pass is only in our notes); "hidden URL" only holds if buckets are unreachable otherwise (legacy presign, ACLs); path/bucket normalisation fail-closed unknown; same-origin script risk on browser downloads (need attachment+nosniff+CSP); `file=` sanitising/meaning; policy in a spreadsheet vs code (two truths); token endpoint and downloads on the same host `safelock.wsd.sgn`. Design: authorization granularity, cross-service token reuse (need `aud`), token lifetime vs long download, no audit/anomaly, 403-vs-404 enumeration, range abuse. Docs: `bytes=0-60` is 61 bytes not "60 characters", range/206/416 semantics, no error table, sgn vs lvn token validity, no `/v1`, `cleada.sj` dotted bucket/alias unclear, proxy-vs-redirect unclear (earlier S3Proxy design = check auth then 1-minute presigned redirect).

**Combined secured solution (7 layers):** identity (AuthHub/ID.Pass with expiry, binding, revocation) -> token issue (POST JSON, `aud` per service, `scope`, `jti`, JWKS, audit; see [[project-okta-sso-jwt-architecture]] review of 2026-10-12) -> Safelock enforcement (JWKS verify, `aud=safelock`, scope `safelock:read`, AD-group claims -> prefix policy as code, default deny) -> private buckets only reachable from Safelock role/VPC endpoint -> response safety headers -> per-identity audit/rate limit -> one OpenAPI contract. Main new idea: **download tickets** (token-exchange of the access token into a ~5 min, single-object, caller-bound ticket in an Authorization header, never in the URL). Rules: no credential in a URL; different `aud` per service.

**Proposal:** Tier 0 verify (Q1-Q8 + six probes on a test bucket); Tier 1 docs rewrite; Tier 2 additive hardening (headers, `file=` sanitising, uniform 404, audit, rate limits, HEAD/ETag/416, policy-as-code); Tier 3 token integration (`aud`/`scope`, JWKS, tickets, host split, `/v1/s3` with alias); Tier 4 usage logs first. D1-D8 with recommendations; 15 owner questions (Q1 bucket bypass, Q2 auth/aud checked, Q3 path validation, Q4 `file=`, Q5 headers, Q6 sheet vs routingtable.js, Q7 redirect vs stream, Q8 token valid on both hosts).

**Ties to existing memory:** Safelock = S3 sharable-URL service built from WSD.Utility.S3Proxy (01/2025 hosts sgn/lvn) and the last of 6 apps moved to AuthHub (switched 10/13/2025, closed 10/21/2025) ([[project-okta-sso-jwt-architecture]]); many APIs hand out safelock URLs ([[project-file-access-api-unification-2026-10]], [[project-wafer-regen-api-review-2026-10]], [[project-tudas-api-doc-review-2026-10]], [[project-gdb-ws-api-doc-review-2026-10]]).

**How to apply:** send the PDF to the owners only when Yu Han approves, ideally in the same conversation as the token-endpoint review so Tingyu and Safelock owners agree `aud`/`scope`/ticket design; ask Q1-Q3 first (can reorder everything). The shortlife-token review files were not edited. Live Windows memory still needs the sync (repo snapshot only, not committed).

**Status 2026-10-13:** MD now ends with a Key actions list (6 items, no owners/dates) and status log; PDF rebuilt from it and in sync (5 pp). Pending: owner opinion exchange, Tier 0 probes, D1-D8 decisions.
