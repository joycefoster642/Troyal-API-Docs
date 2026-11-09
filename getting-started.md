# Getting Started with the Troyal API

This guide takes you from zero to your first successful API request. It should take about five minutes.

> Troyal is a fictional product. This guide is part of a technical writing portfolio project.

## Before you begin

You need:

- A Troyal account (sign up at the Troyal dashboard)
- A tool for sending requests, such as cURL or Postman

## Step 1: Get your API key

1. Log in to the Troyal dashboard.
2. Go to **Settings > API keys**.
3. Select **Create key**, give it a name, and copy it.

Keep your key private. Anyone who has it can read and change your data.

## Step 2: Make your first request

The base URL for all requests is:

```
https://api.troyal.example/v1
```

Send a `GET` request to `/works` to list the works in your account. Pass your API key in the `Authorization` header.

```bash
curl https://api.troyal.example/v1/works \
  -H "Authorization: Bearer YOUR_API_KEY"
```

## Step 3: Check the response

A successful request returns `200 OK` and a list of your works:

```json
{
  "data": [
    {
      "id": "wrk_1001",
      "title": "Midnight in Lagos",
      "type": "music",
      "created_at": "2026-11-01T09:30:00Z"
    }
  ]
}
```

If you have no works yet, `data` is an empty list.

## Troubleshooting

| Status | Meaning | What to do |
|--------|---------|------------|
| 401 | Missing or invalid API key | Check that your key is copied correctly and sent in the `Authorization` header. |
| 404 | The path doesn't exist | Check the URL for typos. |
| 429 | Too many requests | Wait a moment, then try again. |

## Next steps

- Read the [API design](Troyal-api-design.md) to see every endpoint.
- Create your first work with `POST /works`.
