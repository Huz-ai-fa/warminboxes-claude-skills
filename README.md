# WarmInboxes plugin for Claude and Cursor

**Cold email deliverability and infrastructure, done by your AI directly.**
Check whether a domain is burned, audit one before you send, read bounces and
headers, review copy before it goes out, clean and segment lead lists, size an
inbox fleet, set up warm rotation, and work any drop in replies the way
WarmInboxes' operators do, with the research behind every call.

- **52 free tools** through WarmInboxes Free MCP: live DNS, blocklist and
  authentication checks, bounce and header reading, copy checks, list tools,
  planning and pricing, the diagnostic playbook and the research library.
  **No API key, no sign-up.**
- **10 skills** that teach the AI how to use them well: what to check first,
  what a finding is actually worth, and when buying anything is the wrong answer.
- **When you're ready for your own account**, a second plugin connects it with
  an API key, stored in your system's credential store, never in the chat.

Version 1.3.0, updated 2026-11-11.

## Install

**Claude Code**

```
/plugin marketplace add Huz-ai-fa/warminboxes-claude-skills
/plugin install warminboxes@warminboxes
```

**Cursor:** install **WarmInboxes** from the Cursor Marketplace once it is
listed, or add this repository as a plugin from Cursor's plugin settings.

**Claude apps and Cowork:** from the Claude plugin directory once it is listed.
WarmInboxes Free MCP on its own is also a custom connector at
`https://warminboxes.com/mcp`, no sign-in.

Then just ask. The skills decide themselves when they apply.

## The skills

| Skill | Answers | Try |
|---|---|---|
| `inbox-checker` | Is this domain burned? | "Is acme.com still safe to send from?" |
| `domain-audit` | Is this domain set up right to send from? | "Audit outreach-acme.com before I launch." |
| `bounce-diagnosis` | What do these bounces mean? | "Why are these bouncing?" + paste |
| `cold-email-copy` | Is this copy going to spam? | "Check this email before I send it." + paste |
| `lead-lists` | Where do I get leads, and is my list clean? | "Find me a free list of SaaS founders in the UK." |
| `list-segmentation` | Who hosts my recipients, and what should I send from? | "Split this CSV by email provider." |
| `infrastructure-planner` | How many inboxes do I need, and what does it cost? | "How many inboxes for 3,000 a day?" |
| `warm-rotation` | How do I make inboxes last longer? | "Plan a warm rotation for 60 inboxes on a 12-day sequence." |
| `cold-email-playbook` | Why did my replies drop, and what does the research say? | "My reply rate halved this week. What do I do?" |
| `infrastructure-api` | How do I connect my WarmInboxes account? | "How do I order inboxes from Claude?" |

Every skill follows the same rules:

- **Every verdict cites the check behind it**, with a link to re-run it by hand.
- **A check that did not run is reported as unknown**, never as a pass. The
  Spamhaus DBL is not queried and is reported as not checked.
- **No invented reputation scores.** Mailbox providers keep those private.
- **Free fixes come before paid ones.** A clean result is reported clean, with
  nothing to buy.
- **No secret passes through the chat**, and anything that charges a card is
  previewed and confirmed first.

## What's behind it

**The diagnostic playbook:** nine scenarios (reply rate dropped, blocklist
listing, Outlook junk, Gmail spam, bounces rising, is it burned, placement
tests, new setup, scaling), each with the questions to ask, the checks in
order and when new infrastructure is and isn't the answer.
https://warminboxes.com/plugins/playbook

**Operator notes:** about 90 rules from running fleets (the decision tree,
the 5 → 10 → 15 ramp, full rest on a sender-side bounce spike, rest or
replace, short sequences), each tagged with its evidence and confidence.
`find_resources` with `type: "note"` lists them.

**Research:** five published studies with stable IDs, data files and
citations. https://warminboxes.com/research

