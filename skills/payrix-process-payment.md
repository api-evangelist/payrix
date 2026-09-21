---
name: payrix-process-payment
description: Process a card sale or auth/capture on Payrix Pro, idempotently.
api: openapi/payrix-partner-openapi.yml
operations:
- postCustomers
- postTokens
- postTxns
- getTxnsId
generated: '2026-09-20'
method: generated
source: openapi/payrix-partner-openapi.yml ; conventions/payrix-conventions.yml
---

# payrix-process-payment

Process a card sale or auth/capture on Payrix Pro, idempotently.

## Steps

1. Authenticate every call with the `APIKEY` header (private key). Sandbox base `https://test-api.payrix.com`, production `https://api.payrix.com`.
2. Optionally create the payer with `postCustomers` and vault the card with `postTokens` (prefer PayFields/PayFrame so raw PAN never touches your server).
3. Create the payment with `postTxns`: `type: 1` (sale) or `type: 2` (auth) then a later `type: 3` capture with `fortxn`. `total` is in cents. Always send a unique `REQUEST-TOKEN` header so a retry returns the original record instead of double-charging.
4. Confirm with `getTxnsId`; read `response.errors[]` (code, msg, field, errorCode) on HTTP 400.
5. On HTTP 429 (error code 64) wait out the 10 second block before retrying the same endpoint.

## Rules

- Errors: HTTP 400 with `errors[]`; 401 invalid key; 429 rate limited (see rate-limits/payrix-rate-limits.yml).
- Idempotency: `REQUEST-TOKEN` header on every write (conventions/payrix-conventions.yml).
- Pagination: `page[number]`, `page[limit]` (max 100); check `response.details.page.hasMore`.
