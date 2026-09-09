---
name: project-aws-singapore-capacity-risk
description: "AWS/Singapore power-ceiling risk to WSD's core AWS ap-southeast-1 (Singapore) footprint, raised Apr 2024 — 97% of WSD's ~800TB S3 data sits in this one region"
metadata:
  node_type: memory
  type: project
  modified: 2026-09-07T08:45:42.042Z
  originSessionId: 077ee51e-a0d3-4dac-9391-fb44fc17b43c
---

Raised 04/2024 at the monthly "AWS/Broadcom Technical Cadence" meeting: Nael (AWS) mentioned AWS is looking into eliminating new-resource additions to the Singapore (ap-southeast-1) region. This directly threatens WSD, whose data/compute/services are overwhelmingly concentrated there — **~97% of WSD's data (~800TB) sits in S3 in ap-southeast-1**. KP separately shared (04/30/2024) that **Singapore itself has hit a power ceiling and will forbid any new data center for the next 3 years** — the underlying reason for AWS's regional growth freeze. One mitigation floated: intentionally extend Reserved Instance (RI) contracts on 2-3 EC2 instances as a buffer against hitting a hard resource limit later.

**Resolved/closed 05/28/2024**: Kin Lum/Yu Han closed this item after clarifying with AWS — no impact to WSD's deployment expected, since **AWS confirmed it will continue investing in the Singapore (SIG) region**. The item was tracked as "gossip"/rumor-driven concern rather than a confirmed policy, and AWS's direct clarification resolved it within about a month of being raised.

**Why:** a region-level capacity constraint on WSD's primary AWS home, not a single-project issue — relevant to any future decision to add compute/storage capacity in ap-southeast-1 (new EC2/RDS/Aurora/MSK instances, S3 growth, etc.).

**How to apply:** treat this as a closed, resolved false alarm as of 05/28/2024 — AWS is continuing to invest in ap-southeast-1, so this is not an active constraint on any 2024+ Singapore-region capacity planning. Keep the history for context only (e.g. if similar rumors resurface). Distinct from [[project-sgdc-to-tkdc-migration]] (that one is about on-premise WSD Windows/CIFS servers physically in a Singapore data center; this one was about the AWS *cloud region* ap-southeast-1) — don't conflate the two Singapore-DC threads.
