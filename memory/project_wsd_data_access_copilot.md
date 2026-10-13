---
name: project_wsd_data_access_copilot
description: "WSD Data Access Co-Pilot design (NL query access via LLM-classify + deterministic-execute) — v1 design done, v2 agent-transition direction agreed, paused for later continuation"
metadata: 
  node_type: memory
  type: project
  originSessionId: ddc762f5-c715-4fcb-b5a4-0a066b170e7e
  modified: 2026-09-09T02:27:55.399Z
---

Full design and reasoning trail lives in `brainstorm/Claude_QueryCopilot.md` (project: `hy.cc.job.digitalclone`), paired with `brainstorm/QueryCopilot_Highlight.pptx` (8 slides). That doc self-documents its own decision history in Appendix B — read it directly rather than trusting this summary once work resumes, since it will have moved on.

**What it is:** lets any WSD stakeholder ask in plain English for data currently reachable only via hand-written SQL-style queries against S3/metadata-DB/gDB. Works by classifying NL requests into a small catalog of known query templates (LLM does classify+match+extract only; everything else — composite-ID resolution, query assembly, sandbox-run, execution — is deterministic software, never LLM free-generation).

**Status as of 2026-09-07:**
- v1 (the Co-Pilot pipeline itself): detailed design complete, not yet built. Pending: SME time for composite-identifier documentation, and a model-tier accuracy benchmark before committing to a build timeline.
- v2 (agent-oriented API transition, next upgrade after v1): direction agreed. Two ideas selected to pursue together as one 6-step to-do plan — publish the catalog as agent-callable tools (#1) + a typed/auto-describable OpenAPI contract (#2), since they share one schema artifact. A third idea (extracting composite-ID resolution into its own standalone service, #3) is explicitly held, not dropped — gated on v1's deterministic-assembly module proving out in production first.
- Deck is split by presentation audience on purpose: Slide 7 (#1+#2 to-do plan) is senior-management-facing; Slide 8 (#3, held) is backup-only, not necessarily shown to senior management.
- Open blocker before any v2 timeline: confirm with GTO/security whether publishing catalog entries as agent-callable tools needs the same review as the currently GTO-approved MCP servers (Confluence/GitHub/Jira/Rally).

**Referenced under a new abbreviation in the weekly status report (09/04/2026)**: Kin Lum's weekly-report line item "Evaluate the Feasibility of WDAC (WSD Data Access Co-Pilot)" is this same project — WDAC appears to be the short internal name now in use for it, filed under "Background AI-Native Ideas." Listed as newly "Initiated" in that report, which sits alongside (not necessarily contradicting) the "paused" status recorded 2026-09-07 above — the two dates are close enough that this may just be the written weekly report catching up to a status that was already changing; confirm current status before assuming either "paused" or "actively initiated" without checking.

**How to apply:** when this topic resumes, read `Claude_QueryCopilot.md` §9 (and Appendix B for full provenance) rather than re-deriving the design from scratch — it already records every direction change and why. Don't re-litigate the LLM/deterministic split (§4) or the buy-vs-build decision (Appendix A) unless new facts have emerged; both were deliberated and closed.

**Status in the weekly WWL report, ww37-ww39 (09/15-09/29/2026)**: the item is now titled "Evaluate New Proposal 'WDAC (WSD Data Access Co-Pilot)'", owners Kin Lum/Yu Han, status In Progress (resolves the earlier paused-vs-initiated ambiguity: the formal report shows active evaluation). Described as V1 = LLM + deterministic software hybrid with sandbox confirmation; V2 = typed schema + OpenAPI contract as agent tools (matches the design above). Pending step as of 09/22: mine/analyze the WUDAS query history (100K+ queries) to segment feature templates, then evaluate technical feasibility. No new update by ww40.
