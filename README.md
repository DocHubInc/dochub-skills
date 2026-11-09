# DocHub skills

Agent skills and the official DocHub MCP for building **DocHub Public API integrations** with
Claude Code, Cursor, Codex, and other coding agents.

This repository is the **source of truth** for DocHub's public agent skills.

## What's in here

There are two separate pieces. You can use either one or both.

| Piece | What it does | Use it when |
|-------|--------------|-------------|
| **Skills** (`skills/`) | Short instructions that tell your agent which DocHub docs and OpenAPI files to read before writing code | You are **writing an integration**: your own app calls the DocHub Public API |
| **MCP** (`https://dochub.com/mcp`) | A remote MCP server that lets the agent act in **your** DocHub account (list documents, fill fields, prepare and send sign requests) | You want the agent to **do something in DocHub now**, not write code |

The skills don't copy auth flows or endpoint parameters into the repo. They send the agent to
the live developer portal, so generated code follows the current API:

- Docs: https://dev.dochub.com/docs/
- Catalog of every docs page and API tag: https://dev.dochub.com/llms.txt
- Full OpenAPI spec: https://dev.dochub.com/api-reference/openapi.yaml

The MCP uses OAuth. This repo contains no API keys, and you never paste one into it.

## Requirements

- A DocHub account
- Node.js 20+ (only for the `npx skills` install)
- For your integration: a DocHub API key or OAuth app. See
  [Authentication](https://dev.dochub.com/docs/authentication) to choose one.

## Install

Choose the section for your agent. **Plugin installs are recommended** for Claude Code, Cursor and
Codex because they add the skills and the MCP in one step.

### Claude Code (plugin, recommended)

Run inside Claude Code:

```text
/plugin marketplace add DocHubInc/dochub-skills
/plugin install dochub@dochub-skills
```

This installs all six skills and the `dochub` MCP server. Restart Claude Code (or start a new
session) so it picks them up.

### Cursor (plugin)

```text
/add-plugin DocHubInc/dochub-skills
```

### Codex (plugin)

```bash
codex plugin marketplace add DocHubInc/dochub-skills
```

```bash
codex plugin add dochub@dochub-skills
```

### Any agent: skills only, with `npx skills`

This uses the open-source [`skills`](https://www.npmjs.com/package/skills) installer. It installs
the **skills only**, not the MCP.

Run it from the root of the project where you'll build the integration:

```bash
npx skills add DocHubInc/dochub-skills
```

The installer asks **which agents to install for**. Make sure your agent is selected (for example
**Claude Code**). Files are saved to a shared `.agents/skills/` folder and then linked into each
selected agent's own folder. If your agent isn't selected, the skills sit on disk but your agent
never sees them.

To skip the picker, name the agent:

```bash
npx skills add DocHubInc/dochub-skills --agent claude-code -y
```

Other useful options:

| Option | Effect |
|--------|--------|
| `--agent claude-code cursor` | Install for several agents |
| `-g`, `--global` | Install for your user account (every project) instead of the current project |
| `--skill dochub-auth dochub-webhooks` | Install only some skills |
| `--copy` | Copy files instead of symlinking (useful if your setup doesn't follow symlinks) |
| `-y` | Skip confirmation prompts |

Where the skills end up for Claude Code:

| Scope | Folder |
|-------|--------|
| Project | `.claude/skills/dochub-*` (a symlink into `.agents/skills/`, or a copy) |
| Global (`-g`) | `~/.claude/skills/dochub-*` |

## Check that it worked

1. Start a **new** agent session. Most agents load skills only at startup.
2. Ask: *"Which DocHub skills do you have?"* The agent should list the `dochub-*` skills.
3. In Claude Code you can also type `/` and look for `dochub-*` entries.

If nothing shows up, see [Troubleshooting](#troubleshooting).

## Connect the MCP (plugin installs)

The plugins register a remote MCP server named `dochub` at `https://dochub.com/mcp`.

1. Open your agent's MCP panel. In Claude Code, run `/mcp`.
2. Select `dochub` and sign in to DocHub in the browser window that opens.
3. Once it shows as connected, ask the agent to act in your account, for example *"List my
   recent DocHub documents."*

To add the MCP **without** the plugin (for example after an `npx skills` install), add this to
your agent's MCP config. For Claude Code, that is `.mcp.json` in the project root:

```json
{
  "mcpServers": {
    "dochub": {
      "type": "http",
      "url": "https://dochub.com/mcp"
    }
  }
}
```

## Skills

You don't call skills by name. The agent loads the right one from what you ask. The prompts below
are examples of requests that trigger each skill.

| Skill | Covers | Example prompt |
|-------|--------|----------------|
| `dochub-best-practices` | Entry point: picks which docs and OpenAPI to read; API key vs OAuth; when to use MCP instead of code | *"I want to add DocHub e-signing to our app. Where do I start?"* |
| `dochub-auth` | API key and OAuth 2.0 setup | *"Add DocHub OAuth login to this Express app."* |
| `dochub-documents` | Listing and managing documents and document roles, fetching PDF URLs | *"Write a helper that lists my DocHub documents and downloads the PDF."* |
| `dochub-sign-requests` | Creating, listing, bulk-sending, resending and voiding sign requests | *"Send a sign request from template X to a list of signers."* |
| `dochub-webhooks` | Webhook receivers, verification, events and endpoint management | *"Add a webhook endpoint that updates our DB when a document is signed."* |
| `dochub-docs` | Looking up any doc page or OpenAPI tag through `llms.txt` | *"What fields does the DocHub sign request response include?"* |

Each skill is a single `SKILL.md` in [`skills/`](skills/). Read them to see exactly what the agent
is told.

## Quick start

After installing, in a new agent session inside your project:

> Set up DocHub OAuth in this app, then add an endpoint that sends a sign request from a template
> and a webhook receiver that records when it's completed.

The agent should:

1. Read the auth docs and pick OAuth (`dochub-auth`).
2. Fetch the Sign Requests OpenAPI tag and write the client call (`dochub-sign-requests`).
3. Read the webhook receiving and events docs and write the receiver (`dochub-webhooks`).

Keep credentials in your app's own environment variables or secret manager. Never commit them.

## Updating and removing

**Claude Code plugin**

```text
/plugin marketplace update dochub-skills
```

Use the `/plugin` menu to disable or uninstall `dochub`.

**`npx skills`**

```bash
npx skills update
```

```bash
npx skills remove dochub-auth dochub-best-practices dochub-docs dochub-documents dochub-sign-requests dochub-webhooks
```

Use `npx skills list` to see what's installed (add `-g` for global installs).

## Troubleshooting

### Skills installed with `npx skills` don't appear in Claude Code

**Symptom:** `.agents/skills/dochub-*` exists and `skills-lock.json` lists the skills, but Claude
Code doesn't see them.

**Cause:** Claude Code reads `.claude/skills/`, not `.agents/skills/`. The installer only links
skills into the folders of the agents you selected, and Claude Code wasn't selected.

**Fix:** pick one:

- Re-run the install for Claude Code:

  ```bash
  npx skills add DocHubInc/dochub-skills --agent claude-code -y
  ```

- Or create the links yourself from the project root:

  ```bash
  mkdir -p .claude/skills && for s in .agents/skills/dochub-*; do ln -s "../../$s" ".claude/skills/$(basename "$s")"; done
  ```

- Or use the Claude Code plugin instead (see [Install](#claude-code-plugin-recommended)).

Then start a new Claude Code session.

### Skills are installed but the agent doesn't use them

- Start a new session. Skills added during a session usually aren't picked up.
- Mention DocHub explicitly in your request (*"using the DocHub Public API, ..."*).
- Check the install scope: a project install only applies when the agent is opened in that project.

### MCP doesn't connect or asks to sign in again

- Run `/mcp` (Claude Code) or open your agent's MCP settings and sign in to `dochub` again.
- Make sure you sign in to the DocHub account you want the agent to act in.

## License

MIT. See [LICENSE](LICENSE).
