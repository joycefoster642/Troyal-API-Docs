# Ownership splits and royalty statements

This guide explains how Troyal divides royalties between the people who own a work, and how to produce a statement showing who is owed what.

## How it works

1. **Splits** record who owns what share of a work. The shares always add up to 100%.
2. **Statements** apply those shares to the revenue a work earned in a period, and show the amount owed to each payee.

If you change a work's splits, statements you generate afterwards use the new shares. Existing statements are not changed.

## Authentication

Send your API key in the `Authorization` header with every request:

```
Authorization: Bearer YOUR_API_KEY
```

## Step 1: Set the ownership splits

Use `PUT /works/{id}/splits` to replace all the splits for a work. This request gives a songwriter 60% and a producer 40%:

```bash
curl -X PUT https://api.troyal.example/v1/works/wrk_8f2a91/splits \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "splits": [
      { "payee_id": "pay_31c7d0", "role": "songwriter", "share_percent": 60 },
      { "payee_id": "pay_77b2e4", "role": "producer", "share_percent": 40 }
    ]
  }'
```

A successful request returns `200`. The `share_percent` values must add up to exactly 100. If they don't, the API returns `400`.

To check the current splits, call `GET /works/{id}/splits`.

## Step 2: Generate a statement

Use `POST /statements` with the work and the period you want to report on:

```bash
curl -X POST https://api.troyal.example/v1/statements \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "work_id": "wrk_8f2a91",
    "period_start": "2026-07-01",
    "period_end": "2026-09-30"
  }'
```

The response is `201`, with the new statement:

```json
{
  "id": "stm_5d09b1",
  "work_id": "wrk_8f2a91",
  "period_start": "2026-07-01",
  "period_end": "2026-09-30",
  "status": "draft",
  "currency": "USD",
  "gross_revenue": 1000.00,
  "lines": [
    { "payee_id": "pay_31c7d0", "share_percent": 60, "amount": 600.00 },
    { "payee_id": "pay_77b2e4", "share_percent": 40, "amount": 400.00 }
  ]
}
```

## How the amounts are worked out

Each payee's amount is the gross revenue multiplied by their share.

| Payee | Share | Calculation | Amount |
| --- | --- | --- | --- |
| pay_31c7d0 | 60% | 1000.00 × 0.60 | 600.00 |
| pay_77b2e4 | 40% | 1000.00 × 0.40 | 400.00 |
| **Total** | 100% | | **1000.00** |

## Find statements later

- `GET /statements` lists statements, newest first. Filter with `work_id`, `payee_id`, `period_start` and `period_end`. Use `limit` to control page size (default 20, maximum 100).
- `GET /statements/{statement_id}` returns one statement with a line for each payee.

## Errors

| Status | Meaning | What to do |
| --- | --- | --- |
| 400 | The request is invalid, for example the shares don't add up to 100, or `period_end` is before `period_start`. | Fix the request and send it again. |
| 401 | The API key is missing or invalid. | Check the `Authorization` header. |
| 404 | The work or statement doesn't exist. | Check the ID. |
| 429 | Too many requests. | Wait for the time in the `Retry-After` header, then retry. |

See the [error codes guide](error-codes.md) for the full list.
