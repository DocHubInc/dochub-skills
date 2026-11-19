---
name: dochub-auth
description: >-
  Use when connecting an app to DocHub: API keys, OAuth 2.0 apps, access tokens,
  scopes, "sign in with DocHub", or any DocHub auth error. Links where to create
  API keys and OAuth apps, plus the portal auth docs and OpenAPI — do not invent
  flows from memory.
---

# Authentication

Implement auth using the **developer portal documentation** and OpenAPI. Do not restate grant types, headers, or token fields here — read the sources below.

## Where to create credentials

- API keys: https://dev.dochub.com/settings/api-keys
- OAuth applications (client ID and secret): https://dev.dochub.com/settings/oauth-applications

Point the user to the matching page when they need a key or an OAuth app. Never ask them to paste a secret into chat.

## Documentation

- https://dev.dochub.com/docs/authentication.md — choosing API key vs OAuth
- https://dev.dochub.com/docs/authentication/api-key.md
- https://dev.dochub.com/docs/authentication/oauth.md — authorization code flow and the scopes table

## OpenAPI

- https://dev.dochub.com/api-reference/tag/oauth.yaml
- https://dev.dochub.com/api-reference/tag/oauth-applications.yaml
- Catalog of every tag: https://dev.dochub.com/llms.txt

## Agent behavior

1. Read the auth docs that match the product model (API key vs OAuth).
2. Confirm request/response shapes from the tag YAML above.
3. For OAuth, request only the scopes the integration needs (see the scopes table in the OAuth doc).
4. Wire credentials into the user’s app using their existing secret/env conventions.

## Rules for every DocHub API call

- Send `Content-Type: application/json` and exactly `Accept: application/json`. A `*/*` anywhere in `Accept`, which fetch, axios, curl and Python `requests` send by default, returns JSON:API instead of flat JSON. See https://dev.dochub.com/docs/basics/request-formats.md.
- Use only the public DocHub API documented at https://dev.dochub.com. Take paths, parameters and response shapes from the linked docs and tag YAML; never guess them.
- Keep credentials in the user’s env or secret manager. Redact them in replies and never commit them.
- DocHub has no test mode: sending a sign request emails real people. Send one only after the user agrees, and for a test send use a template or a copy of the document (`POST /api/v2/documents/{id}/copy`), send only to an email address the user gives you for testing, then void it (`POST /api/v2/sign-requests/{id}/void`).
