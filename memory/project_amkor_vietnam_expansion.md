---
name: project-amkor-vietnam-expansion
description: "pCD/zDB infrastructure expansion to support Amkor's Vietnam site (Blue Chip assembly relocation)"
metadata: 
  node_type: memory
  type: project
  originSessionId: b57dbea7-e0ff-4ed2-a55d-4625560a26cf
  modified: 2026-08-25T20:29:53.492Z
---

New CM site expansion: pCD/zDB infrastructure needs to extend to Amkor Vietnam (ATV) to support a Blue Chip assembly relocation targeted for around July 2026. Amkor's internal security/hardware review process has historically taken months (per past precedent with Amkor Korea), so this is flagged as a lead-time risk against the July target.

**Capacity strategy confirmed (ww25-31)**: KP confirmed both Amkor Korea (ATK) and Amkor Vietnam (ATV) will run WSD production. Longer-term direction: ATK legacy volume phases down toward ~0%, ATV scales up toward ~100%; worst case, Baku production could move fully to ATV by 2H 2027 if legacy-part qualification at ATK becomes impossible under the Raptor integration ([[project-nxg-pipeline-and-raptor]]).

**Infra decisions**: ATV gets 2 virtual servers in a DMZ with VPN access; ATK gets a Secure RDP + Site-to-Site IPSec upgrade. Approximate cost quotes: ATV new build ~$128,990, ATK-K4 upgrade ~$149,734. As of ww33, ATV also requires SFTP (not SMB/445) file access — a new "Add SFTP option for Transfer" task with a late-Sept/early-Oct 2026 deadline.

**Urgent SFTP work scoped and underway (ww34, 08/25/2026)**: "Urgent zDB Transfer & pCD Daemon Upgrade to add SFTP option to Support Bellagio Proto2 @ ATV" (Tingyu/Kin Lum), target date **09/25/2026**. Confirmed scope: ATV forbids SMB access from its DMZ to the share folder, so only SFTP works — needs an SFTP option added to both zDB Transfer and pCD Daemon; Clotho itself needs no change since it stays on the "internal" network; ILBF and DPAT confirmed not needed for this site. Given the urgency, the team is implementing this the traditional way rather than folding it into the schema-driven migration effort (see [[project-ai-friendly-schema-driven-migration]]) — Tingyu adding SFTP to zDB Transfer, Kin Lum adding SFTP to pCD Daemon. Business driver for the tight (late-Sept/early-Oct) deadline: it follows the Bellagio Proto2 ATV qualification timeline, because WSD is being pushed out of Amkor's K4 (Gwangju) site to make room for Amkor's AI customers, or would otherwise face a 20% price hike to stay.

**Why:** an external-partner-dependent infra rollout with a hard business deadline and a slow, non-WSD-controlled approval process, now escalating into a multi-year capacity-shift strategy between the two Amkor sites.

**How to apply:** treat the Amkor review timeline and the ATK→ATV capacity shift as the critical path/risk for this expansion, not WSD's own dev work. Update as the Amkor review and SFTP work progress.
