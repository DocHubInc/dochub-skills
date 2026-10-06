---
name: dochub-docs
description: >-
  Use when looking up DocHub Public API documentation or OpenAPI. Fetch llms.txt, then
  the linked markdown docs and per-tag OpenAPI YAML — do not invent paths or
  fetch Scalar HTML for operation detail.
---

# Docs lookup

Fetch machine-readable sources. Do not guess.

## Start here

https://dev.dochub.com/llms.txt

That index lists every docs page and every API tag. Fetch the files that match the task.

For a whole workflow (creating a document, sending a sign request), read the matching page in the **Guides** section first: https://dev.dochub.com/docs/guides.md. Then use the tag YAML for exact parameters.

## URL convention

Append `.md` to any docs page URL, and `.yaml` to any API reference URL:

| Page | Fetch |
|------|--------|
| https://dev.dochub.com/docs/webhooks/events | https://dev.dochub.com/docs/webhooks/events.md |
| https://dev.dochub.com/api-reference/tag/entities | https://dev.dochub.com/api-reference/tag/entities.yaml |
| https://dev.dochub.com/api-reference | https://dev.dochub.com/api-reference/openapi.yaml |

Each tag YAML is a standalone OpenAPI document (that tag’s paths plus the schemas they reference). Prefer a tag slice over the complete spec. Fetch the complete spec only when the work spans many tags or you cannot tell which tag applies.

## Do not

- Fetch Scalar HTML for operation detail — every endpoint URL returns the same prerendered snapshot
- Invent endpoints or parameters when a slice is available
- Use internal API schemas unless the user explicitly asks
