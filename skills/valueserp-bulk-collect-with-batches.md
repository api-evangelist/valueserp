---
generated: '2026-08-13'
method: generated
name: Collect SERPs in bulk with Batches and a webhook
description: >-
  Move a high-volume collection off the per-minute rate limit by using the
  asynchronous Batches API, and receive completion via the batch webhook or an
  object-storage Destination.
api: openapi/valueserp-search-api-openapi.yml
operations: [googleSearch]
source: >-
  The real-time comparison is grounded in operationId googleSearch, verified in
  openapi/valueserp-search-api-openapi.yml. The Batches, Destinations and
  webhook steps are cited from the published documentation
  (https://docs.trajectdata.com/valueserp/batches-api/overview,
  /batches-api/limits, /batches-api/batches/webhook,
  /destinations-api/overview) and from webhooks/valueserp-webhooks.yml — this
  repository holds NO OpenAPI for the Batches API, so those steps name
  documented operations rather than verified operationIds.
---

# Collect SERPs in bulk with Batches and a webhook

Run thousands of searches without fighting the per-minute rate limit.

## When to use this instead of real-time
- Real-time `googleSearch` is synchronous and capped by the plan's per-minute ceiling (250/min on entry plans, up to 5,000/min at the top tier — `rate-limits/valueserp-rate-limits.yml`).
- Batches run asynchronously and are **not** subject to that per-minute limit. A full 15,000-search batch returns in roughly 2-3 minutes.

## Auth
- Same `api_key` query parameter as the real-time API. See `authentication/valueserp-authentication.yml`.

## Steps
1. **Create the batch** — `POST` to the Batches API with a name, the search defaults, and `notification_webhook` set to your HTTPS receiver. If you omit it, ValueSERP falls back to the webhook URL on your account profile.
2. **Add searches** — up to **1,000 per API request**, up to **15,000 per batch**. If you set `include_html=true`, the ceiling drops to **100 searches per batch**. If you set `max_page=N` on the searches, the batch's capacity is divided by N (`max_page=10` means 1,500 searches, not 15,000).
3. **Start it** — starting is a separate call from creating. Concurrency is **5** batches of up to 250 searches, **1** above that; anything else queues without limit.
4. **Receive completion** — ValueSERP POSTs a `batch_resultset_completed` payload to your webhook with `batch.id`, `result_set.id`, `searches_completed`, `searches_failed` and download links.
5. **Download or auto-deliver** — pull the `download_links.json` / `.csv` URLs (which appear only if `notification_as_json` / `notification_as_jsonlines` / `notification_as_csv` are set), or attach a Destination so result sets are pushed straight to S3, Google Cloud Storage, Azure Blob, Alibaba OSS or any S3-compatible store.

## Webhook receiver rules
- **Respond within 5 seconds.** A slower response is terminated by ValueSERP.
- Failures are retried **up to 5 times with exponential backoff**; after that you get an email (max one per 24 hours). So acknowledge fast and process asynchronously.
- **The callback is unsigned and unauthenticated.** Treat it as a trigger only: dedupe on `result_set.id`, then confirm state with an authenticated Get Result Set call. Put an unguessable token in the webhook URL itself.
- Full contract: `webhooks/valueserp-webhooks.yml`.

## Idempotency warning
- Batch creation is a non-idempotent `POST` and ValueSERP supports **no** `Idempotency-Key` mechanism. A retry after a timeout can create a second batch and spend the credits twice. Read back the batch list before retrying a create. See `conventions/valueserp-conventions.yml`.

## Expiry
- Batches never started are deleted after **2 months**. Result sets are downloadable for **14 days**. Error logs are retained **3 days**. Move anything you need to keep into your own storage.

## Errors
- Same status codes as the real-time API (`errors/valueserp-problem-types.yml`). `searches_failed` in the webhook payload counts per-search failures inside a completed batch — a batch can complete with failures, so always read it.
