# GrowFast for Gemini CLI

> GrowFast is a B2B lead-finding and cold-outreach service built for AI agents to operate over MCP and REST: the agent scans a product's domain, finds matching companies with an MX-checked contact address, drafts the emails and, when told to, sends them with follow-ups; the human owner approves and audits.

## At a glance

- **What it is**: GrowFast is a B2B lead-finding and cold-outreach service built for AI agents to operate over MCP and REST. This repository connects Gemini CLI to the GrowFast MCP server.
- **Who it is for**: AI agents that find customers for a person or company; B2B SaaS founders, small sales teams and agencies who assign that work to an agent and approve it; individuals and small businesses who ask an assistant who their customers could be.
- **What it does not do**: promise leads, replies or meetings; guess email addresses; reach channels other than email (plus Reddit drafts you post yourself). The extension excludes the tools that send email (send_reply, set_autopilot).
- **Price**: 3 domain scans a day are free, with no sending. Sending needs a paid plan (from $49/month: 250 outreach emails and 1,000 domain scans a month) or a credit pack (from $29 for 750 emails, one-time, no subscription). Current prices: https://growfast.sh/pricing.md
- **Integrations**: AISend (email delivery), your own Reddit account, Google sign-in, MCP clients over OAuth 2.1 or an API key.
- **Security basics**: API keys are stored hashed and revocable in Settings > API keys; OAuth shows a consent screen and its key expires after 90 days by default; every email carries your postal address and a one-click unsubscribe link.
- **MCP server**: `https://growfast.sh/api/mcp`; Official MCP Registry name `sh.growfast/growfast`.

## Connect in 60 seconds

```bash
gemini extensions install https://github.com/airdropfi/growfast-gemini-extension
# restart Gemini CLI, then sign in with OAuth:
/mcp auth growfast
```

You need a GrowFast account (https://growfast.sh/sign-up). When you approve, GrowFast shows a consent screen naming the app and what it may do, and creates a revocable API key.

## Example prompts

You tag the work and review the result.

- Assign to GrowFast: scan mynewsletter.com and tell me which kinds of companies would pay for it, with the 5 best fits and why each scored what it did. Show me what you did so I can review it.
- Assign to GrowFast: scan mybakery-supplies.com, then draft first emails to the top 3 companies. Don't send anything. Show me each draft and the page the address was read from so I can approve.
- Assign to GrowFast: listen on Reddit for people who need a tool like mynewsletter.com (keywords: newsletter tool, grow newsletter, find subscribers). Don't post anything; show me the matches with drafted replies and I will choose.
- Assign to GrowFast: for clientsite.com scan the domain, then draft first emails to the 5 best-fit companies, addressed to the named decision-maker where the site names one. Summarise what you did and wait for my approval before anything else.

## Links

- Site: https://growfast.sh
- MCP page, install for every client: https://growfast.sh/mcp
- Agent quickstart (discovery, auth, limits, error codes, human oversight): https://growfast.sh/agents
- MCP server card: https://growfast.sh/.well-known/mcp/server-card.json
- llms.txt: https://growfast.sh/llms.txt
- Privacy: https://growfast.sh/legal/privacy · Terms: https://growfast.sh/legal/terms

---

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
