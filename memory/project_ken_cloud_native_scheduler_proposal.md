---
name: project-ken-cloud-native-scheduler-proposal
description: "Ken's proposal for a cloud-native task scheduler to replace/unify WSD's scattered scheduling mechanisms"
metadata: 
  node_type: memory
  type: project
  modified: 2026-08-25T20:30:49.217Z
  originSessionId: d76cf8c9-d472-429b-98a8-48fc2c8e3550
---

Ken proposed a cloud-native scheduler for WSD's data infrastructure (initial proposal shared 08/25/2026, SW Team review status In Progress). Yu Han asked the whole team to comment before any decision, for two explicit reasons: (a) scheduler mechanisms are already widely used across WSD's data infra — ATA, transfer, feeder, Spotfire, and RT alarms all have their own scheduling logic today — so a cloud-native replacement is a cross-cutting change, not a point fix; (b) scheduling is itself one of the topics the "AI-Friendly Schema-Driven Migration" effort (see [[project-ai-friendly-schema-driven-migration]]) is already touching, so the two efforts must be checked for conflict before either proceeds.

**Why:** a scheduler touches nearly every existing data pipeline component at WSD, so a redesign proposal carries unusually broad blast radius — worth a full-team review rather than a quick approval, and worth explicitly cross-checking against the parallel schema-driven migration work for overlap.

**How to apply:** before endorsing or building against Ken's scheduler proposal, confirm it doesn't duplicate or conflict with the schema-driven migration's own scheduler-schema work (`feeder-scheduler-schema.md` in `WSD.Zdb.Feeder`). Update this file once the team's review lands on a decision.
