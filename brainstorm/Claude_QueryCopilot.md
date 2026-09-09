# WSD Data Access Co-Pilot — Project Design

**Status:** Proposal — v1 detailed design | **Date:** 2026-09-05

**Design provenance — read this first:** This is a **new, independent design**, not a fine-tune or refinement of any prior deck. Everything in this document is derived from the analysis conversation this design came out of — real query samples, real scale numbers, and reasoning worked through from first principles. An existing, unrelated slide deck (a separate "WSD Data SaaS Co-Pilot" brainstorm) was consulted for **visual style only** — its color palette, fonts, and slide layout grammar. None of its content, terminology, organizational claims, or assumed constraints were carried into this design. The two should not be treated as the same lineage. See **Appendix B** for the full clarification log, including the point where content from that other deck briefly leaked into an early draft of this one and was explicitly stripped back out.

---

## 1. Executive Summary

Domain experts currently pull data out of WSD's data systems — S3 (raw and processed files), a metadata database (filterable attributes + storage location, not the data itself), and a graph store (gDB, for process/lineage relationships) — by hand-writing SQL-style queries. Every request funnels through whoever on the team can write the query. That's the bottleneck — not the data platform, not the volume, not the systems themselves.

**WSD Data Access Co-Pilot** lets any stakeholder ask in plain English and get a governed, safe, auditable data pull — without generating free-form SQL and without fine-tuning a model on our schemas. It works because the underlying request patterns are **not novel**: 7 years of query history, once deduplicated, collapse into a small, finite catalog of known business intents (roughly 100–a few hundred, pending final dedup). The system's job is to *classify* a natural-language request into that catalog and *deterministically* execute it — not to reason its way to new SQL each time.

**Value in one line:** turn a request that only 2–3 people can currently make into something anyone can ask in under a minute, with a mandatory safety gate — a capped sandbox run reviewed by the requestor — before anything touches production.

**Definition of "v1" (used throughout this document):** the first shippable scope — a curated catalog covering the highest-frequency, already-known request patterns mined from the 7-year/40,000+ query history, not full coverage of every historical variant. A request outside that catalog falls back to human escalation (§4, step ⑧) rather than free-form generation. The exact catalog size is not yet fixed — see §8 for why, and what determines it.

---

## 2. Current State vs Future State

### 2.1 Current State

```
Business stakeholder
      │  (informal ask: Slack/email/hallway)
      ▼
Domain expert / SQL-literate engineer   ← the bottleneck
      │
      │  hand-writes a SQL-style query against the metadata DB, S3, and gDB
      │  must personally know: table names, field encodings, ID mappings,
      │  composite filename formats, which system holds what
      ▼
Manual execution, manual sanity-check, manual delivery
```

**What's actually expensive here is not SQL syntax.** It's tribal knowledge: which table, which domain, which composite identifier format (e.g. wafer test filenames encode die class + wafer ID + process code + pass count + timestamp + IP + suffix in one string), and which of ~40,000 historical query variants over 7 years is the right shape for a given ask. That knowledge lives in a handful of people's heads.

### 2.2 Future State

```
Business stakeholder
      │  natural language
      ▼
Domain Classifier  →  Template/Asset Matcher  →  Parameter Resolver
      │
      ▼
Deterministic query assembly → Capped sandbox run
      │
      ▼
Stakeholder reviews real sample rows → confirms → full execution against production
```

The tribal knowledge doesn't disappear — it gets **encoded once** into a Catalog of known request templates (with explicit metadata/schema), and the classifier/matcher reuses it forever after. New requests that don't match the catalog fall back to a human, and the resolution gets added to the catalog — the system only gets more capable over time.

### 2.3 Side-by-Side

| | Current | Future |
|---|---|---|
| Who can make a request | 2–3 SQL-literate experts | Any stakeholder, in English |
| Turnaround | Hours to days, queued behind a person | Minutes, capped sample first |
| Knowledge location | In people's heads, undocumented | Explicit Catalog (~100–a few hundred entries) |
| Safety check | Read the SQL (unreliable) | Run a capped sandbox sample, judge real rows |
| Scale-up path | Hire/train more SQL experts | Add catalog entries; classifier reuses them |
| New/unseen request | Same bottleneck as before | Escalates to human once, then joins the catalog |

