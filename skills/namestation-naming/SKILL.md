---
name: namestation-naming
description: Generate brand names, find domains that are actually available, screen names for trademarks, and save shareable shortlists with the connected NameStation account. Use when the user wants a business, product, or brand name, or a domain they can register.
---

# NameStation naming

## 0. Check that the tools are connected

The tools below come from NameStation's remote MCP server at `https://api.namestation.com/mcp`. It signs in with OAuth, so there is nothing to install or host. If tools like `find_available_domains` are not available, tell the user how to connect, and do not guess at domain availability in the meantime:

- Claude Code: `claude mcp add --transport http namestation https://api.namestation.com/mcp`, then run `/mcp` to sign in.
- Codex: `codex mcp add namestation --url https://api.namestation.com/mcp`, then `codex mcp login namestation`.
- Grok Build: `grok mcp add --transport http namestation https://api.namestation.com/mcp`.
- Claude, ChatGPT, Grok, Gemini, Cursor, VS Code, and other apps: setup steps at https://www.namestation.com/integrations.
- Bots and scripts with no person to sign in: the REST API at `https://api.namestation.com/v1` with an API key from https://www.namestation.com/settings/connected-apps.

## 1. Get a usable brief

Ask at most two or three short questions when the brief is thin: what it is, who it is for, and the feel they want. Note any words to keep or avoid, extensions they need (.com only, or .io and .ai too), and length limits. Skip the questions when the user already said enough.

## 2. Pick the tool

| The user wants | Call |
|---|---|
| Names with a domain they can register | `find_available_domains` |
| Name ideas only, no domain check | `generate_name_ideas` |
| To check domains they already have | `check_domains` (up to 100, no credits) |
| To know if a name is safe to use | `check_trademarks` (up to 10 names) |
| To keep or share a shortlist | `save_name_list`, then share `share_url` |
| To add to a list from earlier | `list_name_lists`, then `save_name_list` with `list_id` |
| To know what they can still spend | `get_account` |

Pass the brief as `description`. Use the structured fields instead of stuffing the prompt: `keywords`, `starts_with`, `ends_with`, `min_length`, `max_length`, `style` (`one_word`, `two_words`, `any`), `extensions`, and `exclude` (names already shown or rejected).

## 3. Iterate without repeats

When the user asks for more, call the same tool again with every name already shown in `exclude`. Change one thing at a time (style, length, a keyword) based on what they liked.

## 4. Present results

- Show 5 to 10 picks. Give each a one-line reason.
- Show the domain and its status as returned. Never claim a domain is available unless the tool returned `available: true`.
- For `for_sale` or `premium` domains, show the price when there is one and say it is a purchase, not a registration.
- Include `register_url` or `buy_url` when present, and always include `more_url` (or the list's `share_url`) so they can keep going on NameStation.
- Before the user commits to a name, offer `check_trademarks`. Report the verdict (`clear`, `caution`, `risk`) and say it is a screening, not legal advice.

## 5. Handle limits

- `trial_required`: say generation needs a plan and give the `trial_url` from the result. `check_domains` still works.
- `credits.insufficient`, `credits.org_cap`, or `capped`: say so plainly and give the link the tool returned.
- `timeout`: retry once with a smaller `count` or fewer `extensions`.
- `find_available_domains` charges only when at least one domain is available. `credits_charged` and `credits_remaining` are in every result.
