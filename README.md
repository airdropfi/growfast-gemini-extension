# GrowFast for Gemini CLI

Find B2B customers for your product from [Gemini CLI](https://github.com/google-gemini/gemini-cli) with your own [GrowFast](https://growfast.sh) account. The extension connects Gemini to the GrowFast MCP server and adds three slash commands and a context file. It scans a site for ideal customers, drafts first emails to named decision-makers and sets up Reddit listening. It never sends an email or posts to Reddit: you do that from growfast.sh.

## Before you start

- **You need a GrowFast account** (sign up at https://growfast.sh). This extension is a connector, not a standalone tool.
- Your plan's limits apply to every call.
- A browser must be available once, for sign-in.

## Install

```bash
gemini extensions install https://github.com/airdropfi/growfast-gemini-extension
```

Restart Gemini CLI, then sign in:

```text
/mcp auth growfast
```

A browser window opens: sign in to GrowFast and click Allow. Sign-in is OAuth with PKCE and dynamic client registration, discovered from the server, so there is no key to paste. Check it with `/mcp` (the `growfast` server should list its tools) and try `/growfast:reddit`.

Update later with `gemini extensions update growfast`; remove with `gemini extensions uninstall growfast`.

### Antigravity CLI

This repository is also an Antigravity plugin (`.antigravity-plugin/plugin.json`, `mcp_config.json`, `rules/AGENTS.md`):

```bash
agy plugin install https://github.com/airdropfi/growfast-gemini-extension
```

It connects the same server with the same tools disabled. The slash commands are Gemini CLI commands; in Antigravity just describe what you want. We have not run the plugin in Antigravity.

## Commands

| Command | What it does |
|---|---|
| `/growfast:scan <domain>` | Scan a product site: ideal customer profile, a first email and companies scored 1-10. Saves the scan to your account. |
| `/growfast:draft <details>` | Draft first emails for up to 10 companies to the named decision-maker. Nothing is sent or saved. |
| `/growfast:reddit <domain>` | List Reddit monitors and matches with drafted replies, or set up listening (up to 6 keywords; replaces that product's earlier monitor settings after your yes). |

You can also just describe what you want; `GEMINI.md` carries the same rules.

## What it will and will not do

- It uses the main GrowFast server (`https://growfast.sh/api/mcp`) with `excludeTools` for `send_reply` and `set_autopilot` (they send real email) and for the pay-per-call tools `find_companies` and `research_company`. Nothing in this extension sends mail or posts to Reddit.
- `scan_domain` saves the scan to your account; `set_reddit_monitor` replaces the settings of that product's monitor. Both are real changes on your account.
- Drafts are starting points. Read them, edit them, and read the Reddit thread and the subreddit's rules before you post anything.
- No result is promised: nobody can tell you how many replies or customers you will get.
- Gemini CLI asks you to confirm MCP tool calls. The extension does not set `trust`; we recommend you leave it off.

## Contents

```text
gemini-extension.json        manifest: name, version, remote MCP server https://growfast.sh/api/mcp
.antigravity-plugin/plugin.json  Antigravity plugin manifest
mcp_config.json              Antigravity MCP server definition (same server)
rules/AGENTS.md              Antigravity always-on rule (a copy of GEMINI.md: keep both in sync)
GEMINI.md                    context: what GrowFast is and the rules above
commands/growfast/*.toml     /growfast:scan, :draft, :reddit
assets/icon.png              icon
scripts/validate.mjs         schema checks, no dependencies: node scripts/validate.mjs
```

## Data and privacy

The MCP server receives what the tools need: the domains, product details, company lists and keywords you ask Gemini to send, and the scan, outreach and Reddit data on your GrowFast account. The extension itself stores nothing and runs no code on your machine; it is a server address, Markdown and prompts. Sign-in tokens are stored by Gemini CLI. See the [privacy policy](https://growfast.sh/legal/privacy) and [terms](https://growfast.sh/legal/terms).

## Support

support@growfast.sh or https://growfast.sh. Documentation: https://growfast.sh/docs.

## License

MIT for the files in this repository (see `LICENSE`). The GrowFast service is governed by its terms.
