---
name: service-token-credentials
description: Use when obtaining an access token for a service and choosing credentials, authentication method, or token-request parameters.
---

# Service Token Credentials

Whenever a task needs an access token, first inspect `~/.codex/.env` for credentials matching the service and environment. Credentials may use a service-specific pattern such as `AUTHZ_PRO_CLIENT_ID`, `AUTHZ_PRO_CLIENT_SECRET`, `AUTHZ_PRO_TOKEN_URL`, `AUTHZ_PRO_RESOURCE`, and `AUTHZ_PRO_SCOPE`, or shared credentials such as `ALL_PRO_CLIENT_ID`.

If both matching scopes exist and the user or task has not specified which to use, ask whether to use `ALL_ENV_*` or `SERVICE_ENV_*`. Use only the selected scope; do not combine them. If only one matching scope exists, use it.

Determine the client authentication method from `SERVICE_AUTH_METHOD`, which applies across that service's environments. If absent, use `ALL_AUTH_METHOD`. If both exist, prefer `SERVICE_AUTH_METHOD`.

Supported methods:

- `POST`: Send `client_id` and `client_secret` in the `application/x-www-form-urlencoded` request body.
- `BASIC`: Send them as HTTP Basic credentials and omit them from the body.

If neither method key exists, ask which method to use. Keep grant parameters and any configured resource or scope in the request location expected by the token endpoint, regardless of client authentication method.

Treat `TOKEN_URL` as the token endpoint. Map each other variable suffix to the corresponding token-request body field or query parameter expected by that endpoint. Do not assume every example variable exists, and do not send a body field or query parameter when its matching variable is absent.

Never print or log secret values from `.env`.
