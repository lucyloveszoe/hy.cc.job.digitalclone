---
name: project-file-access-api-unification-2026-10
description: Review of the 4 File Access API doc families (FileQuery, CNTrace Query, ZipScope, dual-token/Safelock) with per-sub-set improvement points and a layered unification approach; where it corrects/extends earlier memory
metadata:
  type: project
---

Reviewed 2026-10-12 from `cc.brcm/WSD.Artifact.Rig.SW.InfraAPIImprovement.Research/family/file-access/` (docx text-extracted, PDFs read; live API not called, server code not seen, Box/Google Doc links in `urls.txt` not opened). Full write-up: `file-access-api-improvement-and-unification-proposal.md` there, building on `file-access-api-unification-findings.md` (2026-10-11). Proposal only, nothing here is decided or scheduled. Siblings: [[project-wudas-api-doc-review-2026-10]], [[project-tudas-api-doc-review-2026-10]].

**What the docs say (verified from source):** FileQuery `/filequery` (type + <=14-day window, MySQL regex on name) and CNTrace `/cntracequery` (product + lot/PIDs, whole lot via `Pid:[]` added 02/22/2023, or file names) both take a quoted single-quote-JSON string and return `filename,url` text; ZipScope trio (Ken Sun, doc 2025/11/26, updated 2026/03/02) is real JSON with `taskid`, per-file `StatusCode`, limits 15 files / 2 MB / 25% raw-zip fallback / 10 req per 30 s; dual-token doc (2024) covers ID.Pass (6 months) -> access token (1 h) at `safelock.wsd.sgn`, downloads at `safelock.wsd.lvn`, base `http://10.202.128.101/zdbdownloadersecure/...`.

**Corrections/extensions to earlier memory and to the 10/11 findings:**
- Legacy endpoints were shut down 05/19/2025 ([[project-okta-sso-jwt-architecture]]), so the FileQuery/CNTrace PDFs are probably stale; the "unauthenticated legacy" finding needs a verify. If dead, migration reduces to retiring stale docs.
- `sgn`/`lvn` Safelock hosts are a regional pair (both stood up 01/21/2025), not an anomaly; the gap is that docs don't say the token works on both.
- `10.202.128.101` is private; the public `54.169.174.1` appears only in the legacy FileQuery PDF.
- Newer ZipScope doc already omits `Email` on FileQuery; only the 2024 dual-token doc still sends it.
- Yu Han's 10/21/2025 caps for ZipScope (<=15 zips, <=3000 entries, <=3 GB, <=1 call/3 s or 20/60 s; [[feedback-engineering-principles]]) are not all in the shipped doc: verify enforcement.
- Backends likely differ today (Aurora MySQL for FileQuery; cntrace via Mongo/Aurora JSON + 2022 bin-merge, CNTRACE download API by Jiapeng shipped 05/31/2022, see [[project-2022-partial-review]]): inference, not verified.

**Proposal themes (ranked, same tiers as the WUDAS/TUDAS reviews):**
1. Verify + docs-only: fix ZipScope doc defects (response pasted as request example, mismatched regex example, `FilePatterns` naming, counter/case drift, 429 vs `code:"400"`), document time-window field/timezone, regex dialect, link/output lifetimes (`cleada.sg/14d/`), real limits; replace broken samples.
2. Additive: accept real JSON next to the quoted form (lenient unwrap), `Retry-After`/`X-RateLimit-*`, typed per-file statuses (`NO_MATCH`, `RAW_ZIP` fallback), `GET /filetypes` registry, OpenAPI, file-access module in the existing `wsddatawudas`/`WSD.Data.WUDAS` SDKs (no new library), caller logging.
3. v2 facade: `files/search`, `files/extract`, `tasks/{id}` over existing logic, cursor paging, async large lot queries, built on current LTS .NET behind a named HTTPS gateway stage (.NET 8 EOL Nov 2026; WCF `.svc` has no first-party .NET 8 path).
4. Bundle into NXG cycle: file catalog populated at ingest (Transfer migration item II), type registry from the schema (also absorbs the pending Production/NPI "+10" type split from [[project-zdb-large-file-risk]]), one async task envelope across WUDAS/TUDAS/File Access, Copilot tool schema from the same spec ([[project-wsd-data-access-copilot]]).

**Reasoning (short):** docs/contract fixes have no regression risk and hit every user; the selector model is the only real fragmentation, so it waits for the backend answer instead of a router guess; type registry and task envelope are cross-family, so they follow the upgrade-cycle principle rather than piecemeal patches.

**Why:** File Access API is the only read path for raw zDB files (bsi/wbsi/wavi/fmtrace/cntrace) and is the API-side half of the large-zip problem; four doc sources have drifted.

**How to apply:** treat as a backlog source; confirm with Yu Han which tier to pursue before opening Jira items. Open questions (12) are at the end of the full write-up: legacy status, `Email`/`cntracequery` on secure path, time-window field, cap enforcement, deterministic taskids, shared store, token portability sgn/lvn, gateway topology, ACL filtering of listings, retention, authoritative FileType list, caller inventory.

**Cross-family umbrella (2026-10-12):** unify the contract first, then generate OpenAPI; see [[project-wsd-api-unification-2026-10]].