---

## 3. See It Work: Pre vs Post, Stage by Stage

Rather than describe the transformation abstractly, walk one real request all the way through.

**Request:** *"Show me OQA test lots for AFEM-8234-AP1 closed since Jan 2026."*

### Pre (today)

```
One hand-written line, produced by someone who already knows the schema:

  select * from lot where productname like ('AFEM-8234-AP1%')
    and testtype = 'OQA' and EndTime >= '2026-01-01 00:00:00'

Cost: hours to days (queued behind the person who can write it).
Knowledge required: table name, field names, wildcard convention — all tribal.
Auditability: none — nobody else can tell how this was derived.
```

### Post (with Co-Pilot) — the same request, stage by stage

| # | Stage | Model? | What comes out |
|---|---|---|---|
| 1 | **NL request** | — | `"Show me OQA test lots for AFEM-8234-AP1 closed since Jan 2026"` |
| 2 | **Domain classify** | LLM | `domain = "be-module"` |
| 3 | **Template match** | LLM | `template = "lot_by_product_test_date"` (1 of ~100 catalog entries in this domain) |
| 4 | **Parameter extraction** | LLM | `{ product: "AFEM-8234-AP1", test_type: "OQA", since: "2026-01-01" }` |
| 5 | **Deterministic assembly** | — (software) | `select * from lot where productname like ('AFEM-8234-AP1%') and testtype='OQA' and EndTime >= '2026-01-01 00:00:00'` |
| 6 | **Capped sandbox run → confirm** | — (software) | Runs with a row cap, shows the requestor real sample rows, waits for confirmation before the uncapped production run |

**Same query text comes out at stage 5 as the hand-written line from "Pre."** Nothing about the underlying data access changed — what changed is *who* can produce it, *how fast*, and *how auditable* the path getting there is. Every stage above is a discrete, inspectable artifact — if something's wrong, you can see exactly which stage produced the wrong value, instead of debugging one opaque hand-written line.

---

## 4. Proposed Architecture / Workflow

```
Natural-language request
      │
      ▼
① Domain Classifier            (LLM — narrow classification, ~5-10 categories)
      │
      ▼
② Template/Asset Matcher       (LLM — match against ~20-80 catalog entries within the domain)
      │
      ▼
③(a) Parameter Extraction      (LLM — pull stated values: dates, product names, test type)
③(b) Composite ID Resolution   (Deterministic — assemble filenames/patterns from parts;
                                 never LLM free-generation)
      │
      ▼
④ Multi-step chain execution   (Deterministic — sequential ID hand-off between calls,
                                 for templates that chain 2+ queries)
      │
      ▼
⑤ Query assembly                (Deterministic compiler — wildcards, IN-lists, date formats)
      │
      ▼
⑥ Capped sandbox run             (Deterministic — capped rows / size / frequency)
      │
      ▼
   Stakeholder reviews real sample rows → confirms
      │
      ▼
⑦ Confirmed execution against production (Deterministic)
      │
      ▼
⑧ Unmatched / low-confidence → escalate to human → resolution added to catalog
```

**Governing rule:** the LLM decides *which known intent this is and what values were stated*; deterministic software decides *exactly how the authorized query is built and run*. The LLM never sees production data directly and never generates a query string from scratch.

### 4.1 Where LLM Is Involved — and Where It Explicitly Is Not

| Step | LLM? | Why |
|---|---|---|
| ① Domain classify | **Yes** | Language understanding, low-stakes classification |
| ② Template match | **Yes** | Matching phrasing variance to known intents |
| ③(a) Extract stated values | **Yes** | Slot-filling from free text — LLM's actual strength |
| ③(b) Composite ID assembly | **No** | Fixed string format — a template-fill task, not a reasoning task |
| ④ Multi-step chaining | **No** | Sequential execution, no judgment needed |
| ⑤ Query assembly | **No** | Deterministic compiler |
| ⑥ Sandbox run | **No** | Rule-based caps |
| ⑦ Confirmed execution | **No** | Deterministic |
| ⑧ Escalation threshold | **No** (rule) | Confidence score from step ② compared to a fixed threshold |

