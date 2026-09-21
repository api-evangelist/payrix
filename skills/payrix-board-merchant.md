---
name: payrix-board-merchant
description: Board a new merchant under a partner portfolio.
api: openapi/payrix-partner-openapi.yml
operations:
- postEntities
- getEntitiesId
- getMerchants
- getMerchantsId
generated: '2026-09-20'
method: generated
source: openapi/payrix-partner-openapi.yml ; conventions/payrix-conventions.yml
---

# payrix-board-merchant

Board a new merchant under a partner portfolio.

## Steps

1. Partner-level `APIKEY` required.
2. Create the business with `postEntities`, embedding the merchant, members (owners) and bank accounts per the boarding guide.
3. Poll `getEntitiesId` / `getMerchantsId` for the merchant boarding status, or subscribe to the merchant status events with the webhook skill.
4. In sandbox, use the Merchant Boarding Sandbox Simulator values to force specific underwriting outcomes.

## Rules

- Errors: HTTP 400 with `errors[]`; 401 invalid key; 429 rate limited (see rate-limits/payrix-rate-limits.yml).
- Idempotency: `REQUEST-TOKEN` header on every write (conventions/payrix-conventions.yml).
- Pagination: `page[number]`, `page[limit]` (max 100); check `response.details.page.hasMore`.
