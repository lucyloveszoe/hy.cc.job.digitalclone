---
name: log
description: Process new files in inbox/ into the project's persistent memory (user/feedback/project/reference types), then archive the source file. Invoke with /log after dropping notes, meeting recaps, decisions, or exported docs into inbox/.
---

# /log — Process Inbox into Memory

This skill implements "Proposal A" of Yu Han's digital-clone project: a passive,
manual-trigger pipeline that turns raw drop-in files into structured, durable memory.
It never sends, publishes, or executes anything — read and file only.

## Memory location

The memory store for this project lives at:
`C:\Users\hany\.claude\projects\C--Temp-AllCode-Claude-hy-cc-job-digitalclone\memory\`

`MEMORY.md` in that folder is the index. Individual memory files sit alongside it.
Follow the memory frontmatter format and the four types (user/feedback/project/reference)
exactly as defined in the global CLAUDE.md instructions.

## Steps

1. List files directly inside `inbox/` (do NOT descend into `inbox/archive/`). If empty,
   report "Nothing new to process." and stop — do not touch memory or MEMORY.md.
2. For each file, in order:
   a. Read it fully (use the appropriate reader for its format — text, docx, pdf, image).
   b. Identify what it contains. A single file can yield zero, one, or several memories:
      - Facts about Yu Han's role, responsibilities, or knowledge → **user** memory.
      - A correction or confirmation about how to approach work → **feedback** memory.
      - A decision, initiative, deadline, or incident context → **project** memory.
      - A pointer to where information lives in an external system → **reference** memory.
      - Routine content with nothing durable (e.g. pure logistics, no new fact/decision/
        preference) → file nothing. Note this in the summary rather than forcing a memory
        into existence.
   c. Before creating a new memory file, check existing files in the memory folder for the
      same topic. If one already covers it, update that file in place rather than creating
      a duplicate — per the "no duplicate memories" rule.
   d. Write/update the memory file using the required frontmatter (name, description,
      metadata.type) and, for feedback/project types, the rule/fact + **Why:** + **How to
      apply:** structure.
   e. Add or update a one-line pointer in `MEMORY.md` for any new file.
   f. If anything in the file is ambiguous, contradicts existing memory, or looks like it
      might be sensitive (e.g. HR matters, confidential deal terms), do NOT guess — leave
      it unfiled and flag it by name in the final summary for Yu Han to clarify.
   g. Only after the memory write(s) for this file succeed, move the source file from
      `inbox/` to `inbox/archive/` unchanged. Never delete a file.
3. After all files are processed, report a concise summary to Yu Han:
   - Which files were processed and archived.
   - Which memory files were created or updated, and their type.
   - Anything skipped or flagged for clarification, and why.

## Guardrails

- Never fabricate content for a memory — only file what the source file actually states.
- Never delete any file, in inbox or elsewhere.
- Never write outside `inbox/`, `inbox/archive/`, and the memory folder above.
- Never take any action beyond reading and filing (no sending, posting, or executing).
