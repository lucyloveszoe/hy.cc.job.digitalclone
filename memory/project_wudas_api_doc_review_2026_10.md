---
name: project-wudas-api-doc-review-2026-10
description: Review of WUDAS DocFx site + ZdbDataSchema Rev 2.16 (2026-08-28) PDF with ranked improvement proposal; what the docs contain, doc/schema defects, and links to existing WUDAS memory
metadata:
  type: project
---

Reviewed 2026-10-12 from `cc.brcm/WSD.Artifact.Rig.SW.InfraAPIImprovement.Research/family/wudas/` (PDF text-extracted, table layout scrambled; DocFx site fetched via curl, WebFetch can't reach it because it forces HTTPS; live API not called; Google Site and Atropos not reviewed). Full write-up: `WUDAS_review_and_proposal.md` in that folder; shareable PDF version `WUDAS_review_and_proposal.pdf` generated 2026-10-12 for the project owner (internal memory/status sections stripped; not yet sent). Proposal only, nothing here is decided or scheduled. Sibling review: [[project-tudas-api-doc-review-2026-10]].

**What the docs say (facts, verified from source):**
- DocFx site `http://wsdsgsvc03.sgn.broadcom.net:8080/WSD.Wudas.DocFx/` (plain HTTP) has only 4 pages: index, Auth Endpoint, Web API examples, SDK get-started. Source of the schema PDF is a Box file, not the site.
- Flow: ID.Pass JWT (from Atropos, read from `%USERPROFILE%\idpass.jwt` in samples) → `GET …/v1/authendpoint/accesstoken` (expiry in `X-W-Expiry`) → `POST …/v2/jobsubmit` (raw SQL body; headers `X-W-Region: sg`, `X-W-Database: zdb`; returns `{code:200, taskid}`) → `POST …/v2/jobstatus` (task id body; `code` 205 = complete, returns `taskresult` URL) → `GET` the URL, CSV only, stored under S3 path `cleada.sg/1d/wudas/`.
- SDKs `wsddatawudas` (Python 3.10) and `WSD.Data.WUDAS` (.NET 6/8/Framework 4.7), both v2.0.1, distributed from UNC shares in Penang and San Jose.
- Schema Rev 2.16 (42 pp): ~100 tables in 15 domains (BE Module, Impedance, FBAR Wafer, Non-FBAR Wafer, GU/VNA Calibration, Wafer Maverick, CuP AOI, EBR DevProto, NFWPS, Pcb Cu, TVer, FVI 2DID, TTD, SAVI/SMETA). Per table: columns, mandatory-filter sets, optional filters, example SQL. Wide tables with dynamic columns each have a Slim tall twin.

**Defects found:** column typos (`Correation` ×3, `ContactorID` vs `ContractorID`, `TesterIPAdress`, `LoadboardName`, `Filename`); mixed naming conventions and a `2` suffix that reads as a version but means another domain; same concept under different names (`TotalRun`/`TotRuns`/`TotalRuns`, `Parameter`/`ParameterName`, 7 time-column names); filters that are not columns (`ID2D`, `MfgModuleId`, `coordinates` vs `coordinate`, TTD `CMDate`); mixed date formats and timezone unspecified except FBAR `ProcessTime` (UTC); no types/units/nullability/joins; broken example (`LowLimit, FROM`); `TopParameter.LotID` "always return empty". API side: status in JSON body not HTTP status, CSV-only output, no schema-discovery or validate endpoint, Windows-only samples, raw SQL sent as `application/json`, long-lived plaintext JWT file.

**Existing memory this ties to (not conflicts):**
- Backend is Snowflake; the 03/17/2026 Summary/File Access API outage from TLS 1.1 retirement is in [[project-aws-resiliency]]; the 10/2021 perf fix and 11/2021 ODBC retirement are in [[project-2021-partial-review]].
- Schema-driven `Zdb.Feeder` migration ([[project-ai-friendly-schema-driven-migration]]) is the natural single source for generated docs. The same note records the non-FBAR pass-bin criteria change, which shows schema behaviour changes that the PDF must track.
- Data Access Copilot ([[project-wsd-data-access-copilot]]) already plans to mine 100K+ WUDAS queries and wants a typed schema + OpenAPI as agent tools; a schema JSON and `/schema` endpoint serve it directly.
- 10/06/2026 NCDetail slow-query fix (17 min → 3.5 s) is in [[project-2026-ww37-40-weekly-review]].
- Large-file strain (Type 53 etc.) is a File Access API topic, see [[project-zdb-large-file-risk]]; File Access API docs are not part of this schema PDF.

**Proposal themes (ranked by value for effort):**
1. Docs-only: fix the typos/broken example/Windows-only samples, add the missing reference pages (`code` table, errors, limits, retention, valid region/database values, SQL dialect, date/timezone rule), serve over HTTPS, link the schema.
2. Additive: generate the schema as machine-readable JSON and render PDF and DocFx from it; `GET /v2/schema` and `POST /v2/jobvalidate`; OpenAPI spec; Parquet/gzip output; SDK on the internal artifact repo (JFrog), cross-platform, token cache.
3. Bundle into a schema upgrade cycle (per [[feedback-engineering-principles]]): rename/unify columns behind compatibility views, parameter dictionary, UTC standardisation, Okta short-life auth.
4. Usage-driven: mine query history into named cached endpoints, published limits and a cost guard.
Suggested first sprint: docs fixes, `jobvalidate`, schema JSON.

**Reasoning for the ranking (short):** docs-only fixes carry no regression risk and hit every user; `jobvalidate` + `/schema` fix the "mandatory filters only in the PDF" gap and double as the copilot's typed tools; a generated schema JSON avoids a third copy of the schema; renames wait for an upgrade cycle because Spotfire Data Functions/user scripts may reference the typo'd names; auth change needs its own plan. Evidence (PDF text extract, DocFx snapshot, extraction script) is in `review-evidence/` in that folder; the PDF extract scrambles table layout, so spot-check before relying on a single table.

**Why:** WUDAS is the main read path for production test data (users, Spotfire, agents); doc and schema drift is a recurring support cost.

**How to apply:** treat as a backlog source, not commitments; confirm with Yu Han which tier to pursue before opening Jira items. Open questions for owners are listed at the end of the full write-up (other `X-W-Database` values, full `code` set, result retention, whether renames are safe for Spotfire Data Functions).

**Cross-family umbrella (2026-10-12):** unify the contract first, then generate OpenAPI; see [[project-wsd-api-unification-2026-10]].
