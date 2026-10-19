---
name: project-wsd-kafka-api-review-2026-10
description: Review of WSD.Kafka HTTP API doc (msk_apiV2 produce + dsm_apiV2 DSM reads): all-500 errors, traceback leak, static atoken, ignored begin/end range, no freshness signal, duplicate-on-retry; tiered plan, D1-D10, 13 owner questions
metadata:
  type: project
---

Reviewed 2026-10-13 from `cc.brcm/WSD.Artifact.Rig.SW.InfraAPIImprovement.Research/family/gen-qs/API.md` (folder is named gen-qs but the content is WSD.Kafka). Doc only: no endpoint called, no Lambda code (`KafkaLambda/...`), `metrics.json`, ACLs or client libs seen. Deliverable: `Kafka_API_review_and_proposal.md` + `.pdf` (owner-facing, findings A-F, tiers 0-4, Q1-Q13, D1-D10; built 2026-10-13, **not sent**). Another family after [[project-diy-api-doc-review-2026-10]] and siblings; umbrella is [[project-wsd-api-unification-2026-10]]. Proposal only, nothing decided or scheduled. Not yet folded into `unification.md`/`.pdf` (still "eight families").

**What the API is:** two API Gateway Lambdas, ap-southeast-1 stage `v2`, single `POST` with `func` switch. `msk_apiV2` = `produce` (sync, one message). `dsm_apiV2` = `dsm_consume_last_res`, `dsm200_get_active_items`, `dsm200_get_item_data`, reading `DSM.RESULT`/`DSM.MT` with a per-request Kafka consumer (5 s timeout). Auth = RS256 JWT (aud `...msk` / `...app`, public key packaged in Lambda) + `atoken` header selecting a Secrets Manager entry with that client's Kafka bootstrap/user/pwd. `dsm_processor` (not HTTP, trigger not in repo) writes the snapshot. Owner per [[project-okta-sso-jwt-architecture]]: Tingyu for MSK API (JWT migration done 03/2025); DSM API owner not named.

**Top findings:** everything but bad JWT is 500 (Reporter greps body for `TOPIC_AUTHORIZATION_FAILED`); DSM 500s carry full Python traceback; `atoken` is a static shared key = Kafka identity, no JWT scopes, DSM readers need Kafka creds; `dsm200_get_item_data` uses only `end-begin` counted back from now (documented defect); no `generatedAt` so a dead processor looks fresh; `delivery_timeout_ms` 300 s vs API Gateway 29 s means 504-then-retry can duplicate (no idempotency key); `item1/item2` double-encoded JSON, three casings, three time formats, unstable `testInstanceId`, dead fields (`Tag`, thresholds units).

**Plan:** Tier 0 verify (traceback in prod? timeouts/504 retries? DSM-reader ACLs and atoken age? processor monitoring? who relies on relative window?) -> Tier 1 docs -> Tier 2 additive (requestId+code, generatedAt, array-returning funcs with isDown, stable key, `messageId` header, `produce_batch`, honour absolute range with cap+truncated) -> Tier 3 (real codes+problem+json, scopes `kafka.produce`/`dsm.read`, snapshot-store read path so readers drop Kafka creds, JWKS/Secrets Manager key, `/v1` resource routes + custom domain, second region) -> Tier 4 caller inventory. First sprint: Tier 0, Tier 1, 2(a)(b)(f). Fit to unification: Kafka produce is an "ingest" type, proposed as a 5th resource type beyond the four in the umbrella.

**Decisions proposed (D1-D10, none made):** order, error contract additive first, optional `messageId`, fix range with announcement if dependants, freshness/health fields, snapshot read path, scopes batched with AuthHub, `/v1` routes with legacy adapter+Sunset, generated OpenAPI source of truth, ownership.

**Next:** Yu Han reviews, then send PDF to MSK/DSM owners; collect Q1-Q5 first; consider adding row 9 + "ingest" type to `unification.md`.

**Why:** DSM is the daemon/feeder health signal and the only HTTP path to Kafka; silent staleness and leaky errors hurt more here than in data-query APIs.

**How to apply:** treat as a backlog source; confirm tier with Yu Han before opening Jira. Not committed to git; live memory folder sync still needed per `sync.md`.
