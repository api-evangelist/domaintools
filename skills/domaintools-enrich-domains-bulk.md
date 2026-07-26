---
name: Bulk-enrich a list of domains
description: Enrich a batch of known domains with DomainTools risk, WHOIS, and infrastructure context via Iris Enrich.
api: openapi/domaintools-iris-openapi.yml
operations: [postIrisEnrich, getIrisEnrich]
---

# Bulk-enrich a list of domains

Authenticate with the `X-Api-Key` header. Base host: `https://api.domaintools.com`, endpoint `/v1/iris-enrich/`.

## Steps

1. **Submit the batch.** Call `postIrisEnrich` (`POST /v1/iris-enrich/`) with a comma-separated or list `domains` payload to enrich many domains in one call.
2. **Single/GET alternative.** For one or few domains, `getIrisEnrich` (`GET /v1/iris-enrich/`) accepts the domain(s) as query parameters.
3. **Consume the enrichment.** Each result carries risk score, WHOIS, infrastructure, and screenshot context for the known domain.

## Rules

- Iris Enrich is for domains you already have; use Iris Investigate (`postIrisInvestigate`) when you need to pivot/discover.
- Prefer POST for large batches to keep credentials and payloads out of URLs (see `authentication/domaintools-authentication.yml`).
- Enrichment consumes account query quota; monitor with `getAccountInfo` (`GET /v1/account/`).
