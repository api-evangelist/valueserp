---
generated: '2026-08-13'
method: generated
name: Rank-track a keyword in a specific market
description: >-
  Run a localized Google organic search and read a domain's position out of the
  SERP, with the localization and pagination rules that make the reading
  reproducible.
api: openapi/valueserp-search-api-openapi.yml
operations: [googleSearch]
source: >-
  operationId googleSearch verified verbatim in
  openapi/valueserp-search-api-openapi.yml. Localization, pagination and billing
  rules cited from conventions/valueserp-conventions.yml and
  https://docs.trajectdata.com/valueserp/search-api/pagination.
---

# Rank-track a keyword in a specific market

Read where a domain ranks for a query, in one named location, reproducibly.

## Auth
- `api_key` as a **query-string** parameter. There is no header alternative. See `authentication/valueserp-authentication.yml`.
- The key appears in the URL, so it lands in proxy and access logs — do not paste request URLs into shared logs or tickets.
- `api_key=demo` works for a first call against production (`sandbox/valueserp-sandbox.yml`).

## Steps
1. **Search** — `googleSearch` (`GET /search` on `https://api.valueserp.com`) with `api_key` and `q`.
2. **Pin the market** — set `location` to a canonical location string (e.g. `London,England,United Kingdom`). ValueSERP then infers `google_domain`, `gl` and `hl` to match. Pass those three explicitly if you need to hold one constant while varying another; explicit values override the inference.
3. **Read the results** — walk `organic_results[]` and match on the target domain. `position` is the rank on that page.
4. **Go deeper if needed** — `page=2` for a single later page, or `max_page=N` to concatenate pages 1..N into one response. With `max_page` set, each result also carries `page` and `position_overall`, so use `position_overall` as the true rank across pages.
5. **Confirm the reading** — `search_metadata.google_url` is the exact Google URL that was scraped, and `search_metadata.id` identifies the request. Store both alongside the rank; a rank without them is not auditable.

## Limits
- `max_page` is capped at **5** for real-time searches (100 for searches added to a Batch).
- Each page successfully retrieved costs **1 credit**. `max_page=5` that yields only 3 pages is billed 3.

## Errors
- `400` — an invalid parameter or an unsupported combination for this search type. Read `request_info.message`.
- `401` — bad `api_key`. `402` — out of credits; enable Overage or top up.
- `429` — over the plan's per-minute ceiling (250/min on entry plans). Back off per `Retry-After`.
- `503` — a live parsing incident for this request type. Retry after `retry_after` seconds.
- Full catalog: `errors/valueserp-problem-types.yml`. **No non-200 response is billed.**

## Notes
- Set `skip_on_incident=true` for scheduled tracking so a parsing incident fails fast (503, unbilled) instead of returning degraded data you would silently record as a rank change.
- Read `request_info.credits_remaining` on every response — it is the only quota signal; there are no `RateLimit-*` headers.
- For more than a few hundred keywords, use the Batches API instead of looping this call. See `conventions/valueserp-conventions.yml`.
