---
name: Detect AI-generated text with GPTZero
description: Score a text passage for AI authorship and interpret the document-, paragraph-, and sentence-level probabilities returned by GPTZero.
api: openapi/gptzero-openapi-original.json
operations: [analyzeText, getReport]
generated: '2026-07-19'
method: generated
---

# Detect AI-generated text with GPTZero

Use the GPTZero v2 API to classify whether a text passage was written by a human, AI, or is mixed.

## Authentication
- Send your API key in the `X-API-Key` header (generate it at https://app.gptzero.me/app/api). API access requires the Professional plan.
- Base URL: `https://api.gptzero.me/v2`

## Steps
1. **Analyze the text** — call `analyzeText` (`POST /predict/text`) with a JSON body containing `document` (the text) and optionally `multilingual: true`.
2. **Read the classification** — the response `classification` is one of `human`, `ai`, or `mixed`. `completely_generated_prob` and `average_generated_prob` give document-level probabilities; `overall_burstiness` and `perplexity` are supporting signals.
3. **Drill into sentences** — iterate the `sentences[]` array for per-sentence `generated_prob` to highlight which spans read as AI-generated.
4. **(Optional) Pull an aggregate report** — call `getReport` (`GET /reports/{reportId}`) to retrieve an aggregate report across previously analyzed documents.

## Conventions & guardrails
- Requests are rate limited to 30,000 per hour across all plans (see `rate-limits/gptzero-rate-limits.yml`); back off on `429`-class responses.
- There is no idempotency key — scoring calls are safe to retry but each call is billed against your monthly word allowance.
- Treat probabilities as signals, not verdicts; do not present a single number as definitive proof of authorship.
