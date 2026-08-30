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

**Third risk, found via digital-clone memory cross-reference (2026-08-29) — the
platform itself may not support this yet**: WSD already ran into this exact problem.
**Die Inventory RAG** (OneTrust case #27580, Foo-Kit) was blocked because VCF's
PAIS+DSM RAG platform only supports standard document RAG (PDF/HTML/DOCX/CSV) — **no
SQL/DB-agent capability**. Its resolution was never recorded in later status reports
(see `project_genai_program.md`) — unknown whether it was dropped, found another
platform, or is still stuck. Query Copilot needs exactly the capability that already
blocked a prior WSD project on WSD's own approved on-prem AI infra. This reframes the
"GTO/security review" open item below from "not yet raised" to "check with Foo-Kit
what actually happened to Die Inventory RAG first" — that answer determines whether
the platform gap has since been solved.

Separately, GTO's standing **internal-vs-external routing rule** (see
`project_genai_program.md`) requires internal-facing AI agents to run on **on-prem
infra only** — cloud is explicitly closed off for internal use cases, and WSD has been
told not to treat cloud as a workaround. Query Copilot is squarely internal (domain
experts querying internal production yield/cost data), so any LLM inference in this
design must go through Vectara's GTO-approved on-prem models (gpt-oss-120b,
gemma-4-31b, etc.), not a cloud model like Claude/Bedrock.

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

**Direction confirmed (2026-08-29): RAG/template-based generation + sandbox trial-run,
not fine-tuning.**
- Rejected fine-tuning a model on WSD's schema/API knowledge as the v1 generation
  approach: the schema is actively moving (NXG Feeder rollout through ~ww38,
  Transfer/Stitcher undated), fine-tuned weights go stale every time it changes,
  there's no training corpus yet, RAG/context-injection is the established better
  tool for factual/structural recall anyway, and on-prem fine-tuning's GTO governance
  status is unresolved (WSD's own "Rich's team" fine-tune concept was still "in
  concept eval" with no recorded outcome). Fine-tuning stays a possible v3+, once a
  real confirmed-query corpus exists and NXG has landed.
- **Confirmation step upgraded**: instead of (or in addition to) "requestor reads the
  SQL," give the requestor a **sandboxed trial run** — capped rows/size/frequency,
  executed against a read replica — so they can eyeball actual sample result rows
  before trusting the number. This is a stronger safety gate than SQL literacy, since
  most domain experts can spot "this returned 50,000 rows for 10 lots" far more
  reliably than they can read a JOIN clause. WSD already has a working precedent for
  this exact guardrail shape: the Zip File Access API's rate limiter (max 10 calls/30s
  per user) and the proposed zDB RTDC hard-CAP (see `project_zdb_large_file_risk.md`).
  Design requirement: the sandbox UI must surface sample rows/row counts, not just a
  final aggregate number.
- **API-signature-first refinement**: where an existing validated API already covers
  an intent (e.g. the Zip File Access API), prefer generating an API call over a raw
  SQL template — reuses validated joins/business logic, zero hallucinated-join risk,
  consistent with "no reinventing wheels." Today WSD's API surface over
  zDB/gDB/WUDAS/TUDAS is narrow/task-specific, so this only covers a slice — see the
  API-consolidation idea below for closing that gap.

**Concrete next actions (2026-08-29)**
- **Build a 1000+ example query corpus** mapped to real human needs, sourced from
  real-world queries (updated target 2026-08-30, up from the original 100+ — scope of
  "real-world" vs. "historical support-request tickets only" not yet confirmed with
  Yu Han, see flag below) — start immediately, zero dependencies (no GTO approval, no
  platform work needed). Does
  triple duty: it's the Option A template library, it's the golden-query regression
  set Option C will eventually need, and it's the training corpus fine-tuning would
  need if that path is ever revisited later.
