---
name: project-draft-wwl-pilot
description: "Proposal B's first pilot task (/draft-wwl) — scope, format, and input-mechanism decisions and reasoning, built 2026-08-28"
metadata:
  node_type: memory
  type: project
  originSessionId: 5c8fc152-f01c-409f-a79a-f377bc2561e4
  modified: 2026-08-29T02:45:36.641Z
---

Yu Han picked the weekly WWL status-report draft as the first Proposal B (Scheduled Draft
Agent) candidate — see [[digital-clone-trust-checkpoints]] for why this was built ahead of
the full 2-3-clean-checkpoint trigger. Three scoping decisions were made explicitly, each
with a rejected alternative:

**v1 scope: only the "In Progress" project/support carry-forward list, not the full report.**
Rejected alternative: drafting all sections (Complete/New/In Progress/Production Support/IT
Support) in one pass. Why: the carry-forward list is the part memory can support most
reliably (it's literally what the accumulated project memory files track), so it's the
narrowest slice with the least exposure if a carried-forward status turns out stale or
wrong. How to apply: don't expand `/draft-wwl` to other sections until this slice has run
cleanly for a few weeks — same trust-building logic as the A→B checkpoint gate.

**Output format: plain markdown, not a .docx matching the historical WWL template.**
Rejected alternative: generating .docx directly. Why: markdown is faster to build and
easier for Yu Han to spot errors in before trusting it; docx generation adds a layer
(templating/formatting) that isn't worth the complexity for an unproven v1. How to apply:
revisit .docx output only if the markdown version proves reliable and Yu Han specifically
wants the polish for direct reuse.

**Delta input: a dedicated `wwl_notes/` drop-folder, not inline chat prompts.**
Rejected alternative: answering a few chat prompts ("what closed this week? what's new?")
during the `/draft-wwl` run itself. Why: mirrors the existing `/log` pattern (manual,
async, opt-in — Yu Han writes notes whenever convenient, not on-demand in a live session),
and keeps Proposal B consistent with Proposal A's "fully opt-in, no synchronous dependency"
design philosophy already established for this project. How to apply: any future Proposal B
task should default to this same async-drop-folder pattern unless there's a specific reason
a task needs live back-and-forth.

**Exclusion rule for "active" vs. "historical archive" memory** (implementation detail worth
recording since it's a judgment call, not a hard rule): `/draft-wwl` treats a memory file as
historical background (excluded from the carry-forward draft) if its filename or `MEMORY.md`
description marks it as an annual/partial-year or ww-range archive (e.g.
`project_2022_partial_review.md`). This heuristic will need revisiting if a future archive
file's naming pattern doesn't match (e.g. a "2025" or "2026" retrospective gets added later)
— check `/draft-wwl`'s `SKILL.md` exclusion rule stays aligned with whatever naming pattern
the yearly-archive memory files actually use.

**Why:** these three decisions all trade a small amount of immediate usefulness for lower
risk/complexity in the first Proposal B artifact — consistent with the project's stated
"narrow first, prove it, then widen" philosophy (see CLAUDE.md's rejection of one broad
autonomous clone agent).

**How to apply:** when scoping the *next* Proposal B task (after `/draft-wwl` proves out),
default to the same pattern established here — narrowest usable slice, plain-text/markdown
output before richer formats, async drop-folder input — unless Yu Han explicitly asks for
something more ambitious once trust is established.
