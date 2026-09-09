---
name: project-sanjose-seoul-otp-module-id-service
description: "Hardware and software overhaul of the San Jose + Seoul OTP Module ID serial-number service, completed 07/29/2024"
metadata: 
  node_type: memory
  type: project
  modified: 2026-09-07T21:24:11.747Z
  originSessionId: 59aa9cac-0722-4254-85a2-83d236cbea48
---

Per Jay (Seoul) and Mohamed (San Jose)'s request to improve OTP serial-number service performance and reliability. Owners: Chao/Ken. Old SJ hardware was a Dell PowerEdge R710 (10+ years old, 16GB RAM, 1GHz NIC) that could no longer even join the Broadcom domain, running Windows Server 2008 with a .NET Framework 4.5 OTP provider program and an old MySQL DB.

**Scope**: new hardware for both sites (Dell PowerEdge R750: dual Xeon Silver 4309Y 2.8GHz, 384GB RAM, 500GB HDD — spec'd generously since the box runs as an application server), OS upgrade to Windows 11 (Seoul) / new box (SJ), MySQL upgrade, .NET Framework + MySQL driver upgrade, and code fixes: naming typos ("Opt"→"Otp", "ohm"→"omh"), a max-mfg-ID typo (65355→65535), and a fix to the C# HTTP response size limit so `GetModuleIDs()` could return up to the full 16384-ID range without hitting the limit. Module ID range split by site: San Jose 1-16383, Seoul 16384-32767.

**Completed and released 07/29/2024**, with buy-off from both site owners (Jay/Seoul, Mark/San Jose). Setup manual and sample client (v2) published to Box/Google Site.

**Why:** decade-old, domain-unjoinable hardware running a production ID-issuance service used by both SJ and Seoul labs was a real operational risk — this closed that gap.

**How to apply:** reference if OTP Module ID service issues resurface at either site; hardware/software are current as of 07/29/2024, not the old R710/Server-2008 stack.
