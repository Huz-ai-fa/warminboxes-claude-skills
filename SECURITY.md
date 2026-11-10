# Security

## What this plugin can reach

- **`warminboxes` plugin:** WarmInboxes Free MCP at `https://warminboxes.com/mcp`.
  Every tool is read-only: it looks something up or reads what it is given, and
  changes nothing anywhere. No API key, no sign-in, no secrets.
- **`warminboxes-account` plugin (optional):** WarmInboxes Infrastructure MCP at
  `https://api.warminboxes.com/mcp`, which acts on your WarmInboxes account
  with the API key you give it. Its tools are limited by the key's scopes.

The plugins contain no scripts, hooks or binaries: Markdown skills, JSON
manifests and an icon.

## Keys and passwords

- A WarmInboxes API key is entered once, in Claude Code's masked field, and
  kept in your system's secure credential store. It is never written to a
  settings file or this repository.
- Never paste an API key, a sequencer password or a registrar password into a
  chat. The skills tell the AI to refuse to use one that is pasted.
- If a key may have leaked, revoke it in the app (Settings, then API keys). It
  stops working on the next request.
- Give an AI a key with only the scopes it needs. A key without `orders:place`
  can preview orders but never place one.

## Reporting a problem

Report a security issue privately through https://warminboxes.com/consultation,
or the WhatsApp line on https://warminboxes.com, rather than in a public issue.
Say what you found and how to reproduce it; we will reply and fix it as fast as
we can.

Data handling is described in the privacy policy: https://warminboxes.com/privacy
