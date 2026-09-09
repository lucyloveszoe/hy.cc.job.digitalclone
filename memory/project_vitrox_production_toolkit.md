---
name: project-vitrox-production-toolkit
description: "WSD Production Toolkit built for ViTrox OneSolution T&R outsourcing (decrypt DPAT .olf / LBF .olbf files), completed Jan 2025"
metadata:
  node_type: memory
  type: project
  originSessionId: c155a9e1-a0c1-49e4-a420-87a1f4788eeb
  modified: 2026-09-08T06:29:00.492Z
---

Martin requested a toolkit (against a T&R-outsourcing roadmap running through 2028) so ViTrox's OneSolution platform can decrypt WSD's DPAT outlier files (.olf) and LBF output files (.olbf) — a command-line tool, no Clotho changes needed. SRS passed internal review 12/04/2024, Martin confirmed green light 12/13/2024. Kin Lum built and shipped it as "WSD.ProductionToolkit": code + unit tests, InnoSetup installer, SDD, user guide, and a public distribution site (https://sites.google.com/broadcom.com/wsd-production-toolkit). Demoed to Martin 01/14/2025, who is distributing the installer to ViTrox; marked complete on target, 01/17/2025.

**Separate deliverable in the same Vitrox relationship — VR20 2DID list / zDB Type 58 (shipped 09/22/2025)**: distinct from WSD.ProductionToolkit above (that's a decrypt utility WSD gives to Vitrox; this is a new zDB *collection* type WSD ingests from Vitrox). Requested by Rebecca/Kok-Kit (05/06/2025) to support Vitrox's VR20 FVI 2DID production data, held pending real Vitrox production output (received 3 sample files 08/11/2025, 5 real files 08/18/2025). Tingyu/Kin Lum (owners) built a new feeder (Type 58 task stored procedures, new `t58` feeder-queue tables `fvifeed`/`fvifeedlog`, upgraded all feeders' shared zDB libraries) plus WUDAS/Spotfire Data Function support for FVI 2DID drill-down (top-level `fvi_product`/`fvi_lot` tables). Marked Complete 09/22/2025 against its (twice-slipped) 09/22 target.

**Why:** first concrete deliverable in the multi-year ViTrox T&R-outsourcing relationship — establishes the toolkit distribution pattern (public Google Site + versioned installer) WSD now uses for external-vendor-facing tools.

**How to apply:** if ViTrox or another T&R outsourcing vendor needs new WSD file-format support, extend WSD.ProductionToolkit rather than building a new one-off tool.