Only 3 of 9 steps touch a model at all, and none of the three require deep reasoning — they're classification and slot-filling over a small, curated catalog.

### 4.2 Model Tier

Classification into ~5–10 domains, matching against ~20–80 known templates, and structured slot-filling of a handful of parameter values are not deep-reasoning tasks. A fast, low-cost model tier (e.g. Claude Haiku-class) is capability-appropriate here — reaching for a frontier-tier model would be paying 5x+ per request for accuracy headroom this task doesn't need. Rough order of magnitude: at cheap-tier pricing, a single classify+extract request costs an estimated **~$0.002** — cost is not the constraint on this design; task fit is.

**Two things to validate before committing to a specific model, not to assume:**
1. **Benchmark against real historical queries** — pull a held-out slice of the golden-query corpus, run it through the candidate model, and measure classification/extraction accuracy directly, rather than reasoning about it in the abstract.
2. **Confirm any data-residency / network-isolation requirement with IT/security up front.** If production inference must stay on-prem, the narrow-scope design still holds — swap in an approved on-prem or open-weight model for the runtime path. A cloud model (used for offline catalog curation, see §5) can still be used to help *build* the catalog even if it can't run at request time.

### 4.3 What LLM Never Does

Never sees production data directly. Never free-generates a query string. Never assembles composite identifiers (e.g. the wafer test filename format) from scratch — that's a deterministic template fill, because a single wrong character there is a silently-wrong query.

### 4.4 Safety Mechanism

Confirmation is **not** "read the generated SQL and spot the bug" — that's an unreliable ask even of experts. It's a **capped sandbox run**: the system executes the matched query with hard caps on row count / result size / frequency, and the stakeholder judges real sample rows before authorizing the full production run. This stays mandatory regardless of how capable the underlying model gets.

---

## 5. Real Query Evidence (Grounding, Not Guesswork)

Sampled from the "most frequent" query log across two domains — **be-module** and **fbar-wafer**:

```
select * from lot where productname like ('AFEM-8234-AP1-QA-RF2%')
  and testtype = 'OQA' and EndTime >= '2022-01-01 00:00:00'

SELECT * FROM WaferTraceZip WHERE DieClassName = '2YB4-1701'
  AND WaferID = 'N2481-P076'
  AND ParaFileName = '2YB4-1701_N2481-P076_PR-EG4090-264_FULL-2PASS_20260319_222655_IP192.168.0.2_Q3JMBAS'
```

Findings that shaped the architecture above:

- **No joins, no aggregation in the observed high-frequency set.** Every sample is a single-table point lookup filtered by identifiers. This is a classification/slot-filling problem, not a query-planning problem — a cost-based cross-source optimizer would be solving a problem we don't actually have yet.
- **Composite identifiers are the real complexity.** A stakeholder will never type `2YB4-1701_N2481-P076_PR-EG4090-264_FULL-2PASS_20260319_222655_IP192.168.0.2_Q3JMBAS`. The system must resolve/construct it from the parts the user *did* say — deterministically.
- **Some templates are 2-step chains** — e.g. query one table for matching lot IDs, then feed those IDs into a second table's `IN (...)` filter. This needs simple sequential ID hand-off, not a general DAG scheduler.
- **Domain is a natural first-level classifier.** The query log is already organized by domain — use that grouping as the first classification step rather than a flat pool of ~100+ templates.

---

## 6. Build Plan

1. **Corpus:** mine 1000+ "golden" queries from the 40,000+ historical log; attach NL context (business description + example phrasing) to each — the labeled data the classifier/matcher needs and doesn't yet have. An LLM can draft candidate descriptions from the raw query strings, but a subject-matter expert must review and correct them — reverse-engineering intent from a query string alone is error-prone.
2. **Catalog:** consolidate the deduplicated query shapes into a Catalog — each entry schema-driven (parameters, types, valid ranges). Explicitly document composite-identifier formats (field-by-field meaning of encoded filenames) as part of this — this is a hard blocker for parameter resolution (§4, step ③b) and can only come from the people who currently hold that knowledge.
3. **Runtime:** wire the chosen model to do domain classify → template match → parameter extraction, then hand off to the deterministic query assembler and sandbox-run executor.

