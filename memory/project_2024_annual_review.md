---
name: project-2024-annual-review
description: "Yu Han's WSD SW team 2024 weekly-status archive (ww01-ww52) — Cloud SFTP migration closure, MLOps Phase 1 GA, GTO Jira boarding, and pointers to the year's 3 major new initiatives"
metadata:
  node_type: memory
  type: project
  modified: 2026-08-22T00:51:29.386Z
  originSessionId: 60cad4c7-e6f3-4ac2-9f91-af516207847b
---

Synthesized from 47 of Yu Han's weekly "WWL" status reports to KP, covering calendar year 2024 (ww01-ww52; some week numbers merged in the source set, e.g. ww01+02, ww26+27, ww48+49, ww50+51). Recurring team unchanged from 2023 (see [[project-2023-annual-review]] and [[user-role-team]]) plus Ken Sun, who completed onboarding this year.

**Three major new/renamed initiatives from 2024 have their own detailed memory files** — see [[project-radar-jira-service-manager]] (Raptor↔GTO-Jira sync, Phase I shipped and stabilized this year), [[project-clotho-nxg-magnus-roadmap]] (DEC2/Kafka/Gen-QS investigation and the "Magnus"/pCD-IV next-gen-Clotho effort, stalled mid-year), and [[project-wsd-logistics-data-integrity]] (renamed "IDI" — Wingra's cross-tool design-data integration project, Phase 1 shipped, later phases on hold).

**FeederDB → Snowflake migration**: this year's execution of the migration flagged as a blocker in [[project-nxg-pipeline-and-raptor]] — see that file for the 3-phase rollout and the Hybrid-vs-Standard-table architecture decision.

**Cloud SFTP Data Collection Migration for 14 CMs / 140+ data types**: the main migration (started 2023, see [[project-2023-annual-review]]) formally closed 01/26/2024, with leftover CM stragglers (STM, TDK, Kinsus, LGIT) pushed into ongoing "continual support" that ran for most of the year as each CM slowly completed SFTP cutover and file-matching verification.

**MLOps BSIR Phase 1**: officially released to production 01/08/2024 (Google Site published), closing out the 2023-started POC (see [[project-2023-annual-review]]). Phase II (A2I labelling/retrain automation) remained a "Middle Term In-Queue" item, not started, through year-end.

**Non-FBAR Wafer Sampling @ Chipbond**: Ken Sun's onboarding/training project (started as 2023 New-Hire training vehicle), officially released 02/02/2024.

**GTO Jira boarding**: the SW team's adoption of Broadcom GTO Jira (vs. WSD's own Jira) as its requirements/priority coordination tool — initiated per KP's FY24-goal recommendation, piloted via the WSD Radar-2-Jira project, formally closed as a setup effort 02/06/2024 (see [[reference-wsd-systems]] for the JIRA reference entry).

**Recurring backlog items that never moved all year**: "Pushing QRE data into zDB," "Dynamic Wafer Radius Processing," and "MIPI Translation to Infer Module States in Test" appear as "(Initiated)" in the New-Requests-Investigation backlog in essentially every single 2024 weekly report without ever advancing — a useful signal that this particular backlog bucket is effectively frozen/low-priority rather than actively worked.

**Inari P34 (new CM sub-site) zDB/pCD accessibility**: WSD extended zDB/pCD/Workflow infra to a new CM sub-site (Inari P34) in 2024 (Phase 2 dedicated-server approach preferred over a temporary P3-linked Phase 1) — a smaller-scale precedent for the kind of new-CM-site infra rollout later repeated at much larger scale in [[project-amkor-vietnam-expansion]] (2026). Last recorded activity ~08/20/2024; no confirmed closure date found in the material reviewed.

**Why:** gives 2024-specific continuity context between the 2023 archive and the 2025-2026 material already in memory — useful for recognizing when a 2026 initiative is a continuation, renaming, or repeat of 2024 work (especially the Clotho/DEC2/Magnus → NXG lineage and the FeederDB/Snowflake → NXG-pipeline-unblock lineage).

**How to apply:** treat this as historical/background context; don't assume anything described here as "in progress" is still active without checking more recent material. This 47-file 2024 batch was processed via targeted grep-sampling (quarterly/year-end checkpoint reads plus keyword sweeps for new-initiative markers) rather than a full sequential read of every week, given the reports' heavy week-over-week content duplication — so week-by-week granularity for routine/small items was not preserved, only durable decisions, completions, and new initiatives.
