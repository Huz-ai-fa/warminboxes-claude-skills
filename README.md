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

## All 33 free tools

Every check a skill runs is also a page you can use in a browser, and most are
callable as an API. **API** marks the ones with an HTTP endpoint; the rest run
entirely in your browser.

### DNS & Authentication

- [BIMI Checker](https://warminboxes.com/bimi-checker) **API** — Logo record, SVG, VMC validation
- [CNAME Checker](https://warminboxes.com/cname-checker) **API** — Follow a full CNAME chain to find why a tracking domain will not resolve
- [DKIM Checker](https://warminboxes.com/dkim-checker) **API** — Auto-scans 26 common selectors
- [DMARC Checker](https://warminboxes.com/dmarc-checker) **API** — Policy, tags, and verdicts explained
- [DNS Checker](https://warminboxes.com/dns-checker) **API** — All record types
- [Domain Expiry Checker](https://warminboxes.com/domain-expiry-checker) **API** — Expiration monitoring via RDAP, falling back to WHOIS for ccTLDs that publish no RDAP service
- [MTA-STS Checker](https://warminboxes.com/mta-sts-checker) **API** — DNS record, policy file fetch, enforce mode, TLS-RPT
- [Nameserver Checker](https://warminboxes.com/nameserver-checker) **API** — Bulk NS lookup with DNS provider identification
- [Record Generator](https://warminboxes.com/record-generator) — Correct SPF/DKIM/DMARC with ESP presets
- [SPF Checker & Generator](https://warminboxes.com/spf-checker) **API** — Recursive lookup counting against the 10-lookup limit

### Diagnostics

- [Blacklist Checker](https://warminboxes.com/blacklist-checker) **API** — Domain/IP against 20+ blacklists
- [Bounce Analyzer](https://warminboxes.com/bounce-analyzer) — Paste NDRs/SMTP codes for plain-English root cause plus a pause-or-continue calculator
- [Deliverability Checker](https://warminboxes.com/deliverability-checker) **API** — A-F domain grade across SPF, DKIM, DMARC, MX, blacklists, domain age
- [Google Domain Checker](https://warminboxes.com/google-domain-checker) **API** — See if a domain is already tied to a Google Workspace account or clear for fresh Google inboxes
- [Header Analyzer](https://warminboxes.com/header-analyzer) — SPF/DKIM/DMARC, alignment, and hops from raw headers
- [Outlook SCL Analyzer](https://warminboxes.com/scl-analyzer) — Paste Microsoft 365 headers; explains SCL, BCL, SFV, composite auth, and why the message went to Junk
- [SURBL Checker](https://warminboxes.com/surbl-checker) **API** — Bulk-check domains against the SURBL URI blacklist, one domain or a whole CSV

### Campaign QA

- [Inbox Previewer](https://warminboxes.com/inbox-previewer) — Sender/subject/snippet as Gmail and Outlook render them
- [Pre-Send Checker](https://warminboxes.com/pre-send-checker) — Upload a lead CSV + paste a sequence; audits merge tags against real CSV columns, list quality, spintax, links/formatting, and simulates send volume with follow-up accumulation
- [Spam Checker](https://warminboxes.com/spam-checker) — 200+ weighted spam triggers and formatting red flags
- [Spintax & Liquid Tester](https://warminboxes.com/spintax-tester) **API** — Preview every spintax variation and personalization variable
- [Suppression Checker](https://warminboxes.com/suppression-checker) — Check a campaign list against customers/unsubscribes/bounces by exact email AND company domain; detects account collisions

### Lists & Planning

- [CSV Combiner](https://warminboxes.com/csv-combiner) — Merge several lead CSVs into one, deduped, with mismatched headers reconciled
- [CSV List Cleaner](https://warminboxes.com/csv-cleaner) — Dedupe, role/disposable removal, in-browser
- [Domain & Inbox Planner](https://warminboxes.com/inbox-planner) **API** — Target volume to exact domains/inboxes with ramp-up
- [ESP Calculator](https://warminboxes.com/esp-calculator) **API** — Upload a list and get the infrastructure built for the ESPs it actually contains
- [ESP Segmentation](https://warminboxes.com/esp-segmenter) **API** — Split lead lists by Google, Microsoft, and gateway-protected inboxes via MX
- [Inbox Rotation Planner](https://warminboxes.com/inbox-rotation-planner) — Two-batch rotation schedules
- [Infrastructure Calculator](https://warminboxes.com/calculator) **API** — Sends per day to the exact inbox and domain count, with cost
- [Sequencer CSV Converter](https://warminboxes.com/sequencer-csv-converter) — Convert lead CSVs between Instantly, Smartlead, and Email Bison formats
- [Unsubscribe Generator](https://warminboxes.com/unsubscribe-generator) **API** — Compliant footers (CAN-SPAM/GDPR)
- [Verification Waterfall Calculator](https://warminboxes.com/waterfall-calculator) — Cheapest verifier order and cost per usable email

### Writing

- [Cold Email AI](https://warminboxes.com/AI) — Cold email copy from an assistant trained on real deliverability data
