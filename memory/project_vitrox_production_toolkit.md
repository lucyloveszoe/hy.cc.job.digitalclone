---
name: project-vitrox-production-toolkit
description: "WSD Production Toolkit built for ViTrox OneSolution T&R outsourcing (decrypt DPAT .olf / LBF .olbf files), completed Jan 2025"
metadata:
  node_type: memory
  type: project
  originSessionId: c155a9e1-a0c1-49e4-a420-87a1f4788eeb
  modified: 2026-08-24T02:05:22.806Z
---

Martin requested a toolkit (against a T&R-outsourcing roadmap running through 2028) so ViTrox's OneSolution platform can decrypt WSD's DPAT outlier files (.olf) and LBF output files (.olbf) — a command-line tool, no Clotho changes needed. SRS passed internal review 12/04/2024, Martin confirmed green light 12/13/2024. Kin Lum built and shipped it as "WSD.ProductionToolkit": code + unit tests, InnoSetup installer, SDD, user guide, and a public distribution site (https://sites.google.com/broadcom.com/wsd-production-toolkit). Demoed to Martin 01/14/2025, who is distributing the installer to ViTrox; marked complete on target, 01/17/2025.

**Why:** first concrete deliverable in the multi-year ViTrox T&R-outsourcing relationship — establishes the toolkit distribution pattern (public Google Site + versioned installer) WSD now uses for external-vendor-facing tools.

**How to apply:** if ViTrox or another T&R outsourcing vendor needs new WSD file-format support, extend WSD.ProductionToolkit rather than building a new one-off tool.
