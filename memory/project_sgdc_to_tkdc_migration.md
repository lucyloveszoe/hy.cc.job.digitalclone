---
name: project-sgdc-to-tkdc-migration
description: "GTO mandate to move all R&D assets/servers out of Singapore DC to Tokyo DC or LVN DC by early 2026, including Spotfire clusters"
metadata:
  node_type: memory
  type: project
  originSessionId: c155a9e1-a0c1-49e4-a420-87a1f4788eeb
  modified: 2026-08-24T05:58:51.321Z
---

**Kicked off 09/19/2025**: GTO (Suryanto) informed WSD that all R&D assets must be moved out of the Singapore DC, to either Tokyo DC or LVN DC — this explicitly includes WSD's existing **Spotfire clusters**. Target timeline: **early 2026**. Open question at kickoff: whether Windows-based servers are in scope at all (Linux servers are confirmed in scope); WSD is compiling the full inventory of impacted services/applications.

**09/23/2025 progress**: Choo-Yau/Kin Lum/YK submitted a "Window Server Build" ticket for 16 new VMs at the Tokyo Colo (TKY) — 10 for Choo-Yau/YK, 6 for Kin Lum — as a hedge in case Windows servers do need to move, even though GTO itself (Vasudev, then escalated to Krishna → Manjunath → Deepak Upadhyay) still hasn't confirmed whether SGN Windows servers must migrate or can stay. Treat the Windows-server scope as genuinely unresolved, not just slow-to-confirm.

**Overlap with the Spotfire DMZ migration**: this GTO-mandated DC move covers the same Spotfire clusters that [[project-aws-resiliency]] tracks moving from the DMZ zone to the internal zone (prototype install started 09/16/2025). These are two distinct GTO initiatives that both touch Spotfire infrastructure on similar timelines — check both files together before assuming Spotfire's current migration plan is final, since a DC relocation could change or duplicate the DMZ re-IP work.

**Dropped — WSD Windows servers stay in Singapore (10/14/2025)**: GTO (Vasudev/Rajasekhar) confirmed (1) all WSD Windows VM servers will remain in SGN as-is, (2) the `wsdcifs.pnginas03.sgn.broadcom.net` CIFS share storage also stays in SGN, and (3) the storage team will give WSD at least 2 months' notice before any future migration. WSD's own preference (Choo-Yau/YK/Kin Lum) was "option 2-1" — the most straightforward, minimum-workload path — after GTO separately rejected WSD's request to grab extra resources at the Tokyo instance. **This initiative is closed; the Spotfire DMZ-zone migration in [[project-aws-resiliency]] proceeds as its original SG-DC (not Tokyo) plan**, with no DC relocation to reconcile after all. Treat this file as historical — no further action expected unless GTO revisits with the promised 2-month notice.

**Why:** a GTO-driven relocation mandate that was ultimately reversed rather than executed — worth keeping as a reference in case GTO revisits SG DC consolidation later.

**How to apply:** no open action items remain. If GTO raises a similar SG-DC relocation request again, check this file for the prior outcome and reasoning before treating it as a first-time ask.