**Target v1 scope:** the highest-frequency, highest-value request shapes identified from the golden-query corpus — not full coverage of the historical 40,000+ variants.

---

## 7. Most Challenging Section

**Composite identifier resolution and catalog curation labor.**

The single hardest, least-automatable piece of this project is decoding and documenting what each segment of identifiers like `2YB4-1701_N2481-P076_PR-EG4090-264_FULL-2PASS_20260319_222655_IP192.168.0.2_Q3JMBAS` actually means, and building the deterministic rules to reconstruct/pattern-match them from partial, business-friendly input. This can't be reverse-engineered reliably by an LLM from the strings alone — it requires the people who currently hold this tribal knowledge to sit down and write it out, once, per domain. It's a one-time cost, but it sits squarely on the critical path: nothing downstream can be built or tested until this documentation exists. Budget real subject-matter-expert time for this, not just engineering time.

---

## 8. Most Risky Section

**The model-tier accuracy assumption is untested.**

The entire pipeline design (§4) is deliberately narrow — 3 of 9 steps touch a model, and none of the three require deep reasoning — on the assumption that a fast, low-cost model tier is accurate enough for classification and slot-filling once the catalog exists. That assumption is *reasonable* (§4.2 explains why), but it has not yet been measured against real historical queries. Two concrete failure modes if it's wrong:

- If real classification/extraction accuracy comes in lower than assumed, v1 may need a narrower per-domain candidate list, more aggressive rule-based pre-filtering before the model is invoked at all, or a lower auto-confirm threshold — pushing more requests to human escalation than planned and eroding the "minutes not days" value proposition.
- The exact final catalog size is still pending a full dedup pass on the 40,000+ log — it could land anywhere from ~100 to a few hundred entries. A larger-than-expected catalog makes the template-matching step (②) harder and raises the same accuracy question from a different angle.

**Mitigation already designed in:** the capped-sandbox-run + human-confirmation gate is mandatory regardless of model accuracy, which caps the damage from a wrong classification — a bad match produces a bad sample the stakeholder catches, not a bad production run. This is why the risk above is tolerable rather than disqualifying, and why it's worth resolving with a benchmark before committing to a v1 timeline (see next step below).

**Recommended next step:** benchmark the candidate model tier against a held-out slice of the golden-query corpus before committing to a v1 timeline — this number determines whether the design in §4 holds as-is or needs adjustment.

---

## 9. Next Upgrade — Agent-Oriented API Transition (v2 Direction)

**Not in v1 scope.** Recorded here as the agreed direction for the upgrade after v1 ships, so the reasoning isn't lost between now and then.

### 9.1 Why this is a separate upgrade, not a v1 feature

v1's Co-Pilot is one specific consumer of the existing SQL-string APIs — it does NL → classify → match → extract, then assembles the string itself. The broader industry trend is APIs built so *any* agent can call them directly, without a bespoke NL front-end doing the interpreting each time. Two of the three ideas from that discussion were selected to pursue now; the third is explicitly deferred (§9.4).

### 9.2 Idea #1 + Idea #2 are one artifact, not two sequential projects

These were picked separately but turn out to share the same underlying work:

- **Idea #2 (typed, auto-describable contract):** replace the SQL-string request format with a typed schema per domain/entity — `{domain, entity, filters: {product, test_type, since, ...}}` instead of a hand-built SQL-style string. Published as OpenAPI/JSON-Schema, so tool definitions can be *generated* from it rather than hand-written.
- **Idea #1 (catalog published as agent tools):** each catalog entry (e.g. `lot_by_product_test_date`) becomes a callable tool — name, description, typed parameters — usable by any agent, not just Co-Pilot's internal classifier.

