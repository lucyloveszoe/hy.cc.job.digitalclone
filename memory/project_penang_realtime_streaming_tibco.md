---
name: project-penang-realtime-streaming-tibco
description: "Penang real-time test-observability streaming (TIBCO Streambase via vendor BCT) — MQTT-broker ingestion to LiveView datamart/MySQL, completed 02/20/2024"
metadata: 
  node_type: memory
  type: project
  modified: 2026-09-08T01:14:50.709Z
  originSessionId: 077ee51e-a0d3-4dac-9391-fb44fc17b43c
---

Chung Sik (PE) requested a real-time test-observability pipeline for Penang: ~300 testers each publishing an output every 3s, needing live analytics/dashboards rather than the team's usual batch/OLAP pipeline (zDB/Snowflake). Chung Sik picked a **TIBCO Streambase** solution over an in-house Snowflake-based alternative — Yu Han's assessment: "mature server-based solution with rich analytics power, but higher cost and much more maintenance/support workload, assuming no WSD-side development required." TIBCO's consultant **Bahwan Cybertek (BCT)** deployed the streaming engine; WSD SW team (Choo-Yau/YK, with Kin Lum/Chao) supplied and configured the AWS infrastructure: 1 Windows + 1 Linux EC2 (both t2.xlarge, 4vCPU/16GB) plus a Linux r4.large MySQL host, all in the same AWS Singapore VPC as Spotfire, with an MQTT relay broker (unsecure 1883 → secured 8883) bridging the Inari/Broadcom domain. Two interfaces/use cases were implemented: ingest from the MQTT broker, transform, and publish into both a **LiveView datamart** and a **MySQL database**. Vendor demo held 01/17/2024; **project completed 02/20/2024**.

**Earlier investigation phase, before the TIBCO decision was made (Sep-Oct 2023)**: the team ran a broader "Investigation Real-time Streaming Solutions" effort starting 09/12/2023 (Choo-Yau/YK), comparing two options rather than starting from a vendor pick — **Option #1: TIBCO Spotfire Real-Time Streaming** (a live Penang demo with 1 parameter, then a 09/12/2023 brainstorming session framed around "test observability") vs. **Option #2: an AWS-based solution** (Yu Han's proposal: Kinesis Data Stream + Kinesis Data Analytics + S3 + QuickSight, MQTT-dependent). By 10/10/2023, Chung Sik was running a technical discussion with Tibco on where/how to deploy the streaming engine, with Yu Han setting two preconditions before a deeper SpotFire RTS dive (precondition detail not captured in the source report read). This confirms the TIBCO/BCT choice recorded below wasn't a first-instinct vendor pick — it beat an in-house AWS/Kinesis alternative that was seriously scoped in parallel.

**Why:** a real, vendor-driven (not in-house) architecture decision for real-time test data — worth knowing this exists as WSD's one "buy" solution for streaming observability, distinct from the team's usual build-it-in-house Snowflake/Kafka/Gen-QS approach tracked in [[project-clotho-nxg-magnus-roadmap]] and [[project-nxg-pipeline-and-raptor]].

**How to apply:** if Yu Han asks about real-time/streaming dashboards for Penang or references TIBCO/Streambase/BCT/LiveView, this is the project. No further updates found in ww08-13 2024 material beyond the 02/20/2024 completion — verify current operational status before assuming it's still running unmaintained BCT-vendor infrastructure.