- **Build the Asset Menu** (Library Inventory + API Catalog) — see dedicated section
  below. This generalizes/replaces a narrower "consolidate APIs into one read-API
  per zDB type" idea: the menu covers both existing APIs and existing libraries, not
  APIs alone.
  - *Naming*: avoid naming this "NXG-\<anything\>" — "NXG" is already overloaded at
    WSD (Kin Lum's schema-driven Feeder/Transfer/Stitcher framework, the 2024 "NXG
    WSD Cloud Data Pipeline" simplification effort, and a separately-referenced "NXG
    TUDAS upgrade" cycle TUDAS's own SQLite migration is bundled into). A fourth
    "NXG" meaning would add confusion, not remove it.
  - *Sequencing idea*: for the zDB-type-specific slice of the API side, riding on
    Kin Lum's Feeder rollout's existing per-type weekly schedule (Types 61/62 done;
    ww35 = 25/26/27/49/50/51; ww36 = 56/58; ww37 = 14/29; ww38 = 13/1) avoids waiting
    on the undated Transfer/Stitcher tracks.

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
- **Sequencing risk, found 2026-08-29**: this is a longer wait than it first looks.
  Feeder rollout only finishes ~ww38 (mid-Sept 2026), and the migration's Transfer
  and CLEADA-Stitcher tracks are still **TBD, no target date set**. If Query Copilot
  needs joins beyond Feeder-covered zDB types, Option B is waiting on undated work.
  Separately, the people who'd actually build this semantic layer — Kin Lum and
  Tingyu — are the same two already mid-rollout on NXG Feeder *and* owning the
  Vectara/G-AI Portal production agents; there's no obvious free bandwidth for
  Option B before ww38 at the earliest.

**Option C — Full agentic NL2SQL with self-verification (later / v2+)**
- LLM freely generates SQL against real schema, executes in a sandbox, self-checks
  (row-count sanity, EXPLAIN cost, schema-lint) before showing a human.
- Closest to the original ask; most flexible, highest residual risk.
- Needs a golden-query regression set to catch silent drift over time — which
  Option A/B naturally produce as a byproduct (every confirmed request becomes a
  labeled example feeding this set later).
- **Possible future execution layer, found 2026-08-29**: Vectara (RAG, no agentic
  execution) and Maverick (GTO's multi-agent orchestration platform, production live
  08/17/2026, "agent-chain processing" workflows, but explicitly **no RAG support**
  as of mid-2026) look like mirror-image gaps. A plausible v2 shape nobody's written
  down yet: Vectara handles NL→template/semantic matching, Maverick handles the
  confirm→execute→return step — once Maverick matures past its current Die
  Inventory Agent pilot (see `project_maverick_agentic_platform.md`). Not a v1
  dependency; worth tracking as Maverick evolves.

## Asset Menu: Library Inventory + API Catalog (consolidation umbrella, 2026-08-29)

Broadens the "prefer an existing API over generating SQL" idea into a single unified
concept: Query Copilot's matching step should draw from one **asset menu** covering
two catalogued asset types, not SQL templates alone.

- **APIs** — already documented via signature. Directly callable, validated
  business logic already enforced (join/aggregation correctness is not the LLM's
  problem for these).
- **Libraries** — long-history-built repo code that already implements a data
  domain's logic (parsing, stitching, calculation) but isn't necessarily exposed as
  a callable API yet. Catalogued with a metadata record per asset: what it does,
  inputs/outputs, owning domain/team/repo, and — critically — whether it's already
  network-callable or needs a thin wrapper before it can safely join the menu. An
  NL-routed request can't reach into arbitrary repo code directly, so this
  callable-or-not flag determines the real build cost per entry, not just a
  documentation exercise.

Both asset types share one metadata schema so the matching step treats them
uniformly regardless of implementation — SQL template, API call, or wrapped
library call. Unmatched/novel requests still fall to the manual queue (per Option A
above), which also signals where the menu needs a new entry.

**Why this is a consolidation umbrella, not just a Query Copilot component**: once
proven, any existing tool-specific effort elsewhere at WSD can be absorbed later
simply by registering its underlying logic as one more catalogued library/API entry
in the shared menu, rather than remaining a separate parallel effort. This project's
notes deliberately don't name which specific existing efforts that would include —
that's a portfolio-level consolidation call for later, not a Query Copilot design
decision to make now.

**Rollout model**: onboard one domain's existing users onto this architecture at a
time — catalog that domain's APIs+libraries first, prove the menu-matching +
sandbox trial-run for that domain, then repeat for the next. Same narrow-first
phasing already used elsewhere in this project (the digital-clone Proposal A→B→C
pattern), just applied at the domain level instead of the whole-system level.

## Long-term plan: Agent-native direct consumption of the Asset Menu (2026-08-30)

**Why this isn't v1**: current GTO policy permits only **on-prem Vectara RAG Agentic
deployment** for internal-facing agents (see the internal-vs-external routing rule
above), and the approved on-prem model roster (gpt-oss-120b, gemma-4-31b, etc.) sits
well below Claude/ChatGPT-class frontier accuracy on the reasoning/tool-selection/
argument-extraction tasks this design leans on. That gap is exactly why Option A
deliberately keeps the LLM's job narrow — RAG-match to one template, extract a few
parameters — instead of trusting it to reason freely across multiple assets or chain
calls together. Widening model autonomy today, on today's approved models, would raise
correctness risk without the accuracy to back it up.

**The long-term shift**: once that gap closes — either (a) GTO approves a
higher-accuracy model for this narrowly-scoped, sandboxed use case, revisiting the
internal-vs-external routing rule, or (b) on-prem models close the gap themselves —
evolve the Asset Menu from "matched by a constrained NL-classifier" into "consumed
directly by a tool-using Agent." Register every catalogued template, API, and wrapped
library as a callable tool: the Asset Menu's shared metadata schema (name, capability
description, inputs/outputs, owner, callable-or-needs-wrapper flag) already doubles as
the tool schema an agent would need. The Agent then plans and chains its own tool calls
instead of being limited to single best-match lookups, unlocking compositional requests
(e.g. "compare X across two zDB types") that Option A's one-shot matching can't serve
today.

