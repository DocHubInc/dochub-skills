---
name: dochub-sign-requests
description: >-
  Use when building or debugging sign-request flows. Read the portal OpenAPI and
  API reference; do not restate request parameters from memory.
---

# Sign requests

## Guide (read first)

- https://dev.dochub.com/docs/guides/send-sign-request.md — the whole flow: find the documentId, assign its fields to signers, send (signing order, prefilled values), wait for signatures by webhook or polling, download the signed PDF
- Document not in DocHub yet? Create it first: https://dev.dochub.com/docs/guides/create-a-document.md

Build create / list / bulk / resend / void flows from the **published OpenAPI** on the developer portal. Do not copy parameter lists into the user’s code from memory — fetch the YAML and follow it.

## OpenAPI

- https://dev.dochub.com/api-reference/tag/sign-requests.yaml
- Catalog of every tag: https://dev.dochub.com/llms.txt

## Auth docs (sign-request calls need the right scheme)

- https://dev.dochub.com/docs/authentication.md
- https://dev.dochub.com/docs/authentication/api-key.md
- https://dev.dochub.com/docs/authentication/oauth.md

## Agent behavior

1. Read the Send Sign Request guide, then fetch the Sign Requests tag YAML for exact parameters before writing client code.
2. Prefer current operations described there; treat any marked deprecated as fallback only.
3. Recommend webhooks for reliable status: see `dochub-webhooks`.
4. Before you finish, run the flow once end to end and confirm the sign request status. Send it to a signer email the user controls, such as one they configured or gave you, not to a real customer.
