---
name: project-ai-friendly-schema-driven-migration
description: "Kin Lum's NXG \"schema-driven\" framework unifying WSD's Feeder, Transfer, and CLEADA-stitcher pipelines"
metadata: 
  node_type: memory
  type: project
  modified: 2026-08-25T20:31:01.578Z
  originSessionId: d76cf8c9-d472-429b-98a8-48fc2c8e3550
---

Kin Lum proposed building a next-gen ("NXG") framework based on a schema-driven concept, applied across three of WSD's core data-pipeline categories, each tracked as its own numbered sub-project (all Kin Lum/Tingyu):

- **(I) Feeder & Scheduler & CI/CD** — target date 10/20/2026. Potentially covers zDB data types 1, 13, 14, 25, 26, 29, 49, 50, 51, 56, 58, 61 (Savi), 62 (Smeta). Sample feeder schema and feeder-scheduler schema published in `WSD.Zdb.Feeder` (`reinv` branch, `_doc/reference/`). Plan is to roll out gradually by file type, from least to highest production impact, monitoring each release ~1 week before moving to the next type. Progress by ww34 (08/25/2026): the **Type 61-savi/62-smeta** vehicle (see [[project-sawnics-avi-metapulse-collection]]) is complete, deployed, and released to production — the first proof the schema-driven approach works end to end. Remaining rollout schedule (11 of 12 file types migrated so far): WW35 = Types 25/26/27/49/50/51; WW36 = Types 56/58; WW37 = Types 14/29; WW38 = Types 13/1.
- **(II) Transfer & CI/CD** — target date TBD; same schema-driven concept applied to the "Transfer" category, expected to cover a wider set of data types than Feeder. No detailed status yet as of ww34.
- **(III) CLEADA Details Stitcher & CI/CD** — target date TBD; applies the same concept to the CLEADA Details Table stitcher category. As of 08/04/2026, Kin Lum was evaluating a schema-based approach to CLEADA detail-table stitching, still in progress.

**Why:** replaces WSD's per-file-type, hand-built feeder/transfer/stitcher code with one reusable, schema-driven framework — directly related to Yu Han's "no re-inventing wheels" principle (see [[feedback-engineering-principles]]) and to Ken's separate cloud-native scheduler proposal, which Yu Han explicitly flagged as needing a no-conflict check against this migration's own scheduler-schema work (see [[project-ken-cloud-native-scheduler-proposal]]).

**How to apply:** treat Type 61/62 (Sawnics AVI/Metapulse) as the reference implementation for how future file types onboard onto this framework. Update the per-type rollout schedule as each week's deployment completes.
