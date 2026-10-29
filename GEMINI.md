
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
| To check one domain quickly, a list of domains, or names you came up with yourself | `check_domains` (1 to 1,000 a call, no credits) |
| To see every extension one name is free on | `check_all_extensions` (`scope`: `top`, `country`, or `all`) |
| Your opinion of one specific name | `lookup_domain`: availability, brand analysis, and trademarks |
| Names like a domain that is taken or too expensive | `find_alternatives` |
| A registered domain they can buy, within a budget | `search_aftermarket` (`keyword`, `max_price`) |
| Related words to build names from yourself | `keyword_research`, then `check_domains` on what you build |
| To know if a name is safe to use | `check_trademarks` (up to 10 names) |
| To keep or share a shortlist | `save_name_list`, then share `share_url` |
| To add to a list from earlier | `list_name_lists`, then `save_name_list` with `list_id` |
| To know what they can still spend | `get_account` |

Pass the brief as `description`. Use the structured fields instead of stuffing the prompt: `keywords`, `starts_with`, `ends_with`, `min_length`, `max_length`, `style` (`one_word`, `two_words`, `any`), `extensions`, and `exclude` (names already shown or rejected).

## 3. Iterate without repeats

When the user asks for more, call the same tool again with every name already shown in `exclude`. Change one thing at a time (style, length, a keyword) based on what they liked.

## 4. Present results

- Show 5 to 10 picks. Give each a one-line reason.
- Show the domain and its status as returned. Never claim a domain is available unless the tool returned `available: true`. That includes names you came up with yourself: run `check_domains` on them first, and `check_trademarks` on the ones you recommend as a brand.
- For `for_sale` or `premium` domains, show the price when there is one and say it is a purchase, not a registration.
- Include `register_url` or `buy_url` when present, and always include `more_url` (or the list's `share_url`) so they can keep going on NameStation.
- Before the user commits to a name, offer `check_trademarks`. Report the verdict (`clear`, `caution`, `risk`) and say it is a screening, not legal advice.

## 5. Handle limits

- `trial_required`: say NameStation needs a paid plan or its free 3-day trial, and give the `trial_url` from the result. This applies to generating names, checking domains, and screening trademarks.
- `credits.insufficient`, `credits.org_cap`, or `capped`: say so plainly and give the link the tool returned.
- `timeout`: retry once with a smaller `count` or fewer `extensions`.
- `check_domains` takes up to 1,000 domains a call on any mix of extensions. A big batch on rare extensions can run out of time; those come back in `not_checked`: call again with them before you say anything about them. Above 15 domains the text lists only the available and for-sale ones; the structured result has all of them.
- Most extensions can be checked. Domains on one NameStation cannot check come back in `unsupported`; tell the user, and suggest a common extension instead. The full list is at https://www.namestation.com/explore/extensions.
- A domain with status `unchecked` could not be checked this time. Never call it available; try it again in a minute.
- `find_available_domains` (10 credits) and `find_alternatives` (5 credits) charge only when at least one domain is available. `lookup_domain` charges 5 credits for a new brand analysis and nothing when NameStation already has one; pass `include_analysis: false` to skip it. `credits_charged` and `credits_remaining` are in every result.

## 6. Answer a naming request on Reddit, X, or Quora

When the user points you at a public post where someone asks for name ideas (or asks you to find such posts):

1. Read the post. Work out what is being named, who it is for, the tone, and any domain or budget mentioned. If it is not really a request for names, say so and stop.
2. Call `find_available_domains` with that brief. If the poster likes a domain that is taken, call `find_alternatives` on it instead. If they have a budget for a premium domain, add `search_aftermarket`.
3. Keep the 5 to 8 best, run `check_trademarks`, and drop any marked `risk`.
4. Call `save_name_list` with those names, a one-line note each, and the post URL as `source_url`. The share link is then tagged for that platform.
5. Draft a reply under 120 words that reads like a helpful person, not an ad: 3 to 5 names with a short reason each, then the share link. No hashtags, no emojis, no hype.
6. Show the draft to the user. Never post it yourself.

The MCP prompt `answer_naming_request` runs these steps for one post URL.
