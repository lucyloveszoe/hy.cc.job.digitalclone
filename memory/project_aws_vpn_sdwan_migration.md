---
name: project-aws-vpn-sdwan-migration
description: "BRCM IT-driven migration of the AWS-BRCM VPN tunnels from Cisco routers to VMware Velo SD-WAN, region by region (SG/Tokyo/Oregon)"
metadata:
  node_type: memory
  type: project
  originSessionId: c155a9e1-a0c1-49e4-a420-87a1f4788eeb
  modified: 2026-08-24T02:09:52.957Z
---

BRCM IT (Dean/Hemanth) is migrating the AWS↔BRCM VPN connections from traditional Cisco-router WAN to VMware Velo SD-WAN, across all AWS regions WSD depends on: Singapore, Tokyo, Oregon. WSD (Kin Lum/Tingyu/YK/Choo-Yau, with Yu Han) was pulled in starting 06/09/2024 as an unplanned "surprise request." First SG attempt (06/17-06/24/2024) failed: post-migration traffic routed through Mumbai instead of Singapore, breaking Spotfire connectivity and LDAP access — rolled back to Cisco. Root cause of the failed cutover: a VPC can only attach to one Virtual Private Gateway, so the team had to build a full new VPG+CG+VPN-Conn+Tunnel set via Terraform (IaC) rather than simply adding tunnels to the existing VPG.

Forcing deadline: GTO is decommissioning the Cisco routers entirely (Tokyo DC 01/17-01/20/2025, SG DC 02/07-02/08/2025), so the SD-WAN cutover had to complete before those dates.

**Status by 01/21/2025**: SG region migrated 01/04/2025 (Cisco→Velo), then IT requested a follow-up change 01/11/2025 adding a 2nd BRCM GW Peer IP to restore the original 2-connection/4-tunnel topology — connection+performance retest passed. Tokyo region completed 01/19/2025, performance check passed. Oregon (LV DC) migration requested by Dean 01/21/2025 on short notice, targeted 02/04-02/05/2025 — pending as of this filing.

**Confirmed (02/04/2025)**: Dean confirmed the Oregon (BRCM LVN ↔ AWS Oregon) VPN migration for 2025-02-05 09:30 SGT / 2025-02-04 17:30 PST — the last of the three regions.

**Closed (02/11/2025)**: Oregon migration completed as scheduled (2 VPN connections, 4 tunnels) with performance check passed. All three regions (SG, Tokyo, Oregon) are now on VMware Velo SD-WAN; marked complete. IaC record: `WSD.TF` `aws/vpc_or/vpc-wsd-uw2_vpn.tf`, `aws/vpc_sg/vpc-wsd-apse1_vpn.tf`, `aws/vpc_tk/vpc-wsd-apne1_vpn.tf`.

**Why:** senior-management-driven BRCM-wide infrastructure change, not something WSD initiated or can decline — but the Cisco decommission dates create a hard forcing function, and a failed cutover directly breaks production connectivity (Spotfire, LDAP, AWS access).

**How to apply:** if a WSD AWS service loses connectivity or shows latency/routing anomalies around a VPN cutover date, check this migration first before assuming an application-level bug. Verify Oregon's completion status before citing as done.
