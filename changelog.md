# Changelog

All notable changes to the Troyal API are listed here, with the newest changes first.

> Troyal is a fictional product. This changelog is part of a technical writing portfolio project.

## Unreleased

Planned for a future version:


## 1.1.0 - 2026-11-22

### Added

- Ownership splits: `GET /works/{id}/splits` and `PUT /works/{id}/splits`. Shares must add up to 100.
- Royalty statements: `POST /statements`, `GET /statements` and `GET /statements/{statement_id}`.
- New guide: [Ownership splits and royalty statements](splits-and-statements.md), with a worked 60/40 example.
  
## 1.0.0 - 2026-11-11

Initial release.

### Added

- Works endpoints: list, create, retrieve, update and delete
- API key authentication, sent in the `Authorization` header
- Standard error responses with error codes `400`, `401`, `404` and `429`
- OpenAPI specification for the Works endpoints
- Webhooks guide: how to receive events, verify signatures and handle retries. See [Webhooks](webhooks.md).
