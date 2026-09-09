---
name: project-workspaceone-migration-impact
description: "BRCM IT's migration of the Windows client image to 1.Secure (Workspace ONE), which broke personal-PC share-folder access to WSD apps (Clotho, pCD, PPYCC, Spotfire, zDB UI) starting mid-2024"
metadata: 
  node_type: memory
  type: project
  modified: 2026-09-07T22:14:57.686Z
  originSessionId: 59aa9cac-0722-4254-85a2-83d236cbea48
---

BRCM IT migrated the Broadcom Imaged Standard Client (Windows) to "1.Secure" (Workspace ONE), which broke personal-PC access to network share folders — impacting Clotho License Requestor, pCD package download, zDB folder upload, Spotfire's Standard Data Function (which referenced an intranet share folder), and zDB UI.

**WSD SW team response (Choo-Yau/YK), 07/11/2024**: fixed Spotfire's `wsd_share.R` data function to stop referencing the intranet server share folder; verified Clotho still works end-to-end (download pCD package, upload to zDB folder) on a migrated PC; published a UserGuide for end users to self-resolve, shared with Penang users. By 07/16/2024 the guideline was confirmed working for ~30% of WSD users (APAC). Chao separately upgraded the Clotho License Requestor/Service so it works for users now in a Workgroup (not domain-joined) configuration.

**Process gap flagged by Yu Han**: end users must now manually re-enter their credentials after every password change — IT intends to eventually automate this but hadn't as of this writing.

**Closed 08/27/2024**: full SW team (both Penang and San Jose members) completed their own WorkspaceONE migration with no further issues; marked Complete. The guideline/UserGuide-based self-resolution approach was the standing fix — no further remediation needed for the client-migration impact itself.

**Related SJ IT change flagged 08/06/2024, still on Hold as its own separate migration (09/17/2024)**: San Jose Data Room servers (including the LVN DC) are planned to move to a new 10Gbps server room and out of the BRCMLTD domain entirely, with IP changes from 135.x to 10.x — affects SJ Spotfire among others. Requirement: these servers must retain both internet access and lab-network reachability after the move. As of 09/17/2024 still pending further step updates from the DA team — this is a distinct, still-unresolved migration from the (now-closed) WorkspaceONE client migration above; don't conflate the two.

**Why:** an externally-imposed BRCM IT client-security change with real production-app blast radius across most of WSD's desktop-facing tools — not something WSD chose or can decline, similar in kind to [[project-aws-vpn-sdwan-migration]].

**How to apply:** if a user reports Clotho/pCD/PPYCC/Spotfire/zDB-UI suddenly failing on a personal PC, check whether their machine was just migrated to Workspace ONE before assuming an app-level bug — point them to the UserGuide first. Watch for the SJ Data-Room-server domain/IP change landing and potentially needing firewall/IP-allowlist updates similar to other 135.x-domain services.
