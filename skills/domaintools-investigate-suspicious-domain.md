---
name: Investigate a suspicious domain
description: Triage a potentially malicious domain using DomainTools risk scoring, WHOIS, hosting history, and profile data.
api: openapi/domaintools-lookups-monitors-openapi.yml
operations: [getDomainProfile, getWhoisLookup, getDomainRiskScore, getRiskScoreEvidence, getHostingHistory]
---

# Investigate a suspicious domain

Authenticate with the `X-Api-Key` header (HMAC signing recommended for production; see `authentication/domaintools-authentication.yml`). Base host: `https://api.domaintools.com`.

## Steps

1. **Get the risk score.** Call `getDomainRiskScore` (`GET /v1/risk/`) with the domain. A 0–100 score classifies threat likelihood.
2. **Pull the evidence.** If the score warrants it, call `getRiskScoreEvidence` (`GET /v1/risk/evidence/`) to see which classifiers (proximity, threat profile) drove the score.
3. **Profile the domain.** Call `getDomainProfile` (`GET /v1/{domain}/`) for a registration/infrastructure overview.
4. **Check registration.** Call `getWhoisLookup` (`GET /v1/{query}/whois/`) for current WHOIS; correlate registrant/registrar.
5. **Review infrastructure changes.** Call `getHostingHistory` (`GET /v1/{domain}/hosting-history/`) to see IP/name-server churn over time.

## Rules

- All operations are GET / read-only (naturally idempotent; no idempotency key needed — see `conventions/domaintools-conventions.yml`).
- Handle the custom error envelope `{ error: { code, message }, resources }` (see `errors/domaintools-problem-types.yml`); a 404 means no record, which is a valid outcome.
- Respect per-product plan limits; back off on 503.