**The connection:** §6 (Build Plan, step 2) already commits to making each catalog entry "schema-driven (parameters, types, valid ranges)" — that work exists in the v1 plan regardless of this upgrade. That same schema *is* the typed contract Idea #2 needs, and it's exactly what Idea #1's tool definitions get generated from.

**The v2 to-do plan** (one shared build sequence — this is also Slide 7 in the deck, `WHY-tag` per step matches the slide):

| # | Step | Tag | What it does |
|---|---|---|---|
| 1 | Define Typed Schema | Foundation | Parameters, types, valid ranges per catalog entry — already funded by v1's catalog-curation labor (§6), not new work. |
| 2 | Publish OpenAPI Contract (#2) | Contract | One machine-readable spec per domain — the typed request format that replaces the SQL-style string as the public contract. |
| 3 | Generate Agent Tools (#1) | Tooling | Callable tool definitions generated from the spec — named, typed, discoverable by any agent, not hand-built per entry. |
| 4 | Wrap the Confirm Gate | Safety | Sandbox-cap → confirm becomes a two-call contract: `preview` returns a token, `execute` requires it — same mandatory gate (§4.4), agent-callable. |
| 5 | GTO/MCP Policy Check | Policy Gate | Confirm whether an agent-callable data API needs the same review as the approved Confluence/GitHub/Jira/Rally MCP servers (§9.5). |
| 6 | Pilot One Domain | Pilot | Ship #1+#2 for one domain end-to-end — prove the pattern before scaling to the rest of the catalog. |

No separate schema-authoring effort for #2 on top of #1 — one schema, two expressions of it (a published spec, and a servable tool set). Steps 1–3 build that shared artifact; 4–6 make it safe and provable before scaling.

### 9.3 What does and doesn't change underneath

| Layer | v1 (today) | v2 (this upgrade) |
|---|---|---|
| Request format callers use | Free-text NL, interpreted by Co-Pilot's own LLM steps | Typed tool call with named parameters — no NL interpretation needed if the caller is already an agent |
| Who does domain-classify / template-match | Co-Pilot's steps ①② (LLM) | The calling agent's own tool-selection — tool *is* the classification. Steps ①② collapse for external callers. |
| Deterministic assembler (composite-ID resolve, query assembly) | Internal to Co-Pilot, steps ④⑤ | **Unchanged in function** — moves one level up the stack to sit behind the typed tool contract instead of behind Co-Pilot's NL pipeline. Still the same deterministic compiler, still never touched by a model. |
| Underlying execution APIs (SQL-style string, S3/metadata-DB/gDB) | Unchanged | **Still unchanged.** This upgrade changes the *request contract facing callers*, not the execution engine. The deterministic assembler still compiles down to the same SQL-style string internally. |
| Sandbox-cap → confirm gate | UI: stakeholder reviews sample rows, confirms | Becomes a two-call tool contract: a `preview` call returns capped sample rows + a confirmation token; a separate `execute` call requires that token. Same mandatory gate (§4.4), expressed as protocol instead of UI. |
| Co-Pilot's own NL front door | The only caller | Becomes *one more caller* of the published tool layer, instead of owning a separate hand-rolled pipeline. (Not required for v2, but a natural follow-on simplification once the tool layer exists.) |

**Net effect:** this upgrade is lower-risk than it might sound — it does not touch the execution APIs or the deterministic assembler's logic, only where the typed/tool contract sits relative to them.

### 9.4 Idea #3 (composite-ID resolution as its own service) — held, not dropped

Splitting composite-ID resolution (§7's hardest problem) out from Co-Pilot into its own standalone, independently-callable service is still worth doing eventually — both sampled domains (be-module, fbar-wafer) lean on composite identifiers, so a standalone resolver would pay off for any future agent-facing API, not just this one.

**Why it's held:** its difficulty is gated by the deterministic-assembly module's own maturity — steps ④ (composite-ID resolution) and ⑤ (query assembly) inside Co-Pilot need to prove out in production first. Extracting a service boundary from logic that hasn't yet been validated risks exporting an unstable interface to new callers instead of a solid one. Revisit once v1's deterministic core has run against real requests long enough to trust it.

### 9.5 Open items before committing to a v2 timeline

- **GTO/MCP policy check:** publishing catalog entries as agent-callable tools likely means an MCP-server (or equivalent) surface for an internal data API. Confirm with GTO/security whether this needs the same review as the currently-approved Confluence/GitHub/Jira/Rally MCP servers, or falls under a different category since it's not a comms channel.
- **Existing consumers of the SQL-string contract:** any caller other than Co-Pilot itself that already depends on the string-based API needs a migration path before that contract can be considered "internal implementation detail" rather than the public one.
- **Catalog growth vs tool-contract stability:** the catalog is designed to grow via escalation (§4, step ⑧). Tool definitions generated from it need an append-only/versioning discipline so adding a new catalog entry doesn't break agents already integrated against the existing tool set.

### 9.6 Deck structure: what's presentable to senior management vs backup

The v2 material is split across two slides in `QueryCopilot_Highlight.pptx`, on purpose, not by accident:

- **Slide 7 — "Agent-Oriented API Transition — v2 To-Do Plan"** covers only #1+#2 (the six-step plan in §9.2). This is the slide meant for senior-management review.
- **Slide 8 — "Composite-ID Resolution Service (#3) — Held"** covers only #3 (§9.4), framed as real-value-but-blocked. Kept as a separate, droppable backup page — not intended for the same senior-management audience as Slide 7, since it documents a deferred idea rather than an active plan.

This mirrors the document structure: §9.1–9.3 and 9.5 (the #1+#2 plan) are the active material; §9.4 (held #3) is intentionally self-contained so it can be skipped without losing context on the rest.

---

## Appendix A — Buy vs Build

We considered buying a semantic-layer / NL-to-SQL product (e.g. Snowflake Cortex Analyst, dbt Semantic Layer + MetricFlow, Cube.dev) instead of building this. **Decision: build.**

**The execution surface is our own APIs, not a SQL warehouse.** These products emit SQL text against warehouse tables. Our target is "call our existing data-access APIs with the right parameters," where the deterministic business-logic mapping (joins, cross-system stitching, ID resolution) is **already built and running** inside those APIs. No semantic-layer product speaks our APIs' proprietary query-string dialect. Buying one would leave 100% of the actual integration work undone — we'd be paying for a capability (SQL generation over a warehouse) we don't need, while still building the capability we do need (NL → our API calls) from scratch anyway.

**What we keep from "buy" thinking:** the *pattern* — a declarative catalog of business-callable capabilities, an LLM constrained to selecting from that catalog rather than generating free text, and a deterministic compiler/executor underneath. That pattern is exactly what these products do internally; we're applying the same pattern to our own API layer instead of to a warehouse.

**One exception, not a reason to buy the whole thing:** graph/lineage data (gDB — wafer/lot genealogy, process path) doesn't fit a relational semantic model well, regardless of vendor. It stays a separate capability behind its own adapter whether we build or buy — so it isn't an argument for buying.

| | Buy: Semantic-Layer Product | Build: Catalog Match on Our APIs |
|---|---|---|
| Output | SQL against warehouse tables | Calls to our existing APIs |
| Runtime dependency | Cloud LLM (typical for these products) | Model tier of our choosing |
| Speaks our API dialect | No | Yes — that's the point |
| Reuses existing deterministic logic | No — logic would be rebuilt in the vendor's semantic model | Yes — already built and running |
| Graph/lineage (gDB) fit | Same limitation, regardless of vendor | Same — separate adapter either way |

---

## Status

**v1:** Detailed design (this document) — ready to move from proposal to build, pending sign-off on the build plan (§6) and SME time allocation for composite-identifier documentation, plus a model-tier benchmark (§8) before committing to a timeline.

**v2 (next upgrade, direction agreed — see §9):** Agent-oriented API transition. Ideas #1 (catalog published as agent tools) and #2 (typed/auto-describable OpenAPI contract) selected to pursue together, since they share one underlying schema artifact — see the 6-step to-do plan in §9.2, mirrored as Slide 7 of the deck. Idea #3 (composite-ID resolution as a standalone service) held pending v1's deterministic-assembly module (steps ④⑤) proving out in production — mirrored as the separate, backup-only Slide 8 (§9.6). Not yet scoped into a committed timeline — §9.5 lists what needs resolving first, GTO/MCP policy chief among them.

---

## Appendix B — Design Process & Clarification Log

This design went through multiple rounds of clarification and one explicit direction change. Recorded here so the reasoning trail survives alongside the design itself, not just in a chat transcript that eventually scrolls out of context.

**Phase 1 — Architecture concept, sanity-checked.** Started from a general "LLM plans, deterministic software executes" architecture concept (business-language interpretation → semantic layer → logical planner → physical planner → execution adapters → result validation, with a typed intermediate representation borrowed from compiler design). Assessed as directionally sound — it matches how real production systems in this space work (semantic-layer + LLM front ends) — but flagged as over-scoped for a v1: a dual-stage LLM planner, a cost-based cross-source physical optimizer, and a full BI-style metric/dimension semantic model are all more machinery than a first version needs.

**Phase 2 — Data architecture clarified; buy option evaluated.** The actual data layout was established: raw files (70+ formats) and processed/indexed binary both live in S3; a metadata database holds only filterable attributes and storage pointers, never the data itself; a graph store (gDB) holds process/lineage relationships separately. Against this, a "buy" option (Snowflake Cortex Analyst, as a semantic-layer product) was evaluated for how much data migration it would require. Conclusion: only metadata needs to become SQL-visible to Snowflake — raw binary never moves.

**Phase 3 — Graph/lineage carve-out identified.** gDB's graph-shaped queries (lineage, genealogy, multi-hop traversal) don't fit a relational semantic model well, regardless of vendor. Established early that this needs its own adapter no matter what — not an argument for or against buying, just a fixed constraint.

**Phase 4 — Build vs buy reversed.** A key fact changed the recommendation: a layer of existing, higher-level APIs already exists, accepting a "SQL-style query string" and executing deterministically against the underlying systems — meaning the expensive part (cross-system business-logic mapping) is **already built**. Cortex Analyst-style products generate SQL against a data warehouse; that's a different target than calling proprietary APIs. Buying one would leave the real integration work (natural language → our API calls) completely undone. Decision flipped from "evaluate buying" to "build," and has held since.

**Phase 5 — Scale clarified; problem reframed.** Real numbers: 7 years, 40,000+ historical queries, deduplicating to an estimated 100–10,000 distinct business-case "templates." This reframed "natural language → select API + fill parameters" from an open-ended generation problem into a **classification + retrieval + slot-filling** problem — the single biggest simplification in this design. It also surfaced the key blocking gap: no natural-language descriptions exist yet for these templates, only raw query strings.

**Phase 6 — Real query samples reviewed directly.** A real "most frequent" query sample set (two domains: be-module, fbar-wafer) was reviewed line by line. Findings that reshaped the pipeline: (a) the dominant workload has no joins or aggregation — a cost-based physical optimizer isn't needed for v1; (b) composite identifiers (e.g. wafer test filenames encoding 6-7 fields in one string) are the real hidden complexity, needing a dedicated **deterministic** resolution step, never LLM free-generation; (c) some templates are 2-step ID-handoff chains, not single calls; (d) domain (be-module / fbar-wafer / …) is a natural, already-present first-level classifier.

**Phase 7 — Pipeline right-sized against those findings.** Cut the cost-based physical optimizer and the dual-LLM-planner stages from v1 scope. Added composite-identifier resolution and multi-step chaining as explicit, named pipeline steps. Confirmed — against current published guidance on LLM tool-selection accuracy — that in-context, domain-grouped classification is sufficient at this scale without dedicated vector-retrieval infrastructure; that's deferred until a domain's candidate list actually grows past the range where in-context selection degrades.

**Phase 8 — LLM involvement and model tier mapped precisely.** Landed on exactly 3 of 9 pipeline steps touching a model (domain classify, template match, simple slot-filling) — see §4.1 and Appendix (slide 3 in the deck) for the full breakdown. Composite-identifier assembly was explicitly excluded from the LLM's job even though it looks "structured" — it's a template-fill task, not a reasoning task. Concluded a fast, low-cost model tier is capability-appropriate for the 3 touchpoints that do need a model (not a frontier-tier model), with cost estimated around $0.002/request. Any requirement to run entirely on-prem or on an open-weight model was framed as a policy/infrastructure question to confirm with IT/security — not a model-capability question — and the design was built to not depend on the answer either way.

**Phase 9 — First draft leaked content from an unrelated deck.** The project was named "WSD Data Access Co-Pilot." A first draft of this document and a matching-style slide deck were produced using an existing internal file (`new_proposal.pptx`) purely as a visual-style reference. **In that first draft, real content from that reference file leaked in** — it turned out to be an existing, unrelated "WSD Data SaaS Co-Pilot" brainstorm (different assumptions, different author) containing specific organizational claims: an on-prem-only governance policy, specific system names, a named build plan, and a "sandbox trial run" mechanism presented as already-existing. These got absorbed into the first draft without being critically separated from this conversation's own conclusions.

**Phase 10 — Explicit correction: this is a new design, not a fine-tune.** On review, this was flagged and corrected directly: reuse *only* the reference file's visual style (palette, fonts, layout grammar) — none of its content, terminology, or assumed constraints. Both this document and the deck were rewritten from this conversation's own material only. **This is the key provenance fact this document exists to record: this is a new design, independent of the prior "WSD Data SaaS Co-Pilot" brainstorm — not a refinement of it.** Where a mechanism here resembles something in that other file (e.g. a sandbox-run confirmation gate), it's because this conversation arrived at it independently, not because it was copied.

**Phase 11 — Concrete worked example added.** A dedicated stage-by-stage walkthrough (§3) was added — one real request traced from natural language through to a final query awaiting sandbox confirmation — specifically so the pipeline in §4 isn't left abstract.

**Phase 12 — Self-containment gaps closed.** Two gaps were caught by direct questions and fixed: "v1" had been used repeatedly without ever being defined (fixed — see the Executive Summary definition above); and "3 of 9 pipeline steps" referenced a 9-step breakdown that existed only in this document's §4.1 table, never drawn out anywhere a reader could see it without cross-referencing. Fixed by adding a dedicated, fully-labeled 9-step diagram to the deck, with this document's §4 as its documented source rather than an implicit one.

**Phase 13 — v2 direction scoped, three ideas narrowed to two-plus-one-held.** With v1 design accepted as sufficient for now, three candidate directions were proposed for the next upgrade, all responding to the same trend — APIs built for direct agent use rather than requiring a bespoke NL front-end per consumer: (1) publish the Catalog itself as callable agent tools, (2) replace the SQL-string request contract with a typed, auto-describable OpenAPI/JSON-Schema contract, (3) extract composite-ID resolution out of Co-Pilot into its own standalone, independently-callable service. Decision: pursue (1) and (2) together — recognized as one shared schema artifact rather than two separate efforts (§9.2) — and hold (3), since its difficulty is gated by the deterministic-assembly module (steps ④⑤) proving out in production first, not by any lack of value in the idea itself. Recorded as §9, with the underlying-API-unchanged property (§9.3) called out explicitly since it's the reason this upgrade is lower-risk than "add agent support" might otherwise sound.

**Phase 14 — v2 to-do plan drafted, deck restructured, audience-split by slide.** The high-level #1+#2 pairing from Phase 13 was turned into a concrete six-step build sequence (§9.2's table: Define Typed Schema → Publish OpenAPI Contract → Generate Agent Tools → Wrap the Confirm Gate → GTO/MCP Policy Check → Pilot One Domain). The deck (`QueryCopilot_Highlight.pptx`) was restructured from a single combined slide into two: Slide 7 mirrors this six-step plan (#1+#2 only, senior-management-facing), and Slide 8 was split out to carry #3 alone (held, backup-only — explicitly flagged as not necessarily presented to senior management). This audience split is recorded as §9.6 so the reasoning behind the deck's page boundaries doesn't live only in slide notes nobody reads later.
