---
name: Pivot through passive DNS history
description: Use Farsight DNSDB passive DNS to move from a domain to its historical IPs, name servers, and co-hosted infrastructure.
api: openapi/domaintools-dnsdb-openapi.yml
operations: [lookupRrset, lookupRdata, flexSearch, summarizeRrset]
---

# Pivot through passive DNS history

Authenticate with the `X-API-KEY` header. Base host: `https://api.dnsdb.info/dnsdb/v2`.

## Steps

1. **Forward lookup.** Call `lookupRrset` (`GET /lookup/rrset/{type}/{value}`) with `type=name` to see historical DNS records for a domain (A/AAAA/NS/MX).
2. **Inverse lookup.** Call `lookupRdata` (`GET /lookup/rdata/{type}/{value}`) with `type=ip` to find every name that resolved to a given IP — the core infrastructure pivot.
3. **Pattern search.** Call `flexSearch` (`GET /{method}/{key}/{value}`) for regex/glob discovery across rrnames or rdata.
4. **Summarize scale.** Call `summarizeRrset` (`GET /summarize/rrset/{type}/{value}`) to get counts/first-last-seen without pulling every record.

## Rules

- All read-only GETs; results are time-fenced — use `time_first_after`/`time_last_before` params to scope windows.
- DNSDB returns 429 on rate-limit; check the `getRateLimit` (`GET /rate_limit`) endpoint and back off.
- Records follow the Passive DNS Common Output Format (`pdns_cof`).
