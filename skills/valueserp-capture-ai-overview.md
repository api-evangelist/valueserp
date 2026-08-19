---
generated: '2026-08-13'
method: generated
name: Capture Google AI Overviews for a query
description: >-
  Retrieve Google's AI Overview content — both the main SERP block and the
  overviews nested inside People Also Ask — and account for the extra credits
  each one costs.
api: openapi/valueserp-search-api-openapi.yml
operations: [googleSearch]
source: >-
  operationId googleSearch and the include_ai_overview /
  include_ai_overview_paa parameters verified verbatim in
  openapi/valueserp-search-api-openapi.yml. Credit behaviour and the
  related_questions[].ai_overview shape cited from
  https://docs.trajectdata.com/valueserp/product-updates (October 2025).
---

# Capture Google AI Overviews for a query

Pull Google's generative answer content out of a SERP, from both places it appears.

## Auth
- `api_key` query parameter. See `authentication/valueserp-authentication.yml`.

## Steps
1. **Search with overviews on** — `googleSearch` (`GET /search`) with `api_key`, `q`, and `include_ai_overview=true`.
2. **Also capture the PAA overviews** — set `include_ai_overview_paa=true`. AI Overview content then appears under `related_questions[].ai_overview` as well as in the main block. No additional HTTP request is made.
3. **Localize** — AI Overviews vary sharply by market. Set `location` (which infers `google_domain`, `gl`, `hl`) and record what you used; since June 2025 overviews are captured from all Google domains worldwide.
4. **Read both locations** — an overview can be present in the main SERP block, inside `related_questions[]`, in both, or in neither. Treat absence as normal, not as an error.
5. **Record provenance** — keep `search_metadata.id`, `search_metadata.created_at` and `search_metadata.google_url`. AI Overviews are volatile; a capture without a timestamp and source URL is not comparable to the next one.

## Billing
- Each AI Overview returned adds **1 credit** to the query cost, on top of the search itself. A query returning a main overview plus several PAA overviews costs correspondingly more.
- Check `request_info.credits_used` on the response rather than predicting the cost.

## Errors
- Standard status codes apply; see `errors/valueserp-problem-types.yml`. Non-200 responses are never billed.
- `503` means a parsing incident for this request type — AI Overview parsing is the most volatile surface here, so pass `skip_on_incident=true` in automated collection rather than storing partial output.

## Notes
- The API is unversioned: overview parsing changes land for all callers at once and are announced only on the Product Updates page. Be a tolerant reader — treat every field as optional. See `lifecycle/valueserp-lifecycle.yml`.
- Use Google Web Selective Parsing to return only the fields you need if payload size matters: <https://docs.trajectdata.com/valueserp/google-web-selective-parsing>.
