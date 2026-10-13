---
name: project-dpat-outlier-access-api-review-2026-10
description: Review of CLEADA DPAT outlier access API (dpatoutlierspec / dpatoutlierdetail) from the WSD.Cleada.DocFx site, with ranked improvement proposal and owner questions
metadata:
  type: project
---

Reviewed 2026-10-12 from `cc.brcm/WSD.Artifact.Rig.SW.InfraAPIImprovement.Research/family/dpat-outlier-access/`. Docs only: pages fetched with curl from `http://wsdsgsvc03.sgn.broadcom.net:8080/WSD.Cleada.DocFx/docs/` (HTTP only); live API not called, result CSVs never seen, CLEADA server code, Google Site `sites.google.com/broadcom.com/cleadawsd` and Atropos not reviewed. Write-up: `DPAT_outlier_access_review_and_proposal.md` plus shareable `.pdf` (6 pages, for the project owners) in that folder; evidence snapshot in `review-evidence/docfx_cleada_snapshot_2026-10-12/`.

**What the docs say (verified from source):**
- DPAT access = two CLEADA task types on the shared async job API: `dpatoutlierspec` (args `ProductName`, `SpecVersion` e.g. `"1,3"`) and `dpatoutlierdetail` (args `ProductName`, `LotID` comma list). Body is `{name, scope:"m", args}`.
- Flow: ID.Pass → `GET /v1/authendpoint/accesstoken` → `POST https://5cdwh72vzi…/v2/zdbService.svc/submitjob` (also `jobstatus`, `canceljob`) → `code` 205 gives `taskresult` URL → CSV from S3 (`…/download/s3/cleada.sj/1d/Cleada/…csv`, `1d` suggests one-day retention).
- Codes: 200 accepted, 201 duplicate, 202 pending, 203 running, 204 canceled, 205 complete, 400-406, 500 web error, 501 worker error.

**Main findings:** output CSV never described (columns/grain); `SpecVersion` format and omit-behaviour unclear; no product/version discovery; `scope` undefined (relates to M / L / mtk lanes in [[project-cleada-autoscaling-cluster]]); `jobcancel` page copy-pasted from WUDAS (says GUID); Python and C# post different `jobstatus` bodies; 403 collides with HTTP meaning and the documented "no result" example is 501; "no result" for detail cannot distinguish zero outliers vs not processed vs failed (inference, ties to the 2023 ECS silent-failure and Aug 2025 Proclog disk-full DPAT incidents, see [[feedback-engineering-principles]] and [[project-aws-resiliency]]); samples are Windows-only with no polling loop.

**Proposal tiers:** T1 docs-only (describe outputs, arg tables, fix job-API pages/samples, HTTPS, task catalog); T2 additive (`dpatoutlierspecversions` discovery, optional JSON format, spec version in detail output, `dpatlotstatus`, cache immutable specs, JSON Schema/OpenAPI, `retryafter`); T3 breaking and batched (clean code space, drop `.svc`, per-product authz, shared job API across CLEADA/WUDAS/TUDAS); T4 usage-driven (find real consumers first; clarify relation to `.olf` files and the ViTrox toolkit, see [[project-vitrox-production-toolkit]]). First sprint: T1 items plus spec-version discovery.

**Why:** DPAT outlier data is production-critical test IP and outages were noticed late; the API is the programmatic path for it, and the docs cannot be coded against.

**How to apply:** treat as a backlog source, not commitments. Status: proposal only, nothing decided. The 10 owner questions at the end of the write-up (CSV columns, `SpecVersion` semantics, `scope`, code per case, lot limits, per-product access, retention, real consumers) should go to the CLEADA/DPAT owners before opening Jira items. Related auth review: [[project-okta-sso-jwt-architecture]]; sibling reviews: [[project-wudas-api-doc-review-2026-10]], [[project-tudas-api-doc-review-2026-10]].

**Cross-family umbrella (2026-10-12):** unify the contract first, then generate OpenAPI; see [[project-wsd-api-unification-2026-10]].
