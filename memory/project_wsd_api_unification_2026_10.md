---
name: project-wsd-api-unification-2026-10
description: Cross-family synthesis of the 8 WSD API reviews: unify the underlying contract first (9 rules, 4 resource types, shared service kit), then generate OpenAPI as the exposure layer for people, code and GenAI agents; decisions U1-U9 proposed, nothing decided
metadata:
  type: project
---

Written 2026-10-12 in `cc.brcm/WSD.Artifact.Rig.SW.InfraAPIImprovement.Research/unification.md` (+ `unification.pdf`, 8 pages, built with headless Chrome for the family owners; **not sent**). Synthesises the eight family reviews: [[project-wudas-api-doc-review-2026-10]], [[project-dpat-outlier-access-api-review-2026-10]], [[project-file-access-api-unification-2026-10]], [[project-gdb-ws-api-doc-review-2026-10]], [[project-oee-report-api-review-2026-10]], [[project-tudas-api-doc-review-2026-10]], [[project-diy-api-doc-review-2026-10]], and the short-life token review (in [[project-okta-sso-jwt-architecture]]). Docs only; no live API or code seen. Proposal only, nothing decided or scheduled.

**Yu Han's framing (directive, 2026-10-12):** OpenAPI only *describes*; it does nothing for real unification. The underlying APIs must be changed to a unified contract, then OpenAPI exposes it to humans, code and GenAI agents. An earlier draft answer that treated OpenAPI as the unifier was corrected on this point.

**Proposal:** (1) nine-rule unified contract: HTTPS+DNS behind gateway with one ID.Pass->access-token flow and scopes; one async model (`202`+`Location`, `GET /v1/tasks/{id}`); real HTTP codes + problem+json; JSON/array/UTC half-open/cursor paging, no state-changing or bodied GET; JSON default with CSV/Parquet via `Accept` and a truncation signal; discovery (`/schema`, `/validate`, freshness); published limits; mutations scoped+audited; `/v1` with legacy adapters and Sunset. (2) Four resource types: dataset query, file access, tasks, mutations (+ token service). (3) One shared service kit on .NET 10 (`WSD.AspNetCoreReact` template) plus per-family descriptors. (4) OpenAPI generated code-first from the kit; 3.0.3 likely (API Gateway REST import supports 3.0 not 3.1, **[verify]**), Spectral lint, Arazzo for submit-poll-download, no AsyncAPI needed.

**Cross-family gaps found (same in most families):** plain HTTP to `10.202.128.x` (gDB, OEE, File Access, TUDAS Data Function, DIY stitch); four async dialects; HTTP-looking body codes with other meanings; quoted/bare/raw-SQL bodies, GET with body, state-changing GET; CSV with no types or truncation signal; no discovery, freshness or published limits; doc drift (Box PDF vs DocFx vs docx vs Google Doc). Fit of today's API to OpenAPI: good = token, TUDAS, DPAT, WUDAS, DIY REST; fair = DIY `.svc`, File Access; awkward = gDB (62 table-specific args), OEE (`FileType` query switch).

**Phasing:** Tier 0 verify (OEE auth? DIY `secret` and `default`-stage Lambda? legacy paths alive? call logs) -> Tier 1 docs/transport/.NET 10 (gDB before 2026-11-10) -> Tier 2 shared kit + descriptors (WUDAS and DPAT first) -> Tier 3 unified `/v1` + generated OpenAPI/SDK/agent tools -> Tier 4 breaking cleanup batched with AuthHub/NXG cycle.

**Decisions proposed (U1-U9, none made):** adopt contract; shared kit; WUDAS as reference with DPAT; generated spec; 3.0.3; legacy adapters then Sunset; shared CSV column descriptor; read/write scopes with agent tools read-only and GTO gate; name an owner for the kit.

**How to apply:** treat as the umbrella over the per-family reviews. Do not propose writing specs over current APIs as "unification"; propose contract change first. Next when Yu Han picks it up: send `unification.pdf` to the owners, collect section-8 answers and per-family Tier 0 answers, agree U1-U9, then open Jira items.
