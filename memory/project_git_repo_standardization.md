---
name: project-git-repo-standardization
description: Yu Han-led effort to standardize GitHub repo naming/ownership/branch practices after Mythos scan findings
metadata: 
  node_type: memory
  type: project
  originSessionId: b57dbea7-e0ff-4ed2-a55d-4625560a26cf
  modified: 2026-08-25T20:29:05.192Z
---

Triggered directly by [[project-mythos-security-scanning]]'s non-SW-owned-repo scan, which found WSD's broader (non-SW-team) repo population ungoverned: no naming convention, no owner/team metadata, inconsistent default branches.

Yu Han is moving GitHub org ownership from the person who previously only handled onboarding (Chao) to a newly appointed "GitHub master" role, and is defining enforced naming/branch/ownership standards (including a specific "WSD GitHub Repo Naming Convention," ww33) for repos going forward. WSD runs its own GitHub Enterprise instance, separate from GTO's, plus GTO JFrog for ML dataset storage.

**Concrete issues found (as observed during Mythos non-SW scanning, ww34)**: non-standard free-style repo naming; missing communication channel/organization context (hard to reach the right repo owner); repos that are empty, binary-only, or data-only with no code; "default" branch not reliably holding the latest official release content.

**Draft proposal shared for internal review (08/25/2026)**: Yu Han circulated a draft "WSD GitHub Repo Naming Convention" (Box folder). Still pending: (1) a PDL for "WSD Git owners only" to broadcast the standard once finalized; (2) building an automated enforcement check into GitHub repo creation — before turning that on, Yu Han must confirm with the MLOps and pCD-V owners that it won't block their existing automatic repo-generation mechanisms. Planned enforcement scope also includes: capturing owner/team relationship metadata into WSD's own DB, enforcing a "default branch = latest content" practice, and flagging non-code (data-only/binary-only) repos by name.

**Why:** repo governance had drifted with no owner accountable for standards, which surfaced as a real problem once the Mythos security scan needed to know who owned/could fix what.

**How to apply:** treat this as an ongoing internal governance/ops initiative, distinct from the Mythos security-finding remediation itself (though the two are related). Update as the "GitHub master" role and standards are formalized.
