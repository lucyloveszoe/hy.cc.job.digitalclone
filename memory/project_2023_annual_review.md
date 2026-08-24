---
name: project-2023-annual-review
description: "Yu Han's WSD SW team 2023 weekly-status archive (ww01-ww52) — major project completions, architecture decisions, and the team's earliest recorded GenAI touchpoint"
metadata: 
  node_type: memory
  type: project
  modified: 2026-08-22T00:32:53.143Z
  originSessionId: 60cad4c7-e6f3-4ac2-9f91-af516207847b
---

Synthesized from 47 of Yu Han's weekly "WWL" status reports to KP, covering calendar year 2023 (ww01-ww52; a few week numbers are missing/merged in the source set, e.g. ww04+05, ww20+21, ww29+30, ww48+49). Recurring team: Kin Lum, YK, Choo-Yau, Tingyu, Chao, Jiapeng, Da Wang — the same roster later seen in the 2026 reports (see [[user-role-team]]), confirming multi-year team stability.

**Security & infra hardening (Q1 2023)**: team-wide push to replace identity-based AWS access with AssumeRole + fix Cycode-flagged secrets-in-code across ~45 owned repos. Violations dropped from 9,311 (11/2022 baseline) to <500 "Critical" by 03/21/2023. STMicro AVI Map project (released 12/23/2022) served as the pilot vehicle for this pattern before rolling it out broadly. In parallel: PRDA pipeline migrated from a 2017 home-grown EC2-autoscaling framework to ECS+CI/CD (completed 03/07/2023); an urgent, EOL-driven Aurora MySQL 5.7→8.0 upgrade across Oregon+Singapore clusters completed 03/28/2023 with zero downtime/IP-change surprises. This is a distinct, earlier (Cycode/AssumeRole-based) security-hardening wave from the 2026 "Mythos" program (Claude-based scanning) — see [[project-mythos-security-scanning]] for the current one.

**Data pipeline / CM onboarding expansion**: TnR EBR records migrated off SQLite onto the Genealogy DB + Web API (released 01/10/2023). Wafer-level H2 trace query support added (04/10/2023). LGIT Genealogy DRCR report support added (02/14/2023). AWSC (new qualifying foundry) AOI/DC data path enabled (07/25/2023) as part of a broader Cloud SFTP Data Collection Pilot that completed migrating all 17 CM+type zDB categories from 8 CMs (08/04/2023).

**Dev-Proto / NPI test-data infrastructure**: SJ/Seoul Dev-Proto POC closed 03/21/2023, followed by full Development completing 06/30/2023. Key decision: picked Aurora (reusing existing MongoDB+S3+Aurora data-lake stack) over DynamoDB or DocumentDB for the Dev-Proto core DB — reuse-over-new-tech, consistent with the recurring "no re-inventing wheels" / "prefers reusing existing infra" principle in [[feedback-engineering-principles]]. Also benchmarked and picked "single large binary + sequential range access" over "many tiny S3 segment files" for data stitching performance.

**Clotho platform evolution**: V3.3.1 production rollout completed 03/28/2023 (chose gRPC over MQTT for the high-throughput real-time streaming case — MQTT was fine for ~50 msg/sec but gRPC was needed for >1K msg/sec, 100KB/msg scenarios). V4.0 (Dev-Proto support) and a V4.1.0 upgrade cycle followed later in the year. Separately, "Project Hydra" — an early next-generation, multi-node/cluster-based Clotho platform vision — was first shared internally 01/10/2023, with SOW definition targeted for Q4'FY23; this is plausibly an early conceptual ancestor of the team's later NXG platform work (see [[project-nxg-pipeline-and-raptor]]), but no explicit document link was found confirming that continuity — treat as unconfirmed.

**DPAT capability expansion**: "ParamExpansion" raised the DPAT parameter cap from 32 to 256 (extensible to 512/1024) and removed the 50-lot query restriction (released 02/17/2023). A follow-up "Per-Site Parametric Check" (to keep probing quality consistent across Quadcore's multiple test sites) completed 10/10/2023.

**Earliest recorded internal GenAI touchpoint**: on 12/07/2023, Kin Lum shared keynote takeaways from Snowflake's BUILD conference on "GenAI for Image Processing" / generative AI app development, specifically floating it as inspiration for "WSD MLOps Phase-II" (image labeling/retraining workflows, relevant to BSIR). This predates Yu Han's formal Broadcom WSD AI Champion role and the GenAI governance program (see [[project-genai-program]]) by roughly two years — useful as the earliest known seed of WSD SW team's GenAI interest, not itself part of the governed program.

**MLOps**: a BSIR ML+A2I automation POC (Kin Lum/Tingyu/Jiapeng) completed 08/02/2023; the Data Science stakeholders (Jen Seng/Rebecca) picked MLFlow over Verta by direct vote after a side-by-side platform evaluation — another instance of the "hands-on/benchmarked evaluation before deciding" principle.

**PPYCC / financial tooling**: R&D Financial Portal enhanced for FY24 AOP submission (completed 05/17/2023); a Q4'FY23 PPYCC round upgrade; and a year-end "PPYCC All Regions' Performance Improvement" covering R&D Financial + Customer Sampling scope (completed 12/18/2023, scope trimmed from full POC findings per KP's confirmation).

**Utility tooling**: a GoldenEagle/GoldenDictionary-based REST API prototype for breaking down NPI test headers (released 11/17/2023); Atropos web site improvements for DPAT simulation and wafer-regen UX (completed 12/08/2023).

**Vendor/ops**: DevCraft/Telerik license renewed as a 3-year term (~$12,029), notable as WSD's first-ever vendor to go through Progress Software's Vendor Assessment process (completed 03/07/2023).

**Why:** gives long-horizon context on the SW team's technical trajectory and recurring decision patterns (reuse-over-build, benchmark-before-decide, security-hardening-in-waves) well before the 2025-2026 material already in memory — useful for noticing when a "new" 2026 initiative is actually a second or third iteration of something the team already did in 2023.

**How to apply:** treat this as historical/background context, not current state — none of these 2023 initiatives should be assumed still active without confirming against more recent material. Don't force connections to 2025/2026 initiatives (e.g. Mythos, NXG/Raptor) beyond what's explicitly noted above as unconfirmed.
