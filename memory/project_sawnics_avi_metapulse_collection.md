---
name: project-sawnics-avi-metapulse-collection
description: "Enabling Sawnics \"AVI and Metapulse\" data (new zDB Types 61/62) collection, parsing, and WUDAS query access"
metadata: 
  node_type: memory
  type: project
  modified: 2026-08-25T20:31:11.928Z
  originSessionId: d76cf8c9-d472-429b-98a8-48fc2c8e3550
---

Tingyu/Kin Lum are enabling collection and access for two new Sawnics data types — AVI and Metapulse — per two Statements of Work from Vikram, target date 09/11/2026. New zDB Collection Types: 61 (.savi) and 62 (.smeta).

**Schedule and progress**: planned in sprints — enable .savi/.smeta file collection from Sawnics (WW32), enable parsing (WW33), enable WUDAS data queries (WW35). Status as of 08/25/2026 (ww34): zDB transfer to Sawnics for .savi/.smeta collection released and deployed (completed 08/11/2026); WW33 parsing work integrated .savi/.smeta into the schema-based feeder (completed) — including bit-binary encoding for .savi's true(1)/false(0) custom parameter values, a unified One-Scheduler code path for all types under the schema-based approach, and Jenkins CI/CD plus scheduler/worker test runs; WW34 CLEADA integration (Tingyu) is development-done and in testing.

This is the **first production vehicle** for the broader "AI-Friendly Schema-Driven Migration (I)" feeder framework (see [[project-ai-friendly-schema-driven-migration]]) — its successful Type 61/62 rollout is what the wider schema-driven feeder migration is now being modeled on.

**Why:** extends WSD's zDB data-collection coverage to a new CM (Sawnics) data category while simultaneously proving out the new schema-driven feeder architecture in production.

**How to apply:** treat this as the reference case for how a new file type onboards onto the schema-driven feeder framework. Update as WUDAS query access (WW35 target) completes.
