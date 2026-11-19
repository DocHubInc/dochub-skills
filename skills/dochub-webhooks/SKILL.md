---
name: dochub-webhooks
description: >-
  Use when reacting to DocHub events in code: webhook receivers, automation
  that runs when a document is signed or a sign request is completed, voided or
  expired, verifying webhook deliveries, or managing webhook endpoints. Read
  portal webhook docs and OpenAPI.
---

# Webhooks

Use portal **webhook documentation** for setup, verification, deliveries, and events. Use published OpenAPI for HTTP API shapes. Do not restate headers, payload fields, or event catalogs here.

## Documentation

- https://dev.dochub.com/docs/webhooks.md
- https://dev.dochub.com/docs/webhooks/configuration.md
- https://dev.dochub.com/docs/webhooks/receiving.md
- https://dev.dochub.com/docs/webhooks/deliveries.md
- https://dev.dochub.com/docs/webhooks/events.md

## OpenAPI

- https://dev.dochub.com/api-reference/tag/webhooks.yaml
- Catalog of every tag: https://dev.dochub.com/llms.txt

## Acting on an event

A webhook only tells you something happened. Whatever the receiver does next is a separate API call, so read its docs too before writing it:

| After the event, the receiver… | Read |
|--------------------------------|------|
| Downloads the signed PDF | Step 6 of https://dev.dochub.com/docs/guides/send-sign-request.md, then https://dev.dochub.com/api-reference/tag/documents.yaml for the exact response format |
| Reads sign request or signer status | https://dev.dochub.com/api-reference/tag/sign-requests.yaml |
| Reads or changes the document | https://dev.dochub.com/api-reference/tag/documents.yaml |

Do not guess the shape of these responses from the webhook payload.

## Auth

- https://dev.dochub.com/docs/authentication.md — API key vs OAuth
- Create API keys at https://dev.dochub.com/settings/api-keys and OAuth apps at https://dev.dochub.com/settings/oauth-applications

## Agent behavior

1. Read receiving + events docs before writing a receiver.
2. Confirm manage-endpoint operations from the Webhooks tag YAML when coding against the API.
3. For each follow-up call the receiver makes, read the docs in "Acting on an event".

## Rules for every DocHub API call

- Send `Content-Type: application/json` and exactly `Accept: application/json`. A `*/*` anywhere in `Accept`, which fetch, axios, curl and Python `requests` send by default, returns JSON:API instead of flat JSON. See https://dev.dochub.com/docs/basics/request-formats.md.
- Use only the public DocHub API documented at https://dev.dochub.com. Take paths, parameters and response shapes from the linked docs and tag YAML; never guess them.
- Keep credentials in the user’s env or secret manager. Redact them in replies and never commit them.
- DocHub has no test mode: sending a sign request emails real people. Send one only after the user agrees, and for a test send use a template or a copy of the document (`POST /api/v2/documents/{id}/copy`), send only to an email address the user gives you for testing, then void it (`POST /api/v2/sign-requests/{id}/void`).
