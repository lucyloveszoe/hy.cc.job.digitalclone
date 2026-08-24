---
name: project-radar-jira-service-manager
description: "WSD Radar Service Manager — syncs Raptor's Radar ticketing system into Broadcom GTO Jira, replacing WCC's legacy Python daemon"
metadata:
  node_type: memory
  type: project
  modified: 2026-08-24T05:53:13.241Z
  originSessionId: 60cad4c7-e6f3-4ac2-9f91-af516207847b
---

Built and owned by WSD SW team (Jiapeng/Chao, with Kin Lum on the DB side): a one-directional sync service (Rdr → Jira only, never the reverse — a mandatory philosophy set by Raptor/WCC) that ports Raptor's legacy WCC "Radar2Jira" Python/SVN daemon into a WSD-hosted, Windows-based service writing into Broadcom GTO Jira (project WSDRDR). Built from Radar API (read) + Jira API (create/update) + an Aurora "Golden Copy" DB + Box (for attachments >2GB) — 5 modules: S3API, DBAPI, BoxAPI, MappingAPI, Exception Handling.

**Phase I timeline**: prototype target/completed 01/16/2023 (per [[project-2023-annual-review]]); trial run loaded Nov/Dec 2023 Radar problems; full 2-years-of-history backfill completed 01/12/2024; went fully live with notifications 01/16/2024; monitored ~6 weeks with no missing/mismatched problems, then formally closed Phase I and entered stabilization 02/27/2024 ("accumulate non-critical requests for next couple months before major upgrade").

**Post-release maintenance (2024)**: added a "Raptor Program" Jira attribute (Feb 2024); adjusted the sync polling window to a 1-hour overlap to avoid dangling tickets after finding a manual/auto process overlap gap; log-file rotation bug fixed (two WCC-inherited log handlers writing the same file caused rotation permission errors); Radar password rotation support added. Ongoing "Radar-JIRA-Sync Support" continues as a steady-state maintenance item through year-end 2024.

**Known future risk (as of Dec 2024)**: Raptor/WCC confirmed they plan to deprecate the current cookie-based Radar API authentication in favor of OpenID Connect (OIDC), with an initial target of 09/30/2025. WSD is waiting on WCC/Raptor to finalize and share the new auth code before it can update the Radar-Jira Sync Service.

**Phase II** ("WSD Radar Service Manager — Production Deployment") remains listed under "Middle Term In-Queue Projects" as of year-end 2024 — not yet started.

**OIDC deprecation risk detail (04/08/2025)**: the current cookie-based auth is specifically a **"DAW cookie"**; separately, Apple has started collecting client script environment settings from users, suggesting prep work is underway on Raptor/Apple's side ahead of the 09/30/2025 OIDC cutover. No new timeline or code from WCC/Raptor yet — WSD is still waiting.

**Support ticket closed, but deprecation risk itself unresolved (09/16/2025)**: Jiapeng got a WCC/Raptor update confirming **"no change in near term"** — i.e. Raptor is not actually moving to OIDC on the original 09/30/2025 timeline. WSD's own "Radar-to-Jira API Security Update Support" tracking ticket was closed on this basis (nothing actionable on WSD's side right now), but that just reflects Raptor's inaction, not that the DAW-cookie deprecation risk went away — treat the 09/30/2025 date as likely slipping, and reopen tracking if Raptor/WCC resumes movement.

**Why:** a durable, still-active integration between an external partner system (Raptor/WCC's Radar) and WSD's internal GTO Jira tracking — the OIDC auth deprecation is a concrete forward-looking risk to watch.

**How to apply:** update this file when the OIDC migration work starts/lands (watch the 09/30/2025 Raptor deadline), or when Phase II production deployment is initiated. Don't confuse this with [[project-mythos-security-scanning]] (unrelated Jira-adjacent program) or WSD's own internal GTO Jira project-boarding effort (see [[reference-wsd-systems]]).
