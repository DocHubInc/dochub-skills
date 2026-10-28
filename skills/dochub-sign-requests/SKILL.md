---
name: dochub-sign-requests
description: >-
  Use when building or debugging sign-request flows. Read the portal Send Sign
  Request guide first, then the OpenAPI; do not restate request parameters from
  memory.
---

# Sign requests

## Guide (read first)

- https://dev.dochub.com/docs/guides/send-sign-request.md — the whole flow: find the documentId, assign its fields to signers, send (signing order, prefilled values), wait for signatures by webhook or polling, download the signed PDF
- Document not in DocHub yet? Create it first: https://dev.dochub.com/docs/guides/create-a-document.md

Build create / list / bulk / resend / void flows from the **published OpenAPI** on the developer portal. Do not copy parameter lists into the user’s code from memory — fetch the YAML and follow it.

## OpenAPI

Fetch the tag YAML for the step you are implementing:

- https://dev.dochub.com/api-reference/tag/sign-requests.yaml — send, status, resend, void, bulk
- https://dev.dochub.com/api-reference/tag/document-roles.yaml — list and create roles
- https://dev.dochub.com/api-reference/tag/formfields.yaml — list fields, assign them to roles, fill sender fields
- https://dev.dochub.com/api-reference/tag/documents.yaml — find the document, download the signed PDF
- Catalog of every tag: https://dev.dochub.com/llms.txt

## Auth

Sign-request calls need the right scheme: see `dochub-auth` (overview: https://dev.dochub.com/docs/authentication.md).

## Agent behavior

1. Read the Send Sign Request guide, then fetch the tag YAML for each step above for exact parameters before writing client code.
2. Prefer current operations described there; treat any marked deprecated as fallback only.
3. Recommend webhooks for reliable status: see `dochub-webhooks`.
4. Before you finish, offer to test the flow with one real send, and send only if the user agrees: DocHub has no test mode, so a send emails real people from their account. If they agree:
   - Send a template, or a copy of the document (`POST /api/v2/documents/{id}/copy`), never their real regular document: a regular document can be sent only once, so a test send would make the real send fail with `403`.
   - Send only to an email address the user gives you for testing, not to a real customer.
   - The test passes when the sign request is `IN_PROGRESS` right after the send; signers may show `WAITING` for a few seconds. Do not wait for `FINALIZED`: that needs a person to sign.
   - Then void it with `POST /api/v2/sign-requests/{id}/void`, unless the user wants to finish signing.
