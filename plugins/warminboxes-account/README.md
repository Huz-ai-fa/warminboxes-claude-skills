# WarmInboxes account plugin

Connects your own WarmInboxes account to Claude Code through WarmInboxes
Infrastructure MCP (`https://api.warminboxes.com/mcp`): your orders and build
state, billing, nameservers, exports into Instantly, Smartlead, PlusVibe or
Email Bison, and orders, previewed and confirmed before anything is charged.

```
/plugin install warminboxes-account --marketplace Huz-ai-fa/warminboxes-claude-skills
```

On Claude Code before 2.1.275, add the marketplace first:
`/plugin marketplace add Huz-ai-fa/warminboxes-claude-skills`, then
`/plugin install warminboxes-account@warminboxes`.

Claude Code asks for your API key in a masked field and stores it in your
system's secure credential store. Make the key in the app at
https://app.warminboxes.com under Settings, then API keys; start with a test
key, which uses a sandbox and charges nothing. To change it:
`/plugin configure warminboxes-account@warminboxes`.

Pair it with the `warminboxes` plugin, whose `infrastructure-api` skill tells
Claude how to work the account safely. Full reference:
https://docs.warminboxes.com. Licence: MIT (see `LICENSE`).
