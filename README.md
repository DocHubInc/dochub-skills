# DocHub skills

Public agent skills for building **DocHub Public API integrations** (Cursor, Claude Code,
Codex, and others). The Cursor, Claude, and Codex plugins also ship the official remote MCP
at `https://dochub.com/mcp` (OAuth — no API keys in this repo).

Skills do **not** restate auth flows or endpoint parameters. They point at the public developer
portal docs and OpenAPI:

- Docs: https://dev.dochub.com/docs/
- Catalog: https://dev.dochub.com/llms.txt
- OpenAPI: https://dev.dochub.com/api-reference/openapi.yaml

This repository is the **source of truth** for DocHub’s public agent skills.

## Install

Any agent (Cursor, Claude Code, Codex, and others):

```bash
npx skills add DocHubInc/dochub-skills
```

Use Node.js 20+. Integrator docs: https://dev.dochub.com/docs/ai-agents

## Skills

| Skill | Use when |
|-------|----------|
| `dochub-best-practices` | Choosing which docs/OpenAPI to open for an integration |
| `dochub-auth` | API key or OAuth setup |
| `dochub-documents` | Create, upload, import or copy documents; documents, roles and PDF URLs |
| `dochub-sign-requests` | Send for signature, track status, download the signed PDF; list / bulk / resend / void |
| `dochub-webhooks` | Webhook receivers and endpoint management |
| `dochub-docs` | Looking up docs and OpenAPI via `llms.txt` |

## After install

Start a new agent chat, then ask to scaffold an integration (for example OAuth + send a sign request
from a template). Keep credentials in your app’s own env or secret manager.

If you installed the Cursor, Claude, or Codex plugin, connect the **dochub** MCP and sign in to
DocHub when prompted. Use MCP to act in your DocHub account; use the skills when you are writing an
integration.

## License

MIT. See [LICENSE](LICENSE).
