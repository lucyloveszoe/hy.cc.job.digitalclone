---
name: feedback-slide-deck-style-preservation
description: "When a generated slide/doc artifact gets manually restyled, edit the file in place going forward — never regenerate from the original creation script"
metadata: 
  node_type: memory
  type: feedback
  modified: 2026-08-29T23:56:10.526Z
  originSessionId: b0525ac4-10e5-4e76-ab95-8c4cdf77016d
---

When Yu Han manually edits the style/theme of a generated artifact (e.g. a pptx built
via a pptxgenjs script), all follow-up updates to that artifact must preserve his
manual styling — never regenerate from the original creation script, since that wipes
the manual changes back to the script's original theme.

**Why:** happened concretely on `brainstorm/QueryCopilot_Highlight.pptx` (2026-08-29).
It was first generated via a pptxgenjs script (navy/ice-blue/teal palette, Georgia+
Calibri fonts). Yu Han then manually restyled it directly in PowerPoint (charcoal
`3B4652` background, cyan `0098C7` primary accent, deep blue `005C8A` secondary accent,
teal `007B8C` highlight, gray `53565A` body text, light gray `EEEFF0` panels, all text
run-level overridden to Arial — the underlying theme1.xml still says Calibri, so new
text must explicitly set `fontFace: Arial` rather than relying on theme defaults). He
then explicitly said to follow the new style for all follow-up updates.

**How to apply:** before editing any such artifact again, unpack/inspect the current
file's actual XML (colors, fonts, exact text) rather than trusting a prior generation
script's constants — the file itself is the source of truth once hand-edited. Use the
pptx skill's edit-in-place workflow (unpack → edit XML → clean → pack) for future
content changes to this specific file, not pptxgenjs regeneration. This generalizes
beyond this one file: treat any user hand-edit to a Claude-generated artifact as
overriding whatever produced it, and switch that artifact to edit-in-place mode.
