# WarmInboxes plugin and skills for Claude

Cold email infrastructure work, done by Claude directly: check whether a
domain is burned, audit one before you send from it, split a list by the
recipient's provider, size a mailbox order for a target volume, and, when you
are ready, connect your own WarmInboxes account with an API key.

Version 1.2.0, updated 2026-11-10.

## Install the plugin (Claude Code)

```
/plugin marketplace add Huz-ai-fa/warminboxes-claude-skills
/plugin install warminboxes@warminboxes
```

Or in one step on Claude Code 2.1.275 or later:

```
/plugin install warminboxes --marketplace Huz-ai-fa/warminboxes-claude-skills
```

The plugin brings two things:

- **WarmInboxes Free MCP** (`https://warminboxes.com/mcp`), connected as
  `warminboxes-free`. Read-only deliverability diagnostics, DNS, blocklists,
  copy and list tools, fleet sizing and pricing. **No API key and no signup.**
- **Five skills**, which tell Claude how to use those tools well:

| Skill | Answers |
|---|---|
| `inbox-checker` | Is this domain burned? |
| `domain-audit` | Is this domain set up correctly to send from? |
| `list-segmentation` | Who hosts the people on this list, and what should I send from? |
| `infrastructure-planner` | How many mailboxes do I need, and what does it cost? |
| `infrastructure-api` | I have a WarmInboxes account: how do I connect it with an API key, and order or manage inboxes from Claude? |

Then just ask. "Is acme.com safe to send from?" reaches `inbox-checker`
without naming it; "how do I connect my WarmInboxes account to Claude?"
reaches `infrastructure-api`.

## When you are ready for your account: the API key

The free side needs nothing. Acting on your own WarmInboxes account (reading
your orders and build state, exporting inboxes to your sequencer, placing
orders on your card) goes through **WarmInboxes Infrastructure MCP** at
`https://api.warminboxes.com/mcp`, and that needs an API key. The
`infrastructure-api` skill walks through it; the short version:

1. **Have an account.** A first order is placed in the app,
   https://app.warminboxes.com.
2. **Make a key** in the app: Settings → API keys. Pick only the scopes you
   need (`read`, `write`, `orders:place`). Switch on "Test key" first to try
   everything against a sandbox that charges and builds nothing.
3. **Connect it in your own terminal**, never by pasting the key into a chat:

   ```sh
   export WIB_API_KEY=wib_your_key_here
   claude mcp add --transport http warminboxes-infrastructure https://api.warminboxes.com/mcp \
     --header "Authorization: Bearer $WIB_API_KEY"
   ```

4. Ask Claude for your dashboard. Before any order, Claude previews it, shows
   you the price and waits for your yes.

The full reference is https://docs.warminboxes.com (plain text:
https://warminboxes.com/transactional-api.md).

## Cowork and Claude Desktop

The plugin's skills and Free MCP work wherever Claude plugins do. WarmInboxes
Free MCP can also be added on its own as a custom connector with the URL
`https://warminboxes.com/mcp`; it needs no sign-in.

## Skills only, without the plugin

Each skill folder is self-contained, so you can take one or all of them
without the plugin. Your skills directory is `~/.claude/skills/` for Claude
Code, or the skills folder in settings for Claude Desktop.

```sh
git clone https://github.com/Huz-ai-fa/warminboxes-claude-skills
cp -r warminboxes-claude-skills/domain-audit ~/.claude/skills/
```

Or take one straight from GitHub:

```sh
mkdir -p ~/.claude/skills/domain-audit
curl -sL https://raw.githubusercontent.com/Huz-ai-fa/warminboxes-claude-skills/main/domain-audit/SKILL.md \
  -o ~/.claude/skills/domain-audit/SKILL.md
```

Without the plugin's MCP connection the diagnostic skills call the same free
API over HTTPS, no key needed. `infrastructure-api` also has a
`references/` folder; copy the whole folder.

## What the skills share

The method is the one WarmInboxes' own agents use, published at
https://warminboxes.com/plugins/playbook. Findings are weighed by what they
have been measured to cost (a SURBL listing is common and, in WarmInboxes'
data, did not move reply rate; DMARC `p=none` is a pass), and routing is
Google recipients from Google Workspace, everyone else from Azure.

Each skill carries the same contract, written into every `SKILL.md` rather
than imported, so the folders stay independent:

- **Every verdict cites the check that produced it.** Each result returns a
  `checkedBy` object with the tool's URL, so anything Claude claims can be
  re-run by hand.
- **A check that did not run is reported as unknown**, never as a pass.
- **Free fixes come before paid ones.** Most findings are a DNS record away
  from solved, and the API returns the path to the fix.
- **A product recommendation needs a stated threshold**: a burned domain, a
  fleet too big to hand-manage, a volume target that needs an order. A clean
  result is reported clean, with nothing to buy.
- **No secret passes through the conversation.** API keys, sequencer
  passwords and registrar passwords go in your terminal, your client's config
  or the app, never in chat. Anything that charges a card is previewed and
  confirmed first.

## Rate limits

The free API is open, with a soft daily limit sized for a person doing real
work rather than a script, counted in distinct domains. Past it you get a
`429` naming the two keyed APIs: https://api.warminboxes.com and
https://deliverabilitymonitor.com.

Batch where the endpoint supports it: `check-surbl`, `check-nameservers` and
`segment-esp` all take arrays, and one call with fifty domains counts far
better than fifty calls.

## Related

- Free tools, in a browser: https://warminboxes.com/deliverability-tools
- Machine-readable index: https://warminboxes.com/tools.json
- OpenAPI: https://warminboxes.com/openapi.json
- Free MCP: https://warminboxes.com/mcp
- Infrastructure MCP and API reference: https://docs.warminboxes.com
- For agents: https://warminboxes.com/for-ai
- Burned inbox and placement checks: https://burnedinbox.com

The full, current list of free tools, with which ones an assistant can run and
how, is generated at https://warminboxes.com/tools.json and shown at
https://warminboxes.com/deliverability-tools. It is not copied here, because
a copy here was 22 tools out of date.

## Licence

MIT. See [LICENSE](LICENSE).
