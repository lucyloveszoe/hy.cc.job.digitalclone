---
name: project-cleada-autoscaling-cluster
description: "CLEADA computation auto-scaling cluster adjustment — added a reserved 'L' cluster for high-volume service accounts so they stop competing with personal-account queries"
metadata:
  node_type: memory
  type: project
  originSessionId: 59aa9cac-0722-4254-85a2-83d236cbea48
  modified: 2026-09-08T07:03:51.408Z
---

**Problem (raised by 12/02/2025)**: CLEADA's standard "M-category" cluster (5x EC2 xlarge Linux, 4 workers/EC2) treats all requesters equally, but a growing set of service accounts (`wsdnpiservice@broadcom.net`, `fbar-pg.wsd@broadcom.com`, `wsdspasga`) generate much heavier/higher-volume workload than personal accounts — one or more heavy service-account requests landing on the same EC2 as personal-account queries caused performance downgrade for everyone. Separately flagged but not yet acted on: whether the generic service accounts' ID.pass expiration (currently 6 months) could be extended to reduce maintenance/breakdown risk.

**Fix (Ken/Kin Lum, with YK/Choo-Yau in the loop)**: stood up a reserved **"L" category** auto-scaling cluster (up to 10x EC2 xlarge Linux, 2 workers/EC2) dedicated to service accounts, plus a rule-based request router (checked top-to-bottom, first match wins) so `NDetail`/`NDetailSlim` queries route to a separate medium-TK ("mtk") cluster, the named service accounts route to the new L cluster, and everything else falls back to M. Code reviewed via `WSD.Cleada` PR #238, deployed to production 12/08/2025 (including adding `wsdspasga`, the Spotfire-automation service account, into the L cluster to resolve a separate SG DC Spotfire automation query-slowness complaint).

**Marked Complete (12/16/2025)**: after a 1-week clean evaluation window, "task duration over time" was smoothed to normal scale even while absorbing the previous high spikes of requests/minute.

**Why:** a concrete instance of the recurring "service/automation accounts need separate resource lanes from personal accounts" pattern on shared compute infra — worth checking for if a similar complaint (one heavy consumer degrading a shared pool) surfaces on another CLEADA-adjacent or similarly-shared WSD service.

**How to apply:** treat as closed infra work, not an open investigation. If a new service account needs onboarding to the L cluster, or the ID.pass-expiration-extension idea gets picked up later, update this file rather than creating a new one.
