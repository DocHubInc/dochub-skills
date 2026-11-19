---
name: dochub-best-practices
description: >-
  Use whenever the user mentions DocHub or wants to build with the DocHub API:
  e-signature or e-sign flows, sending documents or contracts for signature,
  onboarding or HR paperwork, uploading documents or templates, webhooks, API
  keys or OAuth. Routes to the right DocHub guides, docs and OpenAPI, and sets
  the rules for every DocHub API call.
---

# DocHub Public API best practices

Help the user **build their own integration** against the **DocHub Public API**. Prefer writing code in their project over one-off live API probes unless they ask to debug against a sandbox.

If they want to act in a **connected DocHub account** (list documents, fill fields, prepare or send a sign request), use the DocHub MCP tools from `https://dochub.com/mcp` instead of scaffolding an integration. Do not use MCP as a substitute for writing the user’s app.

## Source of truth

Do not invent paths, params, or auth details. Read:

1. Guides for whole workflows: https://dev.dochub.com/docs/guides/create-a-document.md and https://dev.dochub.com/docs/guides/send-sign-request.md
2. The docs and tag YAML in the routing table below
3. Catalog: https://dev.dochub.com/llms.txt — then the matching tag YAML
4. Complete spec only if the work spans many tags: https://dev.dochub.com/api-reference/openapi.yaml

Do not fetch Scalar HTML for operation detail — every endpoint URL returns the same prerendered snapshot.

## Routing (what to open next)

Open the links in the right column directly. The matching skill, if it is loaded, adds more detail.

| Building… | Open first | Skill |
|-----------|------------|-------|
| Auth choice / tokens / scopes | https://dev.dochub.com/docs/authentication.md | `dochub-auth` |
| Create, upload, import or copy a document | https://dev.dochub.com/docs/guides/create-a-document.md | `dochub-documents` |
| List/manage files, roles, PDF URL | https://dev.dochub.com/api-reference/tag/documents.yaml | `dochub-documents` |
| Send a document or template for signature | https://dev.dochub.com/docs/guides/send-sign-request.md | `dochub-sign-requests` |
| Resend / void sign requests | https://dev.dochub.com/api-reference/tag/sign-requests.yaml | `dochub-sign-requests` |
| Download the signed PDF | Step 6 of https://dev.dochub.com/docs/guides/send-sign-request.md | `dochub-sign-requests` |
| Status-driven automation | https://dev.dochub.com/docs/webhooks/receiving.md and https://dev.dochub.com/docs/webhooks/events.md | `dochub-webhooks` |
| Unsure where something lives | https://dev.dochub.com/llms.txt | `dochub-docs` |

## Rules for every DocHub API call

- Send `Content-Type: application/json` and exactly `Accept: application/json`. A `*/*` anywhere in `Accept`, which fetch, axios, curl and Python `requests` send by default, returns JSON:API instead of flat JSON. See https://dev.dochub.com/docs/basics/request-formats.md.
- Use only the public DocHub API documented at https://dev.dochub.com. Take paths, parameters and response shapes from the linked docs and tag YAML; never guess them.
- Keep credentials in the user’s env or secret manager. Redact them in replies and never commit them.
- DocHub has no test mode: sending a sign request emails real people. Send one only after the user agrees, and for a test send use a template or a copy of the document (`POST /api/v2/documents/{id}/copy`), send only to an email address the user gives you for testing, then void it (`POST /api/v2/sign-requests/{id}/void`).

## Also

- Prefer the user’s language and their existing env/config patterns (do not invent product-specific env var names).
- After scaffolding, point them at the portal docs/OpenAPI URLs you used.
