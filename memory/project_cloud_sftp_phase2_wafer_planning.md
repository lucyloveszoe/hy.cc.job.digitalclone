---
name: project-cloud-sftp-phase2-wafer-planning
description: Cloud SFTP Family Phase 2 — automating manual/semi-manual Internal Wafer Planning data types (Yellow/Red) beyond the original Cloud SFTP scope
metadata: 
  node_type: memory
  type: project
  originSessionId: c155a9e1-a0c1-49e4-a420-87a1f4788eeb
  modified: 2026-08-24T06:21:49.040Z
---

Per a KP request, Choo-Yau/YK are assessing whether it's technically and financially feasible to automate file transfer and data parsing for "Yellow" and "Red" Internal Wafer Planning data types — file types that were explicitly NOT included in the original Cloud SFTP scope (Suraya's request), currently downloaded semi-manually/manually from Oracle, Incorta, or the FtC Custom Portal.

**Why:** these are inventory-planning inputs Suraya's team currently handles by hand; automating them extends Cloud SFTP's existing automation model to a new category of internal (not CM-sourced) data, pending an ROI assessment per file type.

**Progress timeline**: 04/22/2025 — began working with Suraya and each application owner to figure out automation approach (web API / auto-email / direct SFTP upload) per file type. 04/29/2025 — validated FtC API URL. 05/06/2025 — Suraya expanded scope to include some Planner-team financial data. 05/13/2025 — Suraya put sensitive-data processing on hold; an FtC API test showed data not tallying with the manual download, pending FtC follow-up. 07/07/2025 — FtC Item Master report API verified/bought off by Suraya; FtC provided a 2nd API (CSD report, with 2 previously-missing columns) also verified/bought off. 09/30/2025 — KP referred a new data source, FormFactor Probe core PB data, to be added via SFTP. 10/21/2025 — new CM "FormFactor" created and onboarded to Cloud SFTP. 12/02/2025 — FormFactor data upload confirmed working well; HockThien (FBAR PE) requested to produce a proper SOW for it. Incorta and Oracle automation remain pending — no update from Suraya as of 12/02/2025.

**How to apply:** update this file as FormFactor's SOW lands or as Incorta/Oracle automation progresses; treat "no update from Suraya" as the standing blocker on those two sources.
