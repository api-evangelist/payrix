---
name: payrix-subscribe-webhooks
description: Register a webhook (alert) for platform events.
api: openapi/payrix-partner-openapi.yml
operations:
- postAlerts
- postAlertTriggers
- postAlertActions
- getAlerts
generated: '2026-09-20'
method: generated
source: openapi/payrix-partner-openapi.yml ; conventions/payrix-conventions.yml
---

# payrix-subscribe-webhooks

Register a webhook (alert) for platform events.

## Steps

1. Create the alert container with `postAlerts` (scope it with forlogin, team or division — only one).
2. Attach one `postAlertTriggers` per event: choose `event` from the alertTriggerEvent enum (e.g. `create`, `chargeback.opened`, `payout`) and the `resource`.
3. Attach delivery with `postAlertActions`: `type` web, your HTTPS endpoint in `value`, and an optional header name/value you verify on receipt (no payload signature is documented).
4. List with `getAlerts` to verify.

## Rules

- Errors: HTTP 400 with `errors[]`; 401 invalid key; 429 rate limited (see rate-limits/payrix-rate-limits.yml).
- Idempotency: `REQUEST-TOKEN` header on every write (conventions/payrix-conventions.yml).
- Pagination: `page[number]`, `page[limit]` (max 100); check `response.details.page.hasMore`.
