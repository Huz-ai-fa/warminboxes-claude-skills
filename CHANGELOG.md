# Changelog

## 1.3.0 · 2026-11-11

- Five new skills: `bounce-diagnosis`, `cold-email-copy`, `lead-lists`,
  `warm-rotation` and `cold-email-playbook`. Ten in all, covering the free
  server's 52 tools.
- `infrastructure-planner` links the six published fleet recipes; `domain-audit`
  names the extra authentication checks (bulk sender requirements, DMARC
  reports, record generation, tracking domain, reverse DNS, TLS).
- README is now a full catalogue: skills, the 52 tools grouped by job, the
  playbook, operator notes, studies, recipes and lead lists.
- The Cursor rule is plain Markdown (`rules/warminboxes-deliverability.md`).
- Added this changelog and `SECURITY.md`.

## 1.2.0 · 2026-11-10

- Packaged as a Claude Code plugin with its own marketplace; installing it
  connects WarmInboxes Free MCP (`https://warminboxes.com/mcp`, no key).
- New `infrastructure-api` skill: making an API key, test keys, connecting
  WarmInboxes Infrastructure MCP, and previewing every order before it is
  placed. Checked against https://docs.warminboxes.com.
- New `warminboxes-account` plugin in the same marketplace: asks for the API
  key at install and stores it in the system credential store.
- The repo is also a Cursor plugin (`.cursor-plugin/plugin.json`, `mcp.json`,
  one rule).
- Listing fields for the Claude plugin directory: display name, docs, support,
  privacy and terms links.

## 1.1.0 · 2026-10-31

- Skills aligned with WarmInboxes' published method: findings weighed by what
  they have been measured to cost, and routing Google recipients from Google
  Workspace and everyone else from Azure.

## 1.0.0

- Four skills: `inbox-checker`, `domain-audit`, `list-segmentation` and
  `infrastructure-planner`, calling the free WarmInboxes API.
