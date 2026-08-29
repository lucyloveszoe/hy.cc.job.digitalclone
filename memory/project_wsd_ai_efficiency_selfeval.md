---
name: project_wsd_ai_efficiency_selfeval
description: "WSD-wide Claude Code efficiency self-eval framework + Google Form survey rollout — decisions, files, status"
metadata: 
  node_type: memory
  type: project
  originSessionId: 8f70e22b-463b-457a-b8a7-d8d25e21fa35
  modified: 2026-08-25T22:32:22.120Z
---

Yu Han is building a WSD-wide "Claude Code efficiency self-eval" initiative: a 12-metric
checklist (Hard Metrics / Usage Behavior / Red Flags categories) scored 1-5 per metric,
weighted differently by ICB level (see [[project_org_ai_transition_deck]] if that gets
written — for now, the level framework lives in the "Organization Transition" slide of
the WSD-SWTeam-NextToDo-AI-Plan deck). Collected via a role-tagged (no name/email) Google
Form that Yu Han builds and runs himself with his own Workspace credential — this is a
manual human action, not an MCP/agent integration, so it doesn't run into the
Google-Docs/Sheets-MCP-not-yet-approved constraint that blocks the "digital clone" agent
project itself. Feeds into a future "WSD AI Efficiency Profile" aggregation.

**Key decisions (as of 2026-08-25):**
- 12 metrics / 3 categories: Hard Metrics (Time Reduction Ratio, Rework Rate, Review Tax
  Ratio, Session Turns to Acceptable Output, Cost per Delivered Feature); Usage Behavior
  (Model Selection Fit, Verification Habit, Prompt Quality/Scoping, "Vibe" Sizing Control);
  Red Flags (Bug/Hotfix Rate Post-AI-Assist, Readability/Modularity Regression,
  Over-Reliance on Architectural Judgment).
- ICB weighting: Level 2/3 → Rework Rate + Verification Habit; Level 4/5 → Model Selection
  Fit + Review Tax Ratio; Level 5/6 → Over-Reliance on Architectural Judgment + Cost per
  Delivered Feature.
- Survey platform: Google Forms (chose over Jira Service Management form, Confluence
  page/table, or manual xlsx circulation) — fastest/cheapest, and manual-human-use doesn't
  trigger the MCP policy concern.
- Anonymity: "role-tagged, no name" — collects ICB Level + Team only, to balance honest
  self-report against still being able to break results down by level.
- Form simplified to 4 category-level evidence fields instead of 12 per-metric, to reduce
  survey fatigue for an org-wide rollout.

**Why this memory exists:** this is a multi-session initiative tied to
[[project_genai_program]] and the ICB-level AI growth framework. The full reasoning and
file list lives in a `CLAUDE_SelfEval.md` handoff doc that travels with the deliverable
files themselves (originally created under
`C:\Temp\work-I_2026.08.21\WSD_GenAI\wsd-ai-activities\swteam-ai-discussion\round_2_wrapup\`,
but Yu Han is relocating the whole folder to a new path as a "clean reset"). **The path
above is stale by design** — when this comes up again, ask Yu Han for the current folder
location and read `CLAUDE_SelfEval.md` there for full context rather than trusting any
path recorded here.

**Files produced (names stable, path will change):**
- `Claude-Code-Efficiency-SelfEval-Checklist_2026.08.25.xlsx` — 3-tab checklist (ratings,
  rating scale, ICB weighting). Formulas verified by manual cell-reference read-back, not
  LibreOffice recalculation (not installed in that session's environment) — worth one
  manual Excel check before wide distribution.
- `WSD-AI-SelfEval-Survey-OnePager_2026.08.25.pptx` — 1-slide purpose/how-to summary. Had
  open placeholders `[Date TBD]`, `[Survey Link — TBD]`, `[Deadline — TBD]` as of creation.
- `Google-Form-Content_2026.08.25.md` — exact question-by-question Form content (18
  questions / 5 sections) plus recommended settings and expected response-sheet columns.
- `CLAUDE_SelfEval.md` — full handoff doc with all decisions/reasoning above, in more
  detail.

**Status as of 2026-08-25:** all 4 files drafted and internally consistent. Google Form
not yet created. No survey responses yet. No aggregation script yet.

**How to apply:** when Yu Han mentions this survey, the self-eval checklist, or "WSD AI
Efficiency Profile," use this for context, but verify current file locations before
acting on any path — the folder was mid-move as of this memory's creation. Next expected
step is a pandas aggregation script mapped to the response-sheet columns once Form
responses exist.
