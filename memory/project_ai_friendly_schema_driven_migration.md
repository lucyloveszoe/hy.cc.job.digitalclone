---
name: project-ai-friendly-schema-driven-migration
description: "Kin Lum's NXG \"schema-driven\" framework unifying WSD's Feeder, Transfer, and CLEADA-stitcher pipelines"
metadata: 
  node_type: memory
  type: project
  modified: 2026-09-09T02:27:09.621Z
  originSessionId: d76cf8c9-d472-429b-98a8-48fc2c8e3550
---

Kin Lum proposed building a next-gen ("NXG") framework based on a schema-driven concept, applied across three of WSD's core data-pipeline categories, each tracked as its own numbered sub-project (all Kin Lum/Tingyu):

- **(I) Feeder & Scheduler & CI/CD** — target date 10/20/2026. Potentially covers zDB data types 1, 13, 14, 25, 26, 29, 49, 50, 51, 56, 58, 61 (Savi), 62 (Smeta). Sample feeder schema and feeder-scheduler schema published in `WSD.Zdb.Feeder` (`reinv` branch, `_doc/reference/`). Plan is to roll out gradually by file type, from least to highest production impact, monitoring each release ~1 week before moving to the next type. Progress by ww34 (08/25/2026): the **Type 61-savi/62-smeta** vehicle (see [[project-sawnics-avi-metapulse-collection]]) is complete, deployed, and released to production — the first proof the schema-driven approach works end to end (fully closed out 09/08/2026). Schema-based migration itself reached **all 12 of 12 file types** by ww35 (09/01/2026). Deployment rollout by ww36 (09/08/2026): WW35 = Types 25/26/27/49/50/51 (completed, deployed); WW36 = Types 56/58; WW37 = Types 14/29; WW38 = Types 13/1.
- **(II) Transfer & CI/CD** — target date TBD; same schema-driven concept applied to the "Transfer" category, expected to cover a wider set of data types than Feeder. No detailed status yet as of ww34.
- **(III) CLEADA Details Stitcher & CI/CD** — target date TBD; applies the same concept to the CLEADA Details Table stitcher category. As of 08/04/2026, Kin Lum was evaluating a schema-based approach to CLEADA detail-table stitching, still in progress.

**Why:** replaces WSD's per-file-type, hand-built feeder/transfer/stitcher code with one reusable, schema-driven framework — directly related to Yu Han's "no re-inventing wheels" principle (see [[feedback-engineering-principles]]) and to Ken's separate cloud-native scheduler proposal, which Yu Han explicitly flagged as needing a no-conflict check against this migration's own scheduler-schema work (see [[project-ken-cloud-native-scheduler-proposal]]).

**How to apply:** treat Type 61/62 (Sawnics AVI/Metapulse) as the reference implementation for how future file types onboard onto this framework. Update the per-type rollout schedule as each week's deployment completes.

**ww37-ww40 rollout progress (09/15-10/06/2026)**: Types 56-Tver/58-Fvi completed (WW36 batch, monitored); Types 14-WaferSummary/29-Ttd completed by 09/29; Type 13-WaferCsv completed; Type 1-ModuleCsv completed by 10/06 with one more week of monitoring before the **(I) Feeder** item is called done. Ken also deployed a WUDAS non-FBAR wafer pass-bin criteria change (per Min Yoke; t25 ncf2 pass values {0,1}→{1,81–89}, t51 nfwps {1}→{1,81–89}; ncf3/awscaoi/awscdc unchanged) via the new Zdb.Feeder schema, pending Min Yoke's buy-off. **10/06/2026: (II) Transfer & CI/CD brought into the active queue because Feeder is done**; target date still TBD. 