**What must not change**: the sandbox trial-run + human-confirmation gate (see Key
Safety Upgrade) stays mandatory no matter how capable the model gets — a better model
changes *what* the Agent plans, not whether a human sees real sample rows before
trusting the number. This makes the long-term plan additive to Option A/B, not a
replacement of the safety architecture.

**Dependencies / triggers** (none met yet — tracked, not scheduled):
- GTO policy movement on internal-vs-external routing, or the on-prem roster closing
  the accuracy gap — whichever comes first.
- Maverick maturing past its current Die Inventory Agent pilot to support this kind of
  tool-chaining execution — mirrors the "Vectara does matching, Maverick does
  execution" split already sketched under Option C.
- A confirmed-query corpus large enough (from Option A/B usage) to evaluate a more
  autonomous agent against a regression set before granting it wider tool access.

Not a v1/v2 dependency — noted now so the Asset Menu's shared metadata schema is
designed tool-call-ready from the start, even though nothing acts on it until the
model/policy gap actually closes.

## Recommendation

Start with **Option A**, with the asset menu (above) as how its template library
grows over time — not SQL templates alone, but templates + catalogued APIs +
catalogued libraries, matched uniformly. Rationale: matches Yu Han's own stated engineering
principles — 80/20 scoping, no reinventing wheels, narrow-first-then-widen (same
phasing philosophy already applied to the digital-clone project's Proposal A→B→C).
Ships fast, near-zero correctness risk, and the usage data it collects builds the
case (and the semantic-layer requirements) for Option B later. Option C is a
possible v2 once A/B have logged enough real traffic for a safety-net regression set.

**Reinforced 2026-08-29**: Option A holds up even more strongly once cross-checked
against the rest of the team's current workload. Option B has two independent
blockers (Transfer/Stitcher still undated, no free Kin Lum/Tingyu bandwidth before
ww38), and Option C has a live precedent (Die Inventory RAG) showing WSD's currently
approved on-prem AI platforms can't yet execute SQL-agent workloads. Option A is the
only path with no dependency on either the schema migration finishing or a platform
capability gap closing.

## Open items / not yet decided

- Final name (avoid "NXG" — see naming note above, now three prior conflicting uses).
- Whether/how this sequences against Kin Lum's schema-driven NXG migration
  (Feeder/Transfer/Stitcher) — potential dependency for Option B's semantic layer,
  and now also for the "one read API per type" idea above. Feeder finishes ~ww38
  (mid-Sept 2026); Transfer/Stitcher have no target date yet.
- **Concrete next action 1**: ask Foo-Kit what actually happened to Die Inventory RAG
  (OneTrust #27580) — it hit the same "RAG platform has no SQL/DB-agent capability"
  wall that Query Copilot would need to clear for Option B/C. His answer tells us
  whether that platform gap has since been solved, or whether it's still open.
- **Concrete next action 2**: start building the 1000+ example query corpus (see
  above) — no dependencies, can start now.
- **Flag for Yu Han (2026-08-30)**: the PPT's corpus description was manually widened
  from "real support-ticket examples" to "real world queries" — please confirm whether
  this means sourcing goes beyond historical support-request tickets (e.g. live query
  logs, other usage data) so the .md's sourcing description can be made precise rather
  than left at "real-world queries."
- **Concrete next action 3**: design the Asset Menu's shared metadata schema
  (library/API name, capability description, inputs/outputs, owning domain/team/
  repo, callable-or-needs-wrapper flag) and pick the first domain to catalog and
  onboard.
- GTO/security review: since this generates-then-executes SQL against production
  yield/cost data, it likely needs the same kind of review path used for MCP server
  approvals — not yet raised with GTO/security. Also subject to GTO's
  internal-vs-external routing rule (on-prem-only inference; no cloud LLM).
- No v1 component sketch / build-size estimate done yet (offered, not yet requested).
- No decision yet on which option to actually pursue.
- Long-term Agent-native Asset Menu consumption (see dedicated section above) — gated
  on GTO policy/on-prem model accuracy closing the gap to Claude/ChatGPT-class models;
  not scheduled, tracked only.
