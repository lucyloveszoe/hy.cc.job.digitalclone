---
name: project-cicd-maintenance-automation
description: "CM/Subcon Maintenance Automation by CI/CD — WSD.CICD auto-updater rolled out fleet-wide, completed 01/14/2025"
metadata:
  node_type: memory
  type: project
  originSessionId: c155a9e1-a0c1-49e4-a420-87a1f4788eeb
  modified: 2026-08-24T02:03:03.680Z
---

Built a generic CI/CD auto-upgrade mechanism (WSD.CICD, "cicdupdater") for WSD's Windows service apps (TAP Collect/Xfer, etc.) and task-scheduler apps (devProtoDaemon, lbfFeeder, etc.): each app package + version is tracked via a `cicdupdater.json` manifest in S3 (`wsd.cicd.sg` bucket), with automated deploy/rollback and heartbeat delivery reporting. Originated as the vehicle for TAP Phase 1 Collector auto-deployment (target 01/07/2025, completed on schedule), then Tingyu led a full fleet rollout: assigning every server/app/service combo across all CMs/Subcons/BRCM on-premise sites a manifest and owner. Completed 01/14/2025 — roughly 100 server/app/service deployments migrated to CI/CD auto-maintenance, tracked in a master Google Sheet.

**Why:** replaces ad-hoc manual patching across WSD's CM/Subcon fleet with a standard, auditable auto-upgrade/rollback pipeline — foundational infrastructure other initiatives (e.g. LBF feeder, TAP Collector updates) now build on.

**How to apply:** when a future request involves deploying or upgrading a Windows service/task-scheduler app at a CM/Subcon or on-premise site, this CI/CD auto-updater is the standard mechanism — check for an existing `cicdupdater.json` entry before proposing a new deployment approach.
