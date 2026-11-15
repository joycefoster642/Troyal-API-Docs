# Webhooks

Webhooks let Troyal notify your system when something happens, so you don't have to keep checking the API for changes.

> Troyal is a fictional product. This guide is part of a technical writing portfolio project.

## How webhooks work

1. You give Troyal a URL on your server, called an endpoint.
2. When an event occurs, Troyal sends a `POST` request to that URL with details of the event.
3. Your server responds with a `200` status to confirm it received the event.

## Set up a webhook

1. Log in to the Troyal dashboard.
2. Go to **Settings > Webhooks**.
3. Select **Add endpoint** and enter your URL. It must use `https`.
4. Choose the events you want to receive.
5. Copy your **signing secret**. You'll use it to check that requests really come from Troyal. Keep it private.

## Events

| Event | When it is sent |
|-------|-----------------|
| `work.created` | A new work is created |
| `work.updated` | A work is updated |
| `work.deleted` | A work is deleted |
| `split.updated` | The ownership splits of a work change |
| `statement.generated` | A royalty statement is generated for a payee |

## Event payload

Every event has the same structure:

```json
{
  "id": "evt_5001",
  "type": "statement.generated",
  "created_at": "2026-11-16T09:30:00Z",
  "data": {
    "statement_id": "stm_2001",
    "payee_id": "pay_3001",
    "period": "2026-Q3"
  }
}
```

| Field | Description |
|-------|-------------|
| `id` | A unique ID for the event. Use it to avoid processing the same event twice. |
| `type` | The event name, from the table above. |
| `created_at` | When the event happened, in UTC. |
| `data` | Details of the object the event is about. The fields depend on the event type. |

## Verify the signature

Each request includes an `X-Troyal-Signature` header. It is an HMAC-SHA256 signature of the request body, made with your signing secret. Check it before you trust the request.

```javascript
const crypto = require("crypto");

function isValidSignature(rawBody, signature, secret) {
  const expected = crypto
    .createHmac("sha256", secret)
    .update(rawBody)
    .digest("hex");

  if (signature.length !== expected.length) return false;

  return crypto.timingSafeEqual(
    Buffer.from(expected),
    Buffer.from(signature)
  );
}
```

If the signature doesn't match, reject the request and return `401`.

## Respond and retry

- Respond with a `2xx` status within 10 seconds to confirm receipt.
- If Troyal doesn't receive a `2xx`, it retries up to 5 times over 24 hours, with longer waits between attempts.
- Do slow work, such as updating a database, after you respond, so your server doesn't time out.

## Best practices

- **Handle duplicates.** A retry can deliver the same event twice. Store each event `id` and skip any you have already processed.
- **Verify every request.** Never act on an event without checking its signature.
- **Use HTTPS only.** Troyal won't send events to a plain `http` URL.

## Related

- [Error codes](error-codes.md)
- [API design](Troyal-api-design.md)
