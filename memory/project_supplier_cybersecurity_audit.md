---
name: project-supplier-cybersecurity-audit
description: "CM/subcon cybersecurity resiliency audit initiative, driven by Raptor/Apple Information Security scrutiny of Broadcom's Ft. Collins fab"
metadata: 
  node_type: memory
  type: project
  originSessionId: c155a9e1-a0c1-49e4-a420-87a1f4788eeb
  modified: 2026-08-24T02:10:05.018Z
---

Yu Han-led investigation into CM/subcon suppliers' cybersecurity robustness and business-continuity resilience, initiated per a Mike/KP 1:1 request: "In view of Raptor's recent scrutiny in FtC manufacturing cybersecurity policy... need internal initiative to conduct audit to our key subcons... especially those we have integrated data infrastructure critical for manufacturing operation continuity." Tracked in weekly reports from ~09/30/2024 through at least 01/07/2025 (ww01/2025).

Scope limited (per KP/Mike, 10/01/2024) to CMs/operations with cloud-software integration for continuous production operations — any cyber attack that could stall production. Strategy: help each CM build its own self-audit checklist so they can audit themselves on a regular/ongoing basis, rather than WSD auditing them continuously (no resources for that long-term) — WSD conducts the 1st round to establish the process. Mark McGee flagged Raptor/AIS (Ramji Kannusamy) requesting an on-site FtC visit; Broadcom already holds an ISO 27001 ISMS certificate + annual SOA; GSSM CyberSecurity team (Stephen Sun) already runs supplier questionnaires (Melanie responding). As of 12/02/2024: Chipbond is ISO/IEC 27001 certified; Inari is not yet, but has ISO 14001 + annual internal/external ITGC audits + an annual 3rd-party penetration test.

Ken volunteered (12/17/2024) as SW team's point of contact. SW team brainstormed a WSD-specific (not generic) improvement to-do list to propose to CM/subcons: isolate WSD servers into a domain separate from CM testers; WSD server local admin must not be a CM regular domain user; require dual ISP providers; require A/B dual deployments across separate hardware; enforce routine anti-virus/vulnerability scanning (incl. moving off .NET's EOL Zip library and scanning password-protected zips); pre-process virus scan on S3 uploads; keep frameworks current (retire EOL .NET 6 for .NET 8); remove TeamViewer in favor of DMZ/RDP-on-nonstandard-port or CM VPN with MFA (as of 01/2025 only Chipbond and ASEKr had MFA OTP enabled — all other CMs including Inari still used TeamViewer). Also mapped a critical-path-of-failure matrix per CM: if CM Internet is down but Intranet is up, Clotho/LBF/MQTT/pCD (share-folder mode) still work but DPAT etc. do not; if CM Intranet is down, only Clotho still works.

As of 01/03/2025, Ken shared a further WSD-relevant findings summary, pending polish/consolidation against the "superset" Raptor questionnaire.

**Why:** risk-driven initiative pushed down from senior leadership (via Raptor/Apple's scrutiny of Broadcom's own fab), not routine hardening — Yu Han is the point of contact upward to Mike/KP.

**How to apply:** distinct from [[project-aws-resiliency]] (WSD's own 2026 AWS-region-outage-driven initiative) — this one is about *subcontractor/CM-side* cybersecurity posture, predates it by over a year, and is subcon-facing rather than AWS-infra-facing. Don't conflate the two despite both having "resiliency/cybersecurity" in scope.

**Vulnerability scanning tool decision (01/21/2025)**: Ken evaluated options for the team's own vulnerability scanning — Cycode (BRCM Github Team's tool) had produced no reports since early 2023, effectively dormant; Qualys was skipped; **GitHub Dependabot** was confirmed working and easy for repo owners to enable, so it became the team's chosen dependency-alert mechanism (WSD lacks a GitHub Advanced Security license, so code-scanning/secret-scanning stay unavailable for now). This predates and is distinct from the 2026 GTO-mandated Mythos scanning program in [[project-mythos-security-scanning]], which supersedes this ad hoc effort once it starts.

**Closed 02/04/2025**: the overall supplier cybersecurity/resiliency investigation was marked complete — Ken/YK tagged the WSD-internal to-do list against Raptor's questionnaire item numbers and Yu Han shared consolidated V1 with PE (Chun Kiat). Follow-on execution work continues under two separate, still-open tracking items: "Cybersecurity and Resiliency Internal Improvements" (Yu Han formally requested BRCM GitHub owner Ravi integrate WSD's GitHub org with Cycode; pending Ravi's action and AI-ownership assignment) and "Cybersecurity and Resiliency Sync with CM/Subcons" (pending a joint PE+CM/Subcons+SW sync).

**Progress (02/11/2025)**: Cycode has now been technically integrated into the BRCM tool suite (SW team attended a Cycode demo/review session) — but PR-blocking enforcement (to force-fix flagged vulnerabilities before merge) is not yet enabled, pending further update from Ravi. The internal-improvements tracking item was broken into 5 prioritized action items covering transfers/parsing/Clotho/pCD scanning (high priority, not urgent), CMAC auto-updater gaps (can't yet handle "initial bootstrap," only upgrade/downgrade), GuardDuty S3-scan size limits (5GB cap, low priority), and storing app/service install info as IaC.
