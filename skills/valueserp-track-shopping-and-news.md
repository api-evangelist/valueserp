---
generated: '2026-08-13'
method: generated
name: Track a product across Google Shopping and News
description: >-
  Pull Google Shopping product listings and Google News coverage for the same
  term, with the deprecation caveat that applies to the legacy product request
  types.
api: openapi/valueserp-shopping-api-openapi.yml
operations: [googleShopping, googleNews, googleProduct]
source: >-
  operationIds googleShopping, googleNews and googleProduct verified verbatim in
  openapi/valueserp-shopping-api-openapi.yml,
  openapi/valueserp-news-api-openapi.yml and
  openapi/valueserp-product-api-openapi.yml. Deprecation status cited from
  lifecycle/valueserp-lifecycle.yml and
  https://docs.trajectdata.com/valueserp/search-api/searches/google/product.
---

# Track a product across Google Shopping and News

Read commercial listings and editorial coverage for the same term in one pass.

## Auth
- `api_key` query parameter. See `authentication/valueserp-authentication.yml`.

## Steps
1. **Shopping listings** — `googleShopping` (`GET /search`) with `api_key`, `search_type=shopping`, `q` and `location`. Narrow with `sort_by`, `shopping_filter`, `shopping_buy_on_google`. Since May 2025 the shopping result structure is the improved one; since September 2025 `shopping_graphs` is also returned on Google Web results when `device=mobile`.
2. **News coverage** — `googleNews` (`GET /search`) with `search_type=news` and the same `q`. Bound the window with `time_period`, or `time_period_min` / `time_period_max` for an explicit range.
3. **Align the market** — use the same `location` on both calls so the two readings describe one market. Explicit `google_domain` / `gl` / `hl` override the inference if you need to hold a market fixed.
4. **Paginate consistently** — `page` / `max_page` behave the same on both; `max_page` is capped at 5 real-time and each retrieved page costs a credit.
5. **Join on time, not on rank** — store `search_metadata.created_at` from both responses. Shopping and news move on different clocks.

## Deprecation warning — read before using product detail
- `googleProduct` (`search_type=product`) is **deprecated**. The documentation page is titled "Google Product deprecating on 10/31", and the legacy Product, Online Sellers and Specifications request types are all replaced by **Google Product Details** (`search_type=product_details`).
- ValueSERP emits **no** `Sunset` or `Deprecation` response header and does not state the year, so nothing in the response will warn you. Do not build new work on `googleProduct`.
- This repository holds no OpenAPI for the replacement, so no operationId can be cited for it. See `lifecycle/valueserp-lifecycle.yml`.

## Errors
- Parameter validity is per search type: a parameter accepted for `shopping` may 400 for `news`. Read `request_info.message`.
- `503` with `retry_after` means a parsing incident for that one request type — shopping and news can be affected independently, so handle each call's status separately. See `errors/valueserp-problem-types.yml`.

## Notes
- Non-200 responses are never billed, so retrying costs nothing but time.
- Watch `request_info.credits_remaining`; two calls per term consumes quota twice as fast as rank tracking alone.
