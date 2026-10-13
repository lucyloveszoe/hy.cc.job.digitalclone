---
name: project-oee-report-api-review-2026-10
description: Review of the one-page "OEE Report Processing API" docx (3 CSV reports via one POST endpoint) with tiered improvement proposal; no auth documented, doc defects, owner questions
metadata:
  type: project
---

Reviewed 2026-10-12 from `cc.brcm/WSD.Artifact.Rig.SW.InfraAPIImprovement.Research/family/oee/` (local docx, same file as the Box link in `urls.txt`; Box not re-fetched; docx text + one Postman screenshot only; live API not called, server code not seen). Deliverable: `OEE_Report_API_review_and_proposal.md` plus PDF (same folder, built 2026-10-12 for the OEE API owner; not yet sent). Proposal only, nothing decided or scheduled. Siblings: [[project-wudas-api-doc-review-2026-10]], [[project-tudas-api-doc-review-2026-10]], [[project-file-access-api-unification-2026-10]].

**What the doc says (verified from source):** `POST http://10.202.128.103/oeeweb/oeelambdav2` (form-urlencoded). Query string: `FileType` 34 Downtime / 35 Site-to-Site / 36 Hourly, `StartTime` inclusive, `EndTime` exclusive (`yyyy-MM-dd`), max 30 days. Body: required `TARGET_DEVICE`, `PROCESS_CODE`; optional `Device_code` (34), `MACHINE_CODE` (35), `PRODUCT`/`Trigger_By` EQUIP|MES (36); all comma lists. Response is CSV (undocumented; seen only in the screenshot: 28-day type-34 call = 11.11 MB, 3.54 s, 29 columns incl. `request_by/ack_by/complete_by/created_by/updated_by` and free-text remarks). No author, revision date, auth, errors or limits in the doc. Screenshot dates are 2020.

**Defects/gaps found:** `StartTime/EndTime` in the table vs `StartDate/EndDate` in examples; Python example uses start = end (empty under exclusive end); body-key casing drift; `Device_code` (34) vs `PRODUCT` (36) same example values; screenshot mixes type-34 and type-36 params and shows public `54.251.109.202` vs text `10.202.128.103`; three date formats in one CSV row, no timezone; no auth documented, plain HTTP to hard-coded IP; read as POST; no dimension (device/process/product) discovery; no "updated since" despite mutable rows (`status`, `updated_on`); CSV only, sync whole range.

**Proposal tiers:** Tier 0 verify (auth status/exposure, accepted param spellings, caller inventory); Tier 1 docs fixes + response contract + owner/changelog; Tier 2 additive (HTTPS+DNS, reuse ID.Pass->access token as WUDAS/File Access, error codes, dimension endpoints, `UpdatedFrom/To`, gzip/format, OpenAPI with one resource per report); Tier 3 bundle (ISO-8601 UTC and naming, shared async task envelope, privacy review of operator-ID columns, optional fold into WUDAS tables if data path allows); Tier 4 call-log-driven endpoints + Copilot tool schema ([[project-wsd-data-access-copilot]]). Order: 0, 1, Tier 2 transport/auth/errors, rest of 2, Tier 3 at upgrade cycle ([[feedback-engineering-principles]]). No effort estimates.

**Possible context (unverified link):** OEE IAM role was recreated via Terraform in 05/2025 ([[project-mythos-security-scanning]]); OEE dynamic-tester-grouping / Clotho+MQTT work is in [[project-clotho-nxg-magnus-roadmap]]. Whether this API relates to either is not established.

**Open questions for owner (11, at end of the write-up):** credential/exposure, accepted param names/case, current host, `Device_code` vs `PRODUCT`, >30-day behaviour and size ceiling, timezone/day boundary, late updates, need for operator-ID columns, FileType 34-36 vs zDB registry, caller inventory, data path behind the endpoint.

**Why:** the downtime CSV carries operator IDs and the doc shows no auth, while sibling APIs moved to ID.Pass JWT and legacy File Access paths were shut down 05/19/2025 ([[project-okta-sso-jwt-architecture]]).

**How to apply:** treat as a backlog source, not commitments; confirm with Yu Han which tier to pursue before opening Jira items. Lead with the Tier 0 auth question when talking to the owner.

**Cross-family umbrella (2026-10-12):** unify the contract first, then generate OpenAPI; see [[project-wsd-api-unification-2026-10]].
