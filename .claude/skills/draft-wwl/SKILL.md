# /draft-wwl — Draft the "In Progress" Carry-Forward List

This is Proposal B's first pilot task (see project `CLAUDE.md`): a narrow, manual-trigger,
**draft-only** skill. It never sends, publishes, or merges anything — it only writes a
markdown draft to `wwl_outbox/` for Yu Han to review, edit, and use himself.

**Scope (deliberately narrow for v1)**: this drafts ONLY the "In Progress" project/support
carry-forward list — the section of Yu Han's weekly WWL status report that tracks each
named initiative's status week over week. It does NOT draft Complete, New, Production
Support, or IT Support sections — those stay manual.

## Inputs

- **Memory** (read-only): `C:\Users\hany\.claude\projects\C--Temp-AllCode-Claude-hy-cc-job-digitalclone\memory\`
  — specifically the **active** project-type memory files (current initiatives), not the
  historical yearly/weekly archive files. Treat a memory file as a historical archive (and
  exclude it from the carry-forward list) if its filename or `MEMORY.md` description marks
  it as an annual/partial-year review or a ww-range archive (e.g. `project_2022_partial_review.md`,
  `project_2023_annual_review.md`) — those are background only, not open threads.
- **This week's notes**: files directly inside `wwl_notes/` (do NOT descend into
  `wwl_notes/archive/`).

## Steps

1. List files directly inside `wwl_notes/`. If empty, proceed anyway — draft using memory
   alone, with every thread marked "no update provided this week," and say so plainly in
   the draft and in your summary to Yu Han (don't silently invent activity).
2. Read every file found in `wwl_notes/` fully.
3. Read `MEMORY.md`, then read each **active** project-type memory file (per the exclusion
   rule above) to get the latest recorded status of each named initiative/thread.
4. For each active thread:
   - If the week's notes mention it, draft a short "This week:" delta line using ONLY what
     the notes state — never infer or embellish beyond what's written.
   - If the notes don't mention it, carry forward its last known status as a one-line
     summary, explicitly marked "(carried forward, no update this week)".
5. For any content in the week's notes that doesn't clearly map to an existing active
   thread, list it separately under "Possible New Items (not yet in memory — verify)"
   rather than folding it into an existing thread or silently dropping it.
6. If a note's content contradicts the last known memory state, or is ambiguous about
   which thread it belongs to, do NOT guess — flag it by name in a "Needs Your Input"
   section of the draft instead of resolving it silently.
7. Write the draft to `wwl_outbox/YYYY-MM-DD-wwl-draft.md` (today's date). If a file for
   today already exists, write to `YYYY-MM-DD-wwl-draft-2.md` (increment) rather than
   overwriting it.
8. Only after the draft file is written successfully, move each processed file from
   `wwl_notes/` to `wwl_notes/archive/` unchanged. Never delete a file. Leave `README.md`
   in place (it's the skill's own instructions, not content).
9. Report a concise summary to Yu Han:
   - Which notes files were processed and archived.
   - How many threads got a delta vs. were carried forward with no update.
   - Any items flagged under "Possible New Items" or "Needs Your Input".
   - The path to the draft file.

## Draft format

```markdown
# WWL Draft — In Progress (week of YYYY-MM-DD)
_Draft only — review before using. Carry-forward + delta generated from memory + wwl_notes/._

## [Thread name]
- Status: [last known status, one line]
- This week: [delta from notes, OR "(carried forward, no update this week)"]

...(repeat per active thread)...

## Possible New Items (not yet in memory — verify)
- [note content that didn't map to a known thread]

## Needs Your Input
- [any contradiction or ambiguity found, named specifically]
```

## Guardrails

- Never fabricate a status update — only draft what memory already records or what this
  week's notes explicitly state.
- Never delete any file, in `wwl_notes/` or elsewhere.
- Never write outside `wwl_notes/`, `wwl_notes/archive/`, and `wwl_outbox/` (plus read-only
  access to the memory folder above).
- Never send, post, publish, or merge anything — this produces a draft file only. Yu Han
  decides what happens to it next.
