# Project: Digital Clone of Yu Han

## Mission

Build a digital agent that observes and remembers Yu Han's work activity, understands
his job functions (engineering manager, Broadcom WSD AI Champion, AWS data-automation
expert), and eventually performs part of his job under a strict harness — no unsupervised
external actions, ever.

## Phased approach (decided)

- **Proposal A — Passive Shadow (current phase, in progress)**: pure memory-building.
  Yu Han drops files into `inbox/`, runs `/log` manually, the skill files structured
  memory and archives the source. Zero autonomous actions of any kind.
- **Proposal B — Scheduled Draft Agent (first pilot task built 2026-08-28)**: once memory
  proves reliable (see trigger below), pick ONE narrow recurring task Yu Han already does
  by hand (e.g. weekly status digest, first-pass doc review) and produce draft-only output
  to an outbox. Never sends, publishes, or merges anything itself. First pilot: `/draft-wwl`
  (see below) — drafts only the "In Progress" carry-forward section of the weekly WWL
  report, deliberately the narrowest usable slice. Built ahead of the full 2-3-clean-
  checkpoint trigger (only checkpoint 1 was logged clean as of this build) at Yu Han's
  explicit direction — an intentional exception, not a change to the trigger rule itself.
- **Proposal C — Scoped Task Agents with Approval Gates (later)**: several small,
  single-purpose agents for specific recurring duties, each with an explicit tool
  allowlist and a mandatory human-approval step before anything externally visible fires.

Rejected: one broad autonomous "clone" agent from the start — undefinable scope, no way
to harness it. Always narrow first, prove it, then widen.

## Trigger for moving A → B

Not a data-volume threshold — a trust threshold. Run the periodic checkpoint ("summarize
what you understand about my role and priorities") roughly monthly. After 2-3 checkpoints
come back accurate with little/no correction needed, memory is stable enough to layer in
one draft-only task.

## Integration constraints (important — do not route around)

- Yu Han's actual work surface is Google Workspace (Gmail, Google Chat, Docs, Sheets).
- Broadcom/GTO policy: only GTO-hosted MCP servers permitted, **no local MCP servers**.
  Currently approved: Confluence, GitHub, Jira, Rally. Google Drive/Docs MCP is only
  "planning" — not yet available.
- Decision: do NOT build a Gmail API script or a Google Chat bot to route around this.
  Even though neither is literally an "MCP server," both would functionally replicate
  the unreviewed-AI-access-to-internal-comms risk the policy exists to prevent — and
  building it would undermine Yu Han's credibility as the BU's AI Champion for
  responsible deployment.
- Correct path: (1) ask GTO/security directly whether a personal Gmail/Chat integration
  needs the same review as MCP, and/or (2) wait for Google Drive/Docs MCP to become
  GTO-approved. Until then, manual drop-in via `inbox/` is not a fallback — it is the
  only compliant option, and that's fine for Proposal A.
- GitHub/Jira/Confluence/Rally being pre-approved makes them the realistic first targets
  once Proposal B/C need a live data source, given Yu Han's AWS data-automation work.

## What's built

- `inbox/` — drop zone for raw files (any format: txt/md/docx/pdf/csv/images). Suggested
  naming `YYYY-MM-DD-short-description`, not enforced.
- `inbox/archive/` — processed files land here after successful filing. Nothing is ever
  deleted, only moved.
- `.claude/skills/log/SKILL.md` — the `/log` skill. Manual trigger only (no cron/file-watch
  automation yet — deliberately chosen over automation to keep Proposal A fully opt-in).
  Reads new files in `inbox/` root only, files structured memory into the four types
  (user/feedback/project/reference) at the project's memory folder, updates `MEMORY.md`,
  archives the source only after the memory write succeeds, never fabricates or guesses on
  ambiguous/sensitive content (flags it instead).
- Memory folder for this project:
  `C:\Users\hany\.claude\projects\C--Temp-AllCode-Claude-hy-cc-job-digitalclone\memory\`
- `wwl_notes/` — drop zone for rough weekly notes (any format) feeding `/draft-wwl`.
- `wwl_notes/archive/` — processed notes land here after a successful draft. Nothing is
  ever deleted, only moved.
- `wwl_outbox/` — dated draft-only markdown output from `/draft-wwl`. Yu Han reviews/edits
  before using; nothing here is ever sent, posted, or merged automatically.
- `.claude/skills/draft-wwl/SKILL.md` — Proposal B's first pilot skill. Manual trigger only.
  Reads `wwl_notes/` plus the memory folder's **active** (non-archival) project memories,
  drafts only the "In Progress" carry-forward list, flags unmapped notes as possible new
  items instead of guessing, and never drafts the Complete/New/Production-Support/IT-Support
  sections — those stay fully manual.

## Layout decision: flat, not nested

Considered `YYYY-MM-DD/short-description/` nested folders vs. flat files in `inbox/`.
Chose **flat**: `inbox/` stays near-empty since files archive right after processing, so
there's no clutter problem to solve, and date-prefixed filenames already sort
chronologically without folders. Nesting was deferred as an optional future "bundle"
mode (a subfolder = multiple related files filed as one unit) — only worth building if
multi-file activities (e.g. meeting notes + slides + transcript) become a real recurring
pattern. Not built yet.

## Feedback loop (how Yu Han knows this is working)

1. **Per-run**: `/log`'s own summary of what it filed, updated, or flagged.
2. **Compounding**: `MEMORY.md` loads automatically into every session in this project,
   so understanding accumulates silently across sessions — shows up as fewer repeated
   explanations and more accurate first drafts, not a single dramatic moment.
3. **Explicit audit**: Yu Han periodically asks "summarize what you currently understand
   about my role and priorities" to surface and correct the accumulated profile before
   errors compound. This is also the A → B trust-threshold check (see above).
4. **Per-run** (Proposal B): `/draft-wwl`'s own summary of threads updated vs. carried
   forward, plus anything flagged as a possible new item or needing input — same
   transparency pattern as `/log`, applied to draft output instead of memory writes.

## Current status

- `/log` / Proposal A: in active use. Memory folder covers Yu Han's role/team, working
  principles, and ~20 active initiatives, plus a growing weekly-status archive (2021
  partial, 2022 nearly complete through ww51). Trust checkpoints: 1 of the informal 2-3
  needed logged clean (2026-08-25); see `project_digital_clone_trust_checkpoints.md`.
- `/draft-wwl` / Proposal B: skill and folders (`wwl_notes/`, `wwl_outbox/`) built
  2026-08-28, not yet run with real data.
- Next action: Yu Han drops this week's rough notes into `wwl_notes/`, runs `/draft-wwl`,
  reviews the draft quality before making it a habit.

## Guardrails (non-negotiable, carry into every future phase)

- Never delete any file — archive/move only.
- Never fabricate memory content — file only what a source explicitly states.
- Never take any externally-visible action (send, post, publish, merge) without an
  explicit human-approval step, no matter how much trust has accumulated.
- Never write outside `inbox/`, `inbox/archive/`, `wwl_notes/`, `wwl_notes/archive/`,
  `wwl_outbox/`, and the project's memory folder.
- When ambiguous or sensitive, stop and ask — do not guess.
