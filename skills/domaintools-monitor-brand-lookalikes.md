---
name: Monitor a brand for lookalike domains
description: Stand up Iris Detect monitoring to catch newly registered lookalike/infringing domains and escalate the risky ones.
api: openapi/domaintools-iris-openapi.yml
operations: [postDetectMonitor, getDetectMonitors, getDetectNewDomains, getDomainsWatched, postDetectEscalations]
---

# Monitor a brand for lookalike domains

Authenticate with the `X-Api-Key` header. Base host: `https://api.domaintools.com`, Iris Detect surface under `/v1/iris-detect/`.

## Steps

1. **Create a monitor.** Call `postDetectMonitor` (`POST /v1/iris-detect/monitors/`) with the brand term(s) to watch.
2. **Confirm it exists.** Call `getDetectMonitors` (`GET /v1/iris-detect/monitors/`) to list configured monitors and IDs.
3. **Fetch new detections.** Poll `getDetectNewDomains` (`GET /v1/iris-detect/domains/new/`) for freshly registered candidate domains; page with the `position_token` cursor.
4. **Review the watchlist.** Call `getDomainsWatched` (`GET /v1/iris-detect/domains/watched/`) for domains already under watch.
5. **Escalate.** For confirmed infringements, call `postDetectEscalations` (`POST /v1/iris-detect/escalations/`) to escalate (e.g. blocklist/takedown).

## Rules

- Escalation and monitor create/update are the only write operations — there is no idempotency-key contract, so guard against duplicate monitor creation client-side (see `conventions/domaintools-conventions.yml`).
- Iris Detect returns 422 on semantically invalid request bodies; validate against the operation schema.
- Page exhaustively with `position_token` before assuming a result set is complete.
