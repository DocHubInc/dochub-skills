---
name: dochub-best-practices
description: >-
  Guides DocHub Public API integration decisions: which portal docs and OpenAPI
  to read, when to use API key vs OAuth, sign requests, documents, and webhooks.
  Use when building or reviewing any DocHub Public API integration.
---

# DocHub Public API best practices

Help the user **build their own integration** against the **DocHub Public API**. Prefer writing code in their project over one-off live API probes unless they ask to debug against a sandbox.

If they want to act in a **connected DocHub account** (list documents, fill fields, prepare or send a sign request), use the DocHub MCP tools from `https://dochub.com/mcp` instead of scaffolding an integration. Do not use MCP as a substitute for writing the user’s app.

## Source of truth

Do not invent paths, params, or auth details. Read:

1. Domain skills when the task is specific: `dochub-auth`, `dochub-documents`, `dochub-sign-requests`, `dochub-webhooks`, or `dochub-docs`
2. Guides for whole workflows: https://dev.dochub.com/docs/guides.md (create a document, send a sign request)
3. Portal documentation (`.md` URLs in those skills)
4. Catalog: https://dev.dochub.com/llms.txt — then the matching tag YAML
5. Complete spec only if the work spans many tags: https://dev.dochub.com/api-reference/openapi.yaml

Do not fetch Scalar HTML for operation detail — every endpoint URL returns the same prerendered snapshot.

## Routing (what to open next)

| Building… | Open first |
|-----------|------------|
| Auth choice / tokens / scopes | `dochub-auth` → portal auth docs |
| Create, upload, import or copy a document | `dochub-documents` → https://dev.dochub.com/docs/guides/create-a-document.md |
| List/manage files, roles, PDF URL | `dochub-documents` → Documents / Document Roles YAML |
| Send a document or template for signature | `dochub-sign-requests` → https://dev.dochub.com/docs/guides/send-sign-request.md |
| Resend / void sign requests | `dochub-sign-requests` → Sign Requests YAML |
| Status-driven automation | `dochub-webhooks` → webhook docs + Webhooks YAML |
| Unsure where something lives | `dochub-docs` → `llms.txt` |

## Agent rules

- Prefer the user’s language and their existing env/config patterns (do not invent product-specific env var names).
- Redact secrets in replies. Do not commit credentials.
- After scaffolding, point them at the portal docs/OpenAPI URLs you used.
