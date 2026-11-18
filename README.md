# WarmInboxes

**Cold email deliverability and infrastructure, done by Claude directly.**

```
/plugin install warminboxes --marketplace Huz-ai-fa/warminboxes-claude-skills
```

On Claude Code before 2.1.275, add the marketplace first:
`/plugin marketplace add Huz-ai-fa/warminboxes-claude-skills`, then
`/plugin install warminboxes@warminboxes`.

Installing this plugin connects **WarmInboxes Free MCP**
(`https://warminboxes.com/mcp`): 52 read-only tools, no API key, no sign-up.
They cover:

- live DNS, blocklist and authentication checks;
- bounce and header reading;
- cold email copy checks;
- lead-list cleaning and segmentation;
- inbox fleet sizing, warm rotation and pricing;
- WarmInboxes' diagnostic playbook and published research.

It also adds ten skills that teach Claude how to use those tools well:

| Skill | Answers |
|---|---|
| `inbox-checker` | Is this domain burned? |
| `domain-audit` | Is this domain set up right to send from? |
| `bounce-diagnosis` | What do these bounces mean? |
| `cold-email-copy` | Is this copy going to spam? |
| `lead-lists` | Where do I get leads, and is my list clean? |
| `list-segmentation` | Who hosts my recipients, and what should I send from? |
| `infrastructure-planner` | How many inboxes do I need, and what does it cost? |
| `warm-rotation` | How do I make inboxes last longer? |
| `cold-email-playbook` | Why did my replies drop, and what does the research say? |
| `infrastructure-api` | How do I connect my WarmInboxes account? |

Every verdict cites the check behind it. A check that did not run is reported
as unknown, never as a pass. No reputation scores are invented: mailbox
providers keep those private.

## Privacy

Domains and text sent to the tools are processed in memory to produce the
answer, and are not stored. A daily limit counts distinct domains against the
requesting IP address; those records are deleted within two days. Anonymous
usage events (which tool, the kind of client, a coarse outcome) are kept 90
days. Full policy: https://warminboxes.com/privacy

## Links

- Documentation: https://warminboxes.com/skills
- Support: https://warminboxes.com/consultation
- Your own account (API key): the `warminboxes-account` plugin in the same
  marketplace, and https://docs.warminboxes.com
- Licence: MIT (see `LICENSE`)
