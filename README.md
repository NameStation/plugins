# NameStation plugins

Official [NameStation](https://www.namestation.com) plugin for AI assistants and coding agents.

AI assistants are good at inventing names, but they cannot tell whether a domain is taken, so many of the domains they suggest were registered years ago. This plugin connects your agent to NameStation, which checks every domain live. Your agent can:

- **Generate brand names with domains you can register today.** NameStation's generator writes the names and checks each domain before the agent calls it available.
- **Verify names from any AI tool.** Paste the domains ChatGPT, Gemini, or a brainstorm suggested, up to 100 at a time, and see which are available, taken, or listed for sale, with the price.
- **Screen a shortlist for trademarks** against USPTO and EUIPO records before anyone gets attached to a name.
- **Save picks to a NameStation list** and share one link with a cofounder, client, or team.

## Requirements

- A NameStation account on a paid plan or the free 3-day trial. If your account needs the trial, the sign-in screen offers it and brings you back to finish connecting.
- Generating names uses 10 credits per search, the same as on namestation.com. A search for available domains only charges when at least one comes back available.
- Checking domains, screening trademarks, and saving lists use no credits. Domain checks have a daily allowance per connection.
- When you approve an app, you choose the NameStation organization it bills and can set a monthly credit cap.

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
| Any agent with skills | `npx skills add https://www.namestation.com` (the skill only; add the server separately) |

The plugin adds the remote NameStation MCP server at `https://api.namestation.com/mcp`, so there is nothing to run locally. The first time a tool runs, you sign in to NameStation and approve the connection.

Setup guides for ChatGPT, Claude, Gemini, Grok, Slack, Windsurf, Antigravity, and the REST API are at [namestation.com/integrations](https://www.namestation.com/integrations).

## What to ask

- "Find 10 one-word .com names for a cozy coffee shop in Portland."
- "ChatGPT suggested brewnook.com, kindlecup.com, and emberbean.com. Which can I actually register?"
- "More like Brightnest, but shorter. Skip anything you already showed me."
- "Screen Lumora, Brightnest, and Kindling for trademark conflicts."
- "Save my three favorites to a list I can send to my cofounder."

The naming skill guides the agent through the session: it gathers a short brief, picks the right tool, skips names it already showed, and only calls a domain available when NameStation returned it as available.

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

## Network and credentials

- The only network endpoint is the NameStation MCP server at `https://api.namestation.com/mcp`. Sign-in uses OAuth 2.1 with PKCE through `https://www.namestation.com/oauth/authorize`.
- The plugin asks for no API keys, reads no local files or environment variables, and has no hooks, scripts, or install steps. It is JSON configuration and one Markdown skill.
- Revoke an app or change its credit cap any time in [Connected apps](https://www.namestation.com/settings/connected-apps).

## Support

Questions or problems: [namestation.com/contact](https://www.namestation.com/contact).
