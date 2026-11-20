---
name: dochub-documents
description: >-
  Use when working with DocHub documents or templates in code: uploading a
  file, importing one from a URL, copying a document or template, listing
  documents, managing document roles, or downloading a PDF, including the
  signed PDF. Read the portal guides and OpenAPI.
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

- https://dev.dochub.com/docs/authentication.md — API key vs OAuth
- Create API keys at https://dev.dochub.com/settings/api-keys and OAuth apps at https://dev.dochub.com/settings/oauth-applications

## Agent behavior

1. To create a document, follow the Create a document guide. Fetch the tag YAML above before implementing list/get/roles/PDF helpers.
2. To send a document for signature, read https://dev.dochub.com/docs/guides/send-sign-request.md and https://dev.dochub.com/api-reference/tag/sign-requests.yaml.
3. To download a signed PDF, follow Step 6 of https://dev.dochub.com/docs/guides/send-sign-request.md and take the response format from `documents.yaml`.

## Rules for every DocHub API call

- Prefer flat JSON: send `Content-Type: application/json` and exactly `Accept: application/json` wherever the operation accepts it. A `*/*` anywhere in `Accept`, which fetch, axios, curl and Python `requests` send by default, returns JSON:API instead of flat JSON. Some endpoints are exceptions (bulk endpoints, legacy sign-request endpoints, endpoints that accept only one format): check the request content types the operation lists in its tag YAML and https://dev.dochub.com/docs/basics/request-formats.md#exceptions.
- OAuth endpoints follow the OAuth 2.0 standard instead: token requests (`POST /api/v2/oauth/token`) use `Content-Type: application/x-www-form-urlencoded`. See https://dev.dochub.com/docs/authentication/oauth.md.
- Use only the public DocHub API documented at https://dev.dochub.com. Take paths, parameters and response shapes from the linked docs and tag YAML; never guess them.
- Keep credentials in the user’s env or secret manager. Redact them in replies and never commit them.
- DocHub has no test mode: sending a sign request emails real people. Send one only after the user agrees, and for a test send use a template or a copy of the document (`POST /api/v2/documents/{id}/copy`), send only to an email address the user gives you for testing, then void it (`POST /api/v2/sign-requests/{id}/void`).