| ID | Study |
|---|---|
| WI-RESEARCH-2026-001 | [Does a SURBL listing hurt cold email? 2,817 sending domains](https://warminboxes.com/research/surbl-blocklist-study) |
| WI-RESEARCH-2026-002 | [Every inbox placement test on prewarmed inboxes](https://warminboxes.com/research/inbox-placement-log) |
| WI-RESEARCH-2026-003 | [Google reputation refusals by top-level domain](https://warminboxes.com/research/google-sender-reputation) |
| WI-RESEARCH-2026-004 | [Microsoft's Azure tenant crackdown and the 5.7.237 bounce](https://warminboxes.com/research/microsoft-azure-tenant-crackdown) |
| WI-RESEARCH-2026-005 | [Top-level domains ranked by abuse-list recurrence](https://warminboxes.com/blog/tld-reputation-cold-email) |

**Recipes:** fixed fleet scenarios with every figure shown, checked against
the planner engine at every build. https://warminboxes.com/recipes

| Recipe | |
|---|---|
| [500 cold emails a day, Google-heavy list](https://warminboxes.com/recipes/500-emails-a-day) | one pool, Google to Google |
| [1,000 a day, mixed Google and Microsoft](https://warminboxes.com/recipes/1000-emails-a-day) | a Google pool and an Azure pool |
| [5,000 a day, agency setup](https://warminboxes.com/recipes/5000-emails-a-day) | two pools at agency scale |
| [1,000 a day to a Microsoft-heavy list](https://warminboxes.com/recipes/microsoft-heavy-list) | Azure carries most of it |
| [Sending within the week, prewarmed](https://warminboxes.com/recipes/launching-this-week) | what prewarmed does and doesn't remove |
| [1,000 a day on fresh branded domains](https://warminboxes.com/recipes/fresh-domains-21-days) | your own domains, a 21-day runway |

**Free lead lists:** 999 B2B lists, emailed from each list's page.
https://warminboxes.com/leads

**Warm rotation:** the method, step by step. https://warminboxes.com/warm-rotation

## The 52 free tools

Every tool the plugin's server runs, with the page that does the same job in
a browser. Generated from the live server on 2026-11-11; the current list is
always https://warminboxes.com/tools.json.

### Domain health and authentication

| Tool | What it does | Page |
|---|---|---|
| `check_deliverability` | Deliverability grade (A-F) | [warminboxes.com/deliverability-checker](https://warminboxes.com/deliverability-checker) |
| `check_email_authentication` | Email authentication, all six records | [warminboxes.com/email-authentication-checker](https://warminboxes.com/email-authentication-checker) |
| `check_spf` | SPF record | [warminboxes.com/spf-checker](https://warminboxes.com/spf-checker) |
| `check_dkim` | DKIM record | [warminboxes.com/dkim-checker](https://warminboxes.com/dkim-checker) |
| `check_dmarc` | DMARC record | [warminboxes.com/dmarc-checker](https://warminboxes.com/dmarc-checker) |
| `check_bimi` | BIMI record | [warminboxes.com/bimi-checker](https://warminboxes.com/bimi-checker) |
| `check_mta_sts` | MTA-STS policy | [warminboxes.com/mta-sts-checker](https://warminboxes.com/mta-sts-checker) |
| `check_bulk_sender_requirements` | Gmail, Yahoo and Microsoft bulk sender requirements | [warminboxes.com/bulk-sender-requirements-checker](https://warminboxes.com/bulk-sender-requirements-checker) |
| `generate_dns_records` | SPF and DMARC record generator | [warminboxes.com/record-generator](https://warminboxes.com/record-generator) |
| `analyze_dmarc_report` | DMARC aggregate report | [warminboxes.com/dmarc-report-analyzer](https://warminboxes.com/dmarc-report-analyzer) |
| `check_dns` | DNS records | [warminboxes.com/dns-checker](https://warminboxes.com/dns-checker) |
| `lookup_txt` | TXT records, identified | [warminboxes.com/txt-record-lookup](https://warminboxes.com/txt-record-lookup) |
| `check_cname` | CNAME chain | [warminboxes.com/cname-checker](https://warminboxes.com/cname-checker) |
| `check_nameservers` | Nameservers and DNS provider (bulk) | [warminboxes.com/nameserver-checker](https://warminboxes.com/nameserver-checker) |
| `check_rdns` | Reverse DNS and forward confirmation | [warminboxes.com/reverse-dns-lookup](https://warminboxes.com/reverse-dns-lookup) |
| `check_ssl` | TLS certificate | [warminboxes.com/ssl-checker](https://warminboxes.com/ssl-checker) |
| `check_domain_expiry` | Domain registration and expiry | [warminboxes.com/domain-expiry-checker](https://warminboxes.com/domain-expiry-checker) |
| `check_google_domain` | Google Workspace tenancy | [warminboxes.com/google-domain-checker](https://warminboxes.com/google-domain-checker) |
| `check_tracking_domain` | Click-tracking domain | [warminboxes.com/tracking-domain-checker](https://warminboxes.com/tracking-domain-checker) |

### Reputation and blocklists

| Tool | What it does | Page |
|---|---|---|
| `check_domain_reputation` | Is the domain the problem | [warminboxes.com/domain-reputation-checker](https://warminboxes.com/domain-reputation-checker) |
| `check_blacklist` | Blacklist / RBL check | [warminboxes.com/blacklist-checker](https://warminboxes.com/blacklist-checker) |
| `check_surbl` | SURBL URI blacklist (bulk) | [warminboxes.com/surbl-checker](https://warminboxes.com/surbl-checker) |
| `spamhaus_dbl_guidance` | Spamhaus DBL guidance (not queried) |[check.spamhaus.org](https://check.spamhaus.org) |
| `burned_inbox_guidance` | Inbox placement test and Burned Inbox Checker | [burnedinbox.com](https://burnedinbox.com) |

### Bounces and headers

| Tool | What it does | Page |
|---|---|---|
| `analyze_bounces` | Bounce messages explained | [warminboxes.com/bounce-analyzer](https://warminboxes.com/bounce-analyzer) |
| `analyze_headers` | Email header analysis | [warminboxes.com/header-analyzer](https://warminboxes.com/header-analyzer) |
| `analyze_microsoft_headers` | Microsoft spam verdict (SCL) | [warminboxes.com/scl-analyzer](https://warminboxes.com/scl-analyzer) |

### Copy and campaign checks

| Tool | What it does | Page |
|---|---|---|
| `analyze_cold_email` | Cold email spam check | [warminboxes.com/spam-checker](https://warminboxes.com/spam-checker) |
| `audit_campaign` | Pre-send campaign audit | [warminboxes.com/pre-send-checker](https://warminboxes.com/pre-send-checker) |
| `preview_inbox` | Inbox preview (truncation) | [warminboxes.com/inbox-previewer](https://warminboxes.com/inbox-previewer) |
| `analyze_email_html` | Email HTML: clipping, links and pixels | [warminboxes.com/gmail-clipping-checker](https://warminboxes.com/gmail-clipping-checker) |
| `expand_spintax` | Spintax and merge tag preview | [warminboxes.com/spintax-tester](https://warminboxes.com/spintax-tester) |
| `generate_unsubscribe` | CAN-SPAM opt-out footer | [warminboxes.com/unsubscribe-generator](https://warminboxes.com/unsubscribe-generator) |
| `cold_email_writing_resources` | Cold email writing resources | [warminboxes.com/AI](https://warminboxes.com/AI) |

### Lead lists

| Tool | What it does | Page |
|---|---|---|
| `find_lead_lists` | Free B2B lead lists (WarmInboxes catalogue) |[warminboxes.com/leads](https://warminboxes.com/leads) |
| `clean_csv` | Lead CSV cleaner | [warminboxes.com/csv-cleaner](https://warminboxes.com/csv-cleaner) |
| `combine_csvs` | Combine lead CSVs | [warminboxes.com/csv-combiner](https://warminboxes.com/csv-combiner) |
| `convert_csv` | Sequencer CSV converter | [warminboxes.com/sequencer-csv-converter](https://warminboxes.com/sequencer-csv-converter) |
| `check_suppression` | Suppression list check | [warminboxes.com/suppression-checker](https://warminboxes.com/suppression-checker) |
| `verify_email` | Email verification (bulk) |[warminboxes.com/email-verifier](https://warminboxes.com/email-verifier) |
| `plan_verification_waterfall` | Email verification waterfall | [warminboxes.com/waterfall-calculator](https://warminboxes.com/waterfall-calculator) |
| `lookup_mx` | MX records and mail provider | [warminboxes.com/mx-lookup](https://warminboxes.com/mx-lookup) |
| `segment_esp` | ESP segmentation (bulk) | [warminboxes.com/esp-segmenter](https://warminboxes.com/esp-segmenter) |

### Planning and pricing

| Tool | What it does | Page |
|---|---|---|
| `plan_infrastructure` | Inboxes and domains for a send volume (estimate) | [warminboxes.com/inbox-planner](https://warminboxes.com/inbox-planner) |
| `plan_from_list` | Fleet estimate from a recipient list | [warminboxes.com/esp-calculator](https://warminboxes.com/esp-calculator) |
| `quote_inboxes` | Price a number of inboxes (quote) |[warminboxes.com/calculator](https://warminboxes.com/calculator) |
| `plan_rotation` | Inbox rotation schedule (suggested) | [warminboxes.com/inbox-rotation-planner](https://warminboxes.com/inbox-rotation-planner) |
| `get_pricing` | Price list |[warminboxes.com/price.md](https://warminboxes.com/price.md) |

### Playbook and research

| Tool | What it does | Page |
|---|---|---|
| `get_diagnostic_playbook` | How to work a cold email problem (playbook) |[warminboxes.com/plugins/playbook](https://warminboxes.com/plugins/playbook) |
| `find_resources` | Guides, playbooks, studies and videos |[warminboxes.com/research](https://warminboxes.com/research) |
| `read_resource` | Read a guide or study |[warminboxes.com/blog](https://warminboxes.com/blog) |
| `find_tools` | Free tool for a job |[warminboxes.com/deliverability-tools](https://warminboxes.com/deliverability-tools) |

## When you're ready for your own account

The free side needs nothing. Acting on your own WarmInboxes account (orders,
build state, billing, nameservers, exports into Instantly, Smartlead, PlusVibe
or Email Bison, and placing orders) goes through **WarmInboxes Infrastructure
MCP** with an API key. In Claude Code:

```
/plugin install warminboxes-account@warminboxes
```

1. **Have an account.** A first order is placed in the app,
   https://app.warminboxes.com.
2. **Make a key** in the app: Settings → API keys. Pick only the scopes you
   need (`read`, `write`, `orders:place`). Start with a **test key**: it runs
   everything against a sandbox that charges and builds nothing.
3. **Install the account plugin.** It asks for the key in a masked field and
   keeps it in your system's secure credential store. Never paste a key into
   a chat.
4. Ask for your dashboard. Before any order, the AI previews it, shows you
   the price and waits for your yes.

Other clients and the `wib` command line: https://docs.warminboxes.com.

## Your data

- What the tools are sent (a domain, pasted headers, bounces, copy or a CSV)
  is processed in memory to produce the answer. It is not stored and its
  contents are not logged.
- A daily limit counts distinct domains against the requesting IP address;
  those records are deleted within two days.
- Anonymous usage events (which tool, the kind of client, a coarse outcome;
  no domain, no content, no identifier) are kept 90 days. Saved fleet plans
  (counts and prices only) are kept 30 days.
- Full policy: https://warminboxes.com/privacy

## Skills only, without the plugin

Each skill folder is self-contained. Copy one into `~/.claude/skills/`:

```sh
mkdir -p ~/.claude/skills/domain-audit
curl -sL https://raw.githubusercontent.com/Huz-ai-fa/warminboxes-claude-skills/main/domain-audit/SKILL.md \
  -o ~/.claude/skills/domain-audit/SKILL.md
```

Without the plugin's server connection, the diagnostic skills call the same
free API over HTTPS at `warminboxes.com/.netlify/functions/<tool>`, no key.

## Rate limits

The free tools have a soft daily limit sized for a person doing real work,
counted in distinct domains. Batch where a tool takes arrays
(`check_surbl`, `check_nameservers`, `segment_esp`). Past the limit you get a
`429` naming the two keyed APIs: https://api.warminboxes.com and
https://deliverabilitymonitor.com.

## Links

- Free tools in a browser: https://warminboxes.com/deliverability-tools
- Machine-readable: https://warminboxes.com/tools.json · https://warminboxes.com/openapi.json · https://warminboxes.com/llms.txt
- Account API and Infrastructure MCP: https://docs.warminboxes.com
- Placement test: https://burnedinbox.com
- [CHANGELOG](CHANGELOG.md) · [SECURITY](SECURITY.md) · [LICENSE](LICENSE) (MIT)
