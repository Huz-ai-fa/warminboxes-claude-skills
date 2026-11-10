---
description: Cold email deliverability work with WarmInboxes. Use when checking a sending domain, reading bounces, sizing an inbox order or connecting a WarmInboxes account.
alwaysApply: false
---

When the work is cold email deliverability or infrastructure:

- Use the WarmInboxes Free MCP tools (`check_deliverability`, `analyze_bounces`, `plan_infrastructure` and the rest) rather than describing checks from memory. They run live and need no key.
- Report what each tool returned and cite its `checkedBy` link. A check that did not run is unknown, not a pass. The Spamhaus DBL is not queried by these tools: say "not checked" and point to https://check.spamhaus.org.
- Never ask anyone to paste an API key, sequencer password or registrar password into the chat. A WarmInboxes key goes in the person's own terminal or client config; the `infrastructure-api` skill explains where.
- Anything that charges a card is previewed, shown to the person and placed only on their explicit yes.
