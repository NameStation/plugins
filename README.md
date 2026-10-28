# NameStation plugins

Official [NameStation](https://www.namestation.com) plugin for AI assistants and coding agents. It adds the NameStation MCP server and a naming skill, so your agent can:

- generate brand names and only call a domain available when you can register it today
- check domains you already have, up to 100 at a time
- screen a shortlist against USPTO and EUIPO trademarks
- save picks to a NameStation list you can share

The server is remote (`https://api.namestation.com/mcp`) and signs in with OAuth. There is nothing to run locally. Sign in with your NameStation account the first time a tool is used. Generating names spends credits from your NameStation plan. Checking domains and saving lists are free.

## Install

| App | Install |
|---|---|
| Claude Code | `claude plugin marketplace add namestation/plugins`, then `claude plugin install namestation@namestation` |
| Codex | `codex plugin marketplace add namestation/plugins`, then open `/plugins` and install NameStation |
| GitHub Copilot CLI | `copilot plugin install namestation/plugins` |
| VS Code | Run **Chat: Install Plugin From Source** and enter `https://github.com/namestation/plugins` |
| Grok Build | `grok plugin install namestation/plugins --trust` |
| Gemini CLI | `gemini extensions install https://github.com/namestation/plugins` |
| Cursor | [Add to Cursor](https://www.namestation.com/integrations/cursor) |
| Claude.ai and Claude Desktop | Download [namestation-plugin.zip](https://www.namestation.com/downloads/namestation-plugin.zip) and upload it under **Customize > Plugins** |
| Any agent with skills | `npx skills add https://www.namestation.com` |

Setup guides for ChatGPT, Claude, Grok, Gemini, Slack, Windsurf, Antigravity, and the REST API are at [namestation.com/integrations](https://www.namestation.com/integrations).

## What's inside

| File | Used by |
|---|---|
| `plugin.json`, `mcp.json` | Agent Plugins standard: ChatGPT, Codex, GitHub Copilot, Cursor |
| `.claude-plugin/`, `.mcp.json` | Claude Code, Claude.ai, Grok Build |
| `.cursor-plugin/plugin.json` | Cursor Marketplace |
| `.agents/plugins/marketplace.json` | Codex marketplace |
| `gemini-extension.json`, `GEMINI.md` | Gemini CLI |
| `skills/namestation-naming/` | The naming skill, shared by all of the above |
| `server.json` | MCP Registry entry |

## Support

Questions or problems: [namestation.com/contact](https://www.namestation.com/contact). Manage or revoke connected apps in [Settings, Connected apps](https://www.namestation.com/settings/connected-apps).
