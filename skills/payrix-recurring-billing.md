---
name: payrix-recurring-billing
description: Set up a plan and subscribe a tokenized customer.
api: openapi/payrix-partner-openapi.yml
operations:
- postPlans
- postTokens
- postSubscriptions
- getSubscriptionsId
generated: '2026-09-20'
method: generated
source: openapi/payrix-partner-openapi.yml ; conventions/payrix-conventions.yml
---

# payrix-recurring-billing

Set up a plan and subscribe a tokenized customer.

## Steps

1. Create the billing plan with `postPlans` (amount in cents, schedule and schedule factor).
2. Vault the payment method with `postTokens`.
3. Create the subscription with `postSubscriptions`, then bind the token per the subscriptions guide.
4. Inspect with `getSubscriptionsId`; failed recurring payments surface as transactions and events.

## Rules

- Errors: HTTP 400 with `errors[]`; 401 invalid key; 429 rate limited (see rate-limits/payrix-rate-limits.yml).
- Idempotency: `REQUEST-TOKEN` header on every write (conventions/payrix-conventions.yml).
- Pagination: `page[number]`, `page[limit]` (max 100); check `response.details.page.hasMore`.
