---
name: payrix-reverse-or-refund
description: Void an approved transaction or refund a captured one.
api: openapi/payrix-partner-openapi.yml
operations:
- getTxnsId
- postTxns
generated: '2026-09-20'
method: generated
source: openapi/payrix-partner-openapi.yml ; conventions/payrix-conventions.yml
---

# payrix-reverse-or-refund

Void an approved transaction or refund a captured one.

## Steps

1. Read the transaction with `getTxnsId` and check its status.
2. If it is still Approved (not yet captured in the settlement batch), void it: `postTxns` with `type: 4` and `fortxn` set to the original id.
3. If it is Captured or Settled, refund it: `postTxns` with `type: 5` (card) or `type: 8` (eCheck), `fortxn` = original id, and `total` for a partial refund. The refund is a new transaction debited from the merchant balance.
4. Send a `REQUEST-TOKEN` header on both so the reversal cannot fire twice.

## Rules

- Errors: HTTP 400 with `errors[]`; 401 invalid key; 429 rate limited (see rate-limits/payrix-rate-limits.yml).
- Idempotency: `REQUEST-TOKEN` header on every write (conventions/payrix-conventions.yml).
- Pagination: `page[number]`, `page[limit]` (max 100); check `response.details.page.hasMore`.
