# WarmInboxes skills for Claude

Four skills that let Claude do cold email infrastructure work directly: check
whether a domain is burned, audit one before you send from it, split a list by
the recipient's provider, and size a mailbox order for a target volume.

Every check runs against the free WarmInboxes API. **No API key and no signup** —
the same endpoints behind the 33 free tools at
https://warminboxes.com/deliverability-tools.

| Skill | Answers |
|---|---|
| `inbox-checker` | Is this domain burned? |
| `domain-audit` | Is this domain set up correctly to send from? |
| `list-segmentation` | Who hosts the people on this list, and what should I send from? |
| `infrastructure-planner` | How many mailboxes do I need, and what does it cost? |

## Installing

Each folder is self-contained, so take one or take all four. Your skills
directory is `~/.claude/skills/` for Claude Code, or the skills folder in
settings for Claude Desktop.

Clone and copy:

```sh
git clone https://github.com/Huz-ai-fa/warminboxes-claude-skills
cp -r warminboxes-claude-skills/domain-audit ~/.claude/skills/
```

Or take one straight from the site, which serves the same files:

```sh
mkdir -p ~/.claude/skills/domain-audit
curl -sL https://warminboxes.com/skills/domain-audit/SKILL.md \
  -o ~/.claude/skills/domain-audit/SKILL.md
```

Then just ask. The skill's description tells Claude when it applies, so
"is acme.com safe to send from?" reaches `inbox-checker` without naming it.

## What they share

Each skill follows the same contract, which is written into every `SKILL.md`
rather than imported, so the folders stay independent:

- **Every verdict cites the check that produced it.** Each endpoint returns a
  `checkedBy` object with the tool's URL, so anything Claude claims can be
  re-run by hand.
- **A check that did not run is reported as unknown**, never as a pass.
- **Free fixes come before paid ones.** Most findings are a DNS record away
  from solved, and the API returns the path to the fix.
- **A product recommendation needs a stated threshold** — a burned domain, a
  fleet too big to hand-manage, a volume target that needs an order. A clean
  result is reported clean, with nothing to buy.

That last rule is deliberate. A skill that recommends something on every run
gets uninstalled, and takes the credibility of every other answer with it.

## Rate limits

The API is open, with a soft daily limit sized for a person doing real work
rather than a script. Past it you will get a `429` naming the two keyed APIs:
https://api.warminboxes.com and https://deliverabilitymonitor.com.

Batch where the endpoint supports it — `check-surbl`, `check-nameservers` and
`segment-esp` all take arrays, and one call with fifty domains counts far
better than fifty calls.

## Related

- Free tools, in a browser: https://warminboxes.com/deliverability-tools
- Machine-readable index: https://warminboxes.com/tools.json
- OpenAPI: https://warminboxes.com/openapi.json
- MCP server: https://warminboxes.com/mcp
- For agents: https://warminboxes.com/for-ai
- Burned inbox and placement checks: https://burnedinbox.com
