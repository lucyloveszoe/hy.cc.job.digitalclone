---
name: project-wsd-logistics-data-integrity
description: "IDI (Integrity Data Infra) — cross-tool design-data integration (die/PCB/EBR footprint matching across ADS/Cadence/Allegro) requested by Wingra, later renamed from 'WSD Logistics Data Integrity'"
metadata:
  node_type: memory
  type: project
  modified: 2026-08-24T03:55:02.233Z
  originSessionId: 60cad4c7-e6f3-4ac2-9f91-af516207847b
---

Initiated 02/21/2024 as "WSD Logistics Data Integrity" per a request from Wingra, later renamed **IDI (Integrity Data Infra)**. Owners: Chao/Jiapeng (with KinLum). Problem framed by KP as a classic "day-1" cross-vendor design-tool integration gap: WSD's design flow spans multiple vendor tools (ADS, Cadence, Allegro) with inconsistent naming/formats, so die-layout footprints, PCB/SIP footprints, HFSS components, and EBR (assembly) diagrams drift out of sync — symbols get mixed up, pin numbering doesn't match, obsolete footprints get reused. Wingra's ask: a central database (cloud-hosted, WSD owns technical ownership) that each design tool/role can pull the *current* footprint/symbol from, plus an API and partial web UI, plus visualization for part of the data.

**Scale cited by Wingra (Feb 2024)**: ~166 users checking out SIP-footprint licenses, ~117 unique HFSS-component users, ~175 unique IC-layout users, plus "likely the whole test team" for the EBR side — i.e. this touches most of WSD's hardware design/NPI population, not a niche tool.

**Scope/phasing (per Wingra's multi-phase proposal, shared ~mid-2024)**: Phase 1 = Die Tapeout Process; Phase 2 = "Detour" (further phases undefined in memory). Chao/Jiapeng ran PCB SIP data collection work as part of Phase 1 (clarifying cloud-vs-Broadcom hosting and DB-direct-access-vs-API questions with Wingra).

**Status at year-end 2024**: Wingra shared a "Post-Phase-1-Release Plan" around 07/31/2024, implying Phase 1 (Die Tapeout Process) reached a release milestone. Subsequent phases are tracked as **"IDI Phase-N" and marked Hold** with no firm target date, and a further delay was reported by Wingra/DA as recently as 12/17/2024.

A fuller phase list surfaced 01/07/2025: Phase 1 = Die Tapeout Process; Phase 2 = "Detour"; an unspecified further phase = 3D Components; Phase 3 = Module Tapeout; Phase 4 = EBR — pending a 04/01 review (year not specified in source, presumed 2025).

**Why:** a substantial, multi-hundred-user cross-tool data-integrity initiative that WSD SW team took technical ownership of at Wingra's request — distinct from WSD's own production-test data pipeline work (zDB/WUDAS/Clotho), this is design-side (pre-production, NPI/R&D) data.

**How to apply:** search for "IDI" or "Integrity Data Infra" (not "WSD Logistics Data Integrity") when looking for later updates — the project renamed early in its life. Treat Phase 1 (Die Tapeout) as likely shipped by mid-2024; treat later phases as stalled/on-hold as of the most recent record (Dec 2024) — verify current status before assuming further progress.

**Auth decision (02/04/2025)**: Wingra requested Jiapeng *remove* authentication entirely from IDI's search and advancesync API endpoints. Yu Han rejected that request and instead directed that authentication be added back — but via the OKTA-based ID.Pass token mechanism (see [[project-okta-sso-jwt-architecture]]) rather than NTLM, so engineers never type credentials directly. All "WRITE" operations (upload, adv-sync) must be authenticated and authorized; Jiapeng completed the change and deployed to staging. Why: a request to weaken security got redirected toward a more secure *and* more usable mechanism rather than simply declined or granted as-is.

**IDI auth pattern finalized (02/11/2025)**: IDI's web portal uses Cookie/OKTA auth; its `idi.data` S3 access API uses the ID.Pass token directly (issuer `brcm.wsd.swteam.idpass`) with **in-app ACL** authorization instead of the short-lived Access JWT — same pattern adopted for TAP (Phase 1). Separately, the DA team (Alex) asked IDI to instead support their own custom "DA-Token"; Yu Han declined in favor of the single ID.Pass mechanism — see [[feedback-engineering-principles]].

**Possible reversal, unconfirmed (02/18/2025, still the active direction as of 02/25/2025)**: the report states Chao synced with Alex and "agreed to support DA proposed option" (the custom DA-Token), which on its face contradicts Yu Han's 02/04/2025 rejection above. By 02/25/2025, "Chao and Alex aligned with new DA provided JWT Token method" and work was proceeding on that basis — so this is not a one-off remark, it's the direction actually being executed. Still not clear from the source whether Yu Han himself signed off on this reversal — flag and confirm current status before assuming either the ID.Pass-only principle or the DA-Token approach is Yu Han's actual intent.

**Now deployed to production, still unconfirmed (03/10/2025)**: "(Chao/Alex) Completed alignment with DA provided JWT Token method for IDI Integration. Deployed @ production portal" — the DA-Token mechanism is live in production for IDI, not just in development, with no Yu Han sign-off visible in any weekly report through this date. See [[project-okta-sso-jwt-architecture]] for the full note — this needs a direct confirm-or-unwind conversation with Yu Han/Chao given it's already shipped.

**End-to-end flow not fully live yet (03/18/2025)**: WSD's side is deployed as above, but the report adds that Chao is still waiting for Alex (DA) to finish integrating the token into DA's own upload script — so the DA-Token path isn't fully exercised end-to-end despite WSD's production deployment. Yu Han sign-off is still not recorded.

**DA-Token item CLOSED by Yu Han — administratively, not technically resolved (05/06/2025)**: Yu Han closed the "Integrate IDI (Phase 1) with Broadcom OKTA and Either ID.Pass Token or DA-Token" item outright, citing "2 months no update from DA team and no push from project requestor." The underlying end-to-end gap from 03/18/2025 (Chao waiting on Alex to finish integrating the token into DA's upload script) was still unresolved at closure — this was a close-by-inactivity, not a decision that DA-Token is the approved approach. Full detail in [[project-okta-sso-jwt-architecture]]; if DA/IDI auth resurfaces, the original 02/04/2025 ID.Pass-only ruling above is still the standing architectural principle.

**Schedule slip (02/11/2025)**: per the SW Team 3-Year Roadmap refresh, IDI's plan was pushed back **1 quarter** due to a Phase 1 integration delay (the "Security" workload's Phase 3 Migration+Integration was folded into Phase 2 to compensate, since Phase 1 Backbone progress was ahead of schedule) — see [[project-clotho-nxg-magnus-roadmap]].
