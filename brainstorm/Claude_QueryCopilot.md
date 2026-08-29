# Query Copilot (working name) — NL-to-SQL Data Request Assistant

Status: brainstorming, no decision made yet. Continue this discussion from here next time.

## Naming note

Working name "Query Copilot" used deliberately instead of "NXG" — "NXG" is already
in use for Kin Lum's unrelated schema-driven Feeder/Transfer/CLEADA-stitcher framework
(see digital-clone memory: `project_ai_friendly_schema_driven_migration.md`). Reusing
"NXG" for this would cause confusion in team comms. Rename if a better name comes up,
but avoid "NXG."

## Problem being solved

Domain experts today raise data requests by hand-writing custom SQL against gDB, zDB,
WUDAS, TUDAS, etc. (data ultimately backed by S3/DB). Slow, requires SQL literacy,
funnels through whoever can write the SQL.

## Proposed flow (as originally stated by Yu Han)

1. Requestor describes need in natural language.
2. GenAI bot (RAG over the SW team KB — which documents zDB, WUDAS, TUDAS, TAP, gDB
   schemas/semantics) converts the NL request into a candidate SQL query.
3. Requestor reviews/confirms the SQL.
4. System dynamically generates a UI for the requestor to fill in arguments (date
   range, LotIDs, ProductTags, etc.).
5. Confirmed SQL + collected arguments run against the data-mining/calculation
   pipeline, returning results.

## Verdict: does the concept work?

Yes structurally — this is a known, mature pattern (NL2SQL / text-to-SQL copilot),
comparable to Snowflake Cortex Analyst, Databricks Genie, ThoughtSpot Sage. The
human-confirms-before-execution step is the right safety gate in principle.

**Core risk identified**: the confirmation step assumes the requestor can read SQL
well enough to catch a subtly wrong JOIN or aggregation. Most domain experts can't.
A plausible-but-wrong query produces a confident wrong number that drives a real
yield/cost decision — worse than today's slower-but-correct manual process. This is
the risk any architecture choice must defend against, more than "can the LLM parse
natural language" (it can).

**Secondary risk**: zDB/gDB/WUDAS/TUDAS are denormalized, historically-grown schemas
(zDB alone has 60+ numbered "Types"). An LLM free-generating SQL joins against raw
physical tables is prone to hallucinating relationships that don't actually exist.
RAG over docs reduces but doesn't eliminate this — docs drift from actual schema.

## Three architecture options considered

**Option A — Template library + NL parameter extraction (recommended v1)**
- Mine existing support-request history for the ~20-30 recurring query shapes (80/20).
- Humans write and review each SQL template once, not per-request.
- LLM's only job: RAG-match NL request → best template, then extract parameters
  (dates, LotIDs, ProductTags) from the NL text.
- Dynamic UI = the matched template's declared parameter schema (trivial to
  generate — no free-form SQL-to-UI inference needed).
- Unmatched/novel requests fall into today's manual queue, which also seeds new
  templates over time — library grows from real demand.
- Correctness risk near-zero (SQL itself isn't LLM-authored). Trade-off: doesn't
  handle genuinely novel one-off questions, less flashy.

**Option B — Semantic layer + guarded SQL generation (middle ground)**
- Build a curated semantic layer (documented views over zDB/gDB/WUDAS/TUDAS with
  plain-English column descriptions); RAG the KB against that layer, not raw DDL.
- LLM generates SQL only against the semantic layer — joins are pre-resolved there,
  making hallucinated relationships structurally harder.
- Confirmation UI defaults to a structured plain-English "query summary" (source,
  filters, output columns), with raw SQL as an optional advanced view for
  SQL-literate requestors.
- Add row-limit / cost guardrails and execute only against a read replica.
- **Sequencing note**: if Kin Lum's schema-driven NXG migration (see
  `project_ai_friendly_schema_driven_migration.md`) finishes formalizing zDB/gDB
  schemas as part of its Feeder rollout, this semantic layer becomes much cheaper
  to build. Worth sequencing Option B after/alongside that work rather than
  reverse-engineering schema docs independently.

**Option C — Full agentic NL2SQL with self-verification (later / v2+)**
- LLM freely generates SQL against real schema, executes in a sandbox, self-checks
  (row-count sanity, EXPLAIN cost, schema-lint) before showing a human.
- Closest to the original ask; most flexible, highest residual risk.
- Needs a golden-query regression set to catch silent drift over time — which
  Option A/B naturally produce as a byproduct (every confirmed request becomes a
  labeled example feeding this set later).

## Recommendation

Start with **Option A**. Rationale: matches Yu Han's own stated engineering
principles — 80/20 scoping, no reinventing wheels, narrow-first-then-widen (same
phasing philosophy already applied to the digital-clone project's Proposal A→B→C).
Ships fast, near-zero correctness risk, and the usage data it collects builds the
case (and the semantic-layer requirements) for Option B later. Option C is a
possible v2 once A/B have logged enough real traffic for a safety-net regression set.

## Open items / not yet decided

- Final name (avoid "NXG").
- Whether/how this sequences against Kin Lum's schema-driven NXG migration
  (Feeder/Transfer/Stitcher) — potential dependency for Option B's semantic layer.
- GTO/security review: flagged that since this generates-then-executes SQL against
  production yield/cost data, it likely needs the same kind of review path used for
  MCP server approvals — not yet raised with GTO/security.
- No v1 component sketch / build-size estimate done yet (offered, not yet requested).
- No decision yet on which option to actually pursue.
