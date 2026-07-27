---
name: Run a batch AI-detection job with GPTZero
description: Submit many documents to GPTZero for asynchronous AI-detection, poll the job to completion, and retrieve per-document results.
api: openapi/gptzero-openapi-original.json
operations: [createBatchAnalysis, getBatchStatus, getBatchResults]
generated: '2026-07-19'
method: generated
---

# Run a batch AI-detection job with GPTZero

Use the batch flow when you need to score many documents at once rather than one text passage at a time.

## Authentication
- Send your API key in the `X-API-Key` header. Base URL: `https://api.gptzero.me/v2`.

## Steps
1. **Create the batch** — call `createBatchAnalysis` (`POST /batch`) with a JSON body `{ "documents": [ "...", "..." ] }`. The response returns a batch `id` and a `status` of `pending`.
2. **Poll for status** — call `getBatchStatus` (`GET /batch/{batchId}`) until `status` becomes `completed` (or `failed`). Track progress via `processedDocuments` / `totalDocuments`.
3. **Retrieve results** — once completed, call `getBatchResults` (`GET /batch/{batchId}/results`) to get the per-document analysis results (`classification`, `completely_generated_prob`, and page-level detail for file inputs).

## Conventions & guardrails
- This is a polling workflow — no webhooks/callbacks are published, so poll `getBatchStatus` with a sensible interval rather than tight-looping.
- Professional-plan batches support up to 250 files.
- Rate limit: 30,000 requests/hour across all plans (see `rate-limits/gptzero-rate-limits.yml`).
- Handle `status: failed` explicitly; a failed batch will not return results.
