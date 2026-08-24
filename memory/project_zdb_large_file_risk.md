---
name: project-zdb-large-file-risk
description: Recurring zDB large-file (4-12GB) data-collection strain/integrity risk and proposed mitigations
metadata: 
  node_type: memory
  type: project
  originSessionId: b57dbea7-e0ff-4ed2-a55d-4625560a26cf
  modified: 2026-08-24T06:28:40.432Z
---

Recurring, unresolved risk across weekly reports ww01–ww10 (2026): large zip/data files (.wbsi/.wavi, 4–12GB) in zDB strain collection bandwidth and integrity, causing memory spikes up to ~14GB during processing. Engineering investigated streaming JSON parsing, MemoryMappedFile, and SQLite as candidate mitigations (Yu Han required side-by-side testing on the same dataset before deciding — see [[feedback-engineering-principles]]).

Yu Han's proposed mitigation, now stated in concrete form (reconfirmed ww24-32): a hard CAP enforced at the zDB RTDC collector to reject out-of-range files outright, plus splitting Production vs. NPI file-type IDs using a fixed "+10 offset" numbering convention (e.g. Production type 53 → NPI type 63), so each can get separate size caps/monitoring. Still pending PE response as of ww32 (Aug 2026) — unresolved for many months running.

**Origin traced to Oct 2025**: this exact proposal — hard CAP at the zDB RTDC collector plus a Production/NPI file-type split via a fixed offset convention — was first raised by Yu Han on 10/28/2025, surfaced while investigating "unexpected out-of-range files" during Ken's File Access API enhancement work (Type 23 .fmtrace, 37 .bsi, 53 .wbsi, 57 .wavi; Type 53 .wbsi files in the 4-12GB range were the acute pain point). At origin, the offset convention was framed as "Production WBSI stays Type 53, NPI WBSINPI becomes Type 60 or 63, always keep offset 10 between production vs NPI." Yu Han also proposed backend+client-side caching to avoid repeated computation/bandwidth waste on the same large files. Chung Sik (NPI/PE side) was separately investigating file-size reduction via image compression as a complementary angle. Confirms this is the same unresolved proposal still pending PE as of ww32 (2026) above, not a new one.

**Related but distinct sub-project: "Zip File Access API Enhancement" reaches formal SOW stage (10/29-11/11/2025)**: Ken's partial-zip-extraction API (originally Chung Sik's 10/02/2025 request, the one Yu Han's CAP-restriction principle in [[feedback-engineering-principles]] was applied to) is now a named project with **target date 12/04/2025**. Confirmed technical finding: S3 metadata-retrieval latency for even the largest file types (19/23/53) tested under 1 second across ~90 sample files, so no pre-processing/indexing layer is needed. Chung Sik bought off the SOW/SRS with API signature on 11/11/2025; only workload estimate and schedule remain. Treat this as the concrete API-design counterpart to the RTDC-collector-CAP proposal above — same root problem (large zip files straining zDB), two different fixes at two different layers (collection-time rejection vs. read-time partial extraction).

**Zip File Access API development complete, trial released (12/02/2025)**: target date slipped to 12/11/2025 (from 12/04/2025). All planned dev work finished — DB schema, ECS scheduler/worker, Jenkins CICD, parallel-build performance improvement, new `zdbdownloadersecure` endpoints with a rate limiter (max 10 calls/30s per user) — and a trial version was released to Chung-Sik for testing/buy-off.

**Target date slipped again to 12/23/2025 (12/16/2025)**: still pending Chung-Sik's buy-off on the trial version; documentation still in progress. Treat as functionally done, just waiting on sign-off and docs.

**Zip File Access API Enhancement marked Complete (12/23/2025)**: Chung Sik bought off; external API docs and internal SDD/SRS published; the SW team web portal updated with the public reference page (`sites.google.com/broadcom.com/zdb/file-access-api/zipscope`). Treat this sub-project as closed — the RTDC-collector-CAP proposal (the other half of the large-zip-file problem) remains open/pending PE response.

**Why:** a live reliability/scalability risk on a core data pipeline, tracked over multiple months rather than a one-off ticket.

**How to apply:** update this file when the cap/split proposal is accepted, rejected, or superseded.
