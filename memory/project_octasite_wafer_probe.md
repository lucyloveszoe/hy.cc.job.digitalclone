---
name: project-octasite-wafer-probe
description: "Early-stage initiative to speed up wafer trace-data collection/processing to match new Octa-Site wafer-probe hardware"
metadata:
  node_type: memory
  type: project
  originSessionId: c155a9e1-a0c1-49e4-a420-87a1f4788eeb
  modified: 2026-08-24T06:09:44.373Z
---

**Initiated 09/05/2025** by KP: TDG is improving wafer-probe hardware (e.g. "Octa-Site" — testing 8 sites at once), which will cut wafer test time from ~4-5 hours down to ~1 hour. Today's data pipeline takes ~10 minutes to collect trace data plus cloud processing, giving >30 min latency before data is available in WUDAS — too slow relative to the faster test cycle; KP wants full wafer data collection (including trace data) down to well under the new test time, possibly <3 minutes, and floated writing wafer results directly to file/stream instead of the current pipeline. Yu Han is running the initial sync.

**09/09/2025 update**: Chung Sik framed this as a broader opportunity — with TDG improving wafer-probe hardware generally (Octa-Site being one example), WSD should look into refining the whole data transfer/processing method, not just patch around this one hardware change, to see if there's a way to improve either data-availability speed or unlock new capabilities.

**Three-area improvement plan with owners assigned (09/09/2025, still Pending TDG/PE clarification as of 09/30/2025)**: Yu Han proposed 3 concrete areas, each with a follow-up sync owner — (1) **Data Collection** (owner Tingyu): switch from today's single-zip-per-wafer collection (60K dies, ~5GB, collected only at the very end) to small, high-frequency batch zips (e.g. 50MB/~5 dies each, collected as soon as accumulated) — same total data, much lower tail latency; (2) **Cloud Processing** (owner Kin Lum): have WUDAS/zDB process each die's trace file as soon as its small zip lands in S3, independent of the final CSV, so only the last CSV needs processing at completion time, not the whole 60K-die batch; (3) **Local Post-Processing** (no owner yet): replace local-tester-searches-and-grabs-history-CSV-then-runs-logic with an API-based edge-compute node — call API, poll for result, push straight to zDB — with a bonus that this would also simplify pCD-V/Clotho V7's wafer post-processing story (see [[project-pcd-clotho-v7]]). As of 09/30/2025 all three are pending TDG/PE clarification before SW Team commits to concrete action items.

**Why:** a hardware-driven change (faster wafer probing) that could force a redesign of WSD's wafer trace-data collection/cloud-processing pipeline if latency can't be cut fast enough to keep up.

**How to apply:** still at the initial-sync stage, no scope/target date set yet — update this file once a concrete SOW or design proposal emerges. Related but distinct from [[project-nxg-pipeline-and-raptor]] and the wafer-side work in [[project-pcd-clotho-v7]] — check both if this initiative's scope starts overlapping with either.
