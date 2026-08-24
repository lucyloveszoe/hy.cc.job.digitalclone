# Memory Sync — Backup & Restore

This project has **two copies** of memory:

1. **Live memory folder** (outside the repo) — the only location `/log` ever writes to.
   Path pattern: `C:\Users\<you>\.claude\projects\<encoded-repo-path>\memory\`
   The `<encoded-repo-path>` segment is a direct encoding of this repo's absolute path
   (drive letter, folders, dots all turned into dashes). Today that's:
   `C--Temp-AllCode-Claude-hy-cc-job-digitalclone`
   for the repo at `C:\Temp\AllCode\Claude\hy.cc.job.digitalclone`.

2. **In-repo snapshot** — `memory/` at the repo root. Committed to git (this repo is
   private/Broadcom-internal, so that's an accepted risk). This is a manual backup, not
   a live copy — nothing auto-syncs it.

`inbox/archive/` (the original source PDFs/docx) is **gitignored** on purpose — binary
files bloat git history. Those originals are stored separately by you, outside git.

---

## Direction A — Backup: live memory → repo (routine, after `/log` runs)

Do this whenever you want the in-repo snapshot to reflect the latest memory state
(e.g., after a `/log` run). Manual, on-request only — nothing does this automatically.

1. Ask Claude (or run directly) to copy the live folder over the repo copy:
   ```
   cp -r "C:\Users\<you>\.claude\projects\<encoded-repo-path>\memory\." memory/
   ```
2. Verify no unexpected diff:
   ```
   diff -rq "C:\Users\<you>\.claude\projects\<encoded-repo-path>\memory" memory/
   ```
3. Review with `git status` / `git diff`, then commit yourself:
   ```
   git add memory/
   git commit -m "sync memory snapshot"
   ```

---

## Direction B — Restore: repo → new machine's live memory folder

Do this when setting up this project on a new machine (reinstall, new laptop, etc.).

**Step 1 — Check out the repo.**
Ideally to the **identical absolute path** it lived at before (same drive letter, same
folder structure). This matters because of the encoding rule above — same path in ⇒
same encoded folder name out ⇒ steps 3-4 below become a pure copy with no guessing.
If you can't match the path exactly, see the note at the end.

**Step 2 — Restore original source files into `inbox/archive/`.**
Copy the PDFs/docx from wherever you stored them separately back into
`inbox/archive/` in the fresh checkout. This folder is gitignored, so git checkout
alone won't bring them back — restoring them is a manual copy step. `/log` never
reads `inbox/archive/`, so this restore is safe and won't trigger reprocessing.

**Step 3 — Locate (or create) the new machine's live memory folder.**
- If the repo path matches the old machine exactly: the target folder is
  `C:\Users\<new-user>\.claude\projects\<same-encoded-path>\memory\` — create it if it
  doesn't exist yet (e.g. open one Claude Code session in the checked-out repo first,
  which creates the project folder automatically).
- If the repo path is different on the new machine: open a Claude Code session in the
  new checkout first, then find whatever project folder it created under
  `~/.claude/projects/` on that machine — that's your real target, not the old
  encoded name.

**Step 4 — Copy the memory snapshot in.**
```
cp -r memory/. "C:\Users\<new-user>\.claude\projects\<encoded-path>\memory\"
```
No reload/reindex step needed — `MEMORY.md` auto-loads into every session once it's
in the right folder.

---

## Key gotchas to remember

- The live memory folder name is **derived from the repo's absolute path**, not random —
  keep checkout paths identical across machines to make restores trivial.
- `/log` **always** writes to the live folder, never to `memory/` in the repo. The repo
  copy is a snapshot you refresh manually (Direction A) — it will silently go stale if
  you forget to re-sync after running `/log`.
- `inbox/archive/` originals are never in git — restoring them (Step 2) is always a
  separate manual copy from your own backup, independent of the git checkout.
