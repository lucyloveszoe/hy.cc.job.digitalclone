---
name: project-certificate-lifecycle-keyfactor
description: Broadcom CISO/GTO-driven certificate lifecycle automation (Keyfactor) shortening cert validity ahead of quantum-decrypt risk
metadata: 
  node_type: memory
  type: project
  originSessionId: b57dbea7-e0ff-4ed2-a55d-4625560a26cf
  modified: 2026-08-19T22:17:32.387Z
---

Broadcom-wide, CISO/GTO-driven initiative to automate certificate lifecycle management via Keyfactor, motivated by the long-term quantum-computing decryption threat. Certificate validity periods are being shortened on a schedule: 200 days (2026) → 100 days (2027) → 47 days (2029). This affects WSD's own certificates, not just a vendor concern. WSD owners: Ken, Kin Lum, Jiapeng, YK.

**Why:** a top-down security mandate with a multi-year compliance runway WSD must keep pace with, not a one-off IT task.

**How to apply:** when planning infra/deployment work that involves certificates, account for progressively shorter renewal cycles on this schedule.
