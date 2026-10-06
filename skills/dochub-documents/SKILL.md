---
name: dochub-documents
description: >-
  Use when creating, uploading, importing or copying documents, listing or
  managing documents and document roles, or fetching PDF URLs. Read the portal
  guides and OpenAPI.
---

# Documents

Use the **published OpenAPI** on the developer portal for paths, parameters, and schemas. Do not restate them in chat from memory.

## Guide (read first)

- https://dev.dochub.com/docs/guides/create-a-document.md — upload a file, import one from a URL, or copy an existing document or template

## OpenAPI

- https://dev.dochub.com/api-reference/tag/documents.yaml
- https://dev.dochub.com/api-reference/tag/document-roles.yaml
- Catalog of every tag: https://dev.dochub.com/llms.txt

## Auth

- https://dev.dochub.com/docs/authentication.md

## Agent behavior

1. To create a document, follow the Create a document guide. Fetch the tag YAML above before implementing list/get/roles/PDF helpers.
2. For sending documents for signature, continue with `dochub-sign-requests`.
