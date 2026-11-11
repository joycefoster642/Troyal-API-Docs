# Error Codes

The Troyal API uses standard HTTP status codes. When a request fails, the response includes a JSON body that says what went wrong.

> Troyal is a fictional product. This guide is part of a technical writing portfolio project.

## Error response format

```json
{
  "error": {
    "code": "invalid_api_key",
    "message": "The API key provided is invalid."
  }
}
```

| Field | Description |
|-------|-------------|
| `code` | A short, fixed name for the error. Use it in your code to handle errors. |
| `message` | A readable explanation. It may change, so don't depend on its exact wording. |

## Error codes

| Status | Code | What it means | How to fix it |
|--------|------|---------------|---------------|
| 400 | `invalid_request` | The request is missing a required field or contains an invalid value. | Check the request body. For example, `POST /works` requires a `title` and a `type`. |
| 401 | `invalid_api_key` | The API key is missing or invalid. | Check that your key is copied correctly and sent in the `Authorization` header. |
| 404 | `not_found` | No resource exists with the ID you sent. | Check the ID and the URL for typos. |
| 429 | `rate_limit_exceeded` | You sent too many requests in a short time. | Wait a moment, then try again. |

## Example: a 401 error

This request has no API key:

```bash
curl https://api.troyal.example/v1/works
```

The API responds with `401 Unauthorized`:

```json
{
  "error": {
    "code": "invalid_api_key",
    "message": "The API key provided is invalid."
  }
}
```

To fix it, add your key to the `Authorization` header. See the [Getting Started guide](getting-started.md) for the full request.

## Related

- [API design](Troyal-api-design.md)
- [Getting Started guide](getting-started.md)
