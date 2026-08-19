---
generated: '2026-08-13'
method: generated
name: Monitor a business in the Google local pack
description: >-
  Find a business in Google Maps / local results for a location and capture its
  stable Google entity id so readings can be joined across runs.
api: openapi/valueserp-places-api-openapi.yml
operations: [googlePlaces]
source: >-
  operationId googlePlaces verified verbatim in
  openapi/valueserp-places-api-openapi.yml (GET /search with
  search_type=places). knowledge_graph_id availability on local results cited
  from https://docs.trajectdata.com/valueserp/product-updates (September 2025).
---

# Monitor a business in the Google local pack

Track a business's presence and position in Google's local/places results.

## Auth
- `api_key` query parameter. See `authentication/valueserp-authentication.yml`.

## Steps
1. **Search places** — `googlePlaces` (`GET /search`) with `api_key`, `search_type=places`, `q` (the category or brand term) and `location`.
2. **Pin the geography precisely** — local results are the most location-sensitive surface in ValueSERP. Use a canonical location string from the Locations API (<https://docs.trajectdata.com/valueserp/locations-api/overview>), or `uule` when you need a coordinate-level pin. Two different location strings are two different measurements.
3. **Identify the business by entity, not by name** — read `knowledge_graph_id` (e.g. `/g/11b6w9zg1z`) from the local result. Names and addresses drift; the Google entity id is the only stable join key ValueSERP exposes. Match on it across runs.
4. **Page if the pack is deep** — `page` and `max_page` work here as elsewhere; `max_page` is capped at 5 for real-time requests and each retrieved page costs a credit.
5. **Store the reading** — persist `knowledge_graph_id`, position, `search_metadata.id` and `search_metadata.created_at` together.

## Errors
- `400` — a parameter that is legal for `search_type=search` may be rejected for `places`. Parameter validity is per search type; read `request_info.message`.
- `429` / `503` — back off per `Retry-After` / `retry_after`. See `errors/valueserp-problem-types.yml`.

## Notes
- `knowledge_graph_id` on local results was added in September 2025; older stored readings will not have it, so plan a re-baseline rather than a backfill.
- For deeper detail on one place, the docs expose Map Place Details and Place Details request types; this repository holds no OpenAPI for them, so they are not grounded here. See `mcp/valueserp-mcp.yml` for the full list of surfaces without a spec.
