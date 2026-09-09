---
name: project-automatic-tcf-creation
description: "Cross-region investigation into automating Test Condition File (TCF) creation from spec data — initiated Dec 2024, closed pending future PE/TDG agreement"
metadata:
  node_type: memory
  type: project
  originSessionId: 59aa9cac-0722-4254-85a2-83d236cbea48
  modified: 2026-09-07T23:10:16.605Z
---

A years-old ask ("this topic has been circulating for years") formally raised 12/17/2024 by Lan for SJ RF2 TCF automation specifically, then expanded (per Yu Han's framing) into a cross-region investigation spanning three TDG teams:
- **Seoul (Jay)**: Baku TCF changes.
- **San Jose (Mark/Turker/Steven/Mohamed)**: "Spectra to TCF Automation" — use Tacoma's APIs (or, as a fallback, download+parse the underlying zip files) to extract Spec information and build the TCF automatically. First validation case: Tx Leakage testing, targeted for a January 2025 first implementation.
- **Penang (JJ/Chor Ming)**: multiple related asks — a new Mirage2 1RF channel-list format (different from the conventional Keysight/R&S segment-table style, needs hardware-behavior-defined methods for channel/VST power/reference-level optimization); decoupling Quad-Site instrument VISA alias/assignment out of the TCF into an external XML file; a broader brainstorm on standardizing Clotho TCF — decoupling the PE-authored TCF from the test-program's condition-parsing code, and hosting TCF in Google Sheets/M365 with history tracking.

Yu Han's framing of the goal (12/17/2024): build one standardized, flexible, comprehensive workflow rather than doing bespoke implementation support per team's request — and if feasible, design it to cover RF1 and the new OneRF process, not just RF2.

**Closed 12/23/2024** — after KP/Mohamed coordinated a call with Steven, JJ, and Chung Sik, SW team closed this out of its own active pool, **pending future PE/TDG agreement before any SW involvement starts**. No SW development work was done; this was purely a cross-region requirements-gathering/coordination pass.

**Why:** captures an early, ultimately-shelved TCF-automation ask that predates any later TCF-automation initiative — if a similar request resurfaces, check whether it's a continuation of this exact investigation (same underlying Tacoma-API / Mirage2 / Quad-Site-decoupling threads) before treating it as brand new.

**How to apply:** this item is closed with no committed SW scope — don't assume any TCF automation exists or is in progress unless a newer memory records PE/TDG reaching agreement and SW picking the work back up.
