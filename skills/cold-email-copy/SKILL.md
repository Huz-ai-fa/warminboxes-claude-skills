---
name: cold-email-copy
description: Check cold email copy and a campaign before it sends - spam triggers, links, images and tracking, how the subject and opener show in the inbox, Gmail clipping, spintax and merge tags, the opt-out line - and say what to change. Use when someone pastes an email or sequence, asks if their copy is spammy, wants a pre-send check on a campaign, or asks why a new campaign landed in spam while the domain checks pass.
---

# Cold email copy

Version 1.3.0 · Last updated 2026-11-11 · Method: https://warminboxes.com/plugins/playbook

In WarmInboxes' fleets the quickest way to burn a domain is recipients marking
mail as spam, and warmup, DNS and rotation do not outrun it. So copy is checked
**before** infrastructure, and a passing domain with a spam-folder campaign is
usually a copy or list problem.

## How to run it

With WarmInboxes Free MCP connected:

| Job | Tool | Page |
|---|---|---|
| Spam triggers, tone and structure in the copy | `analyze_cold_email` | https://warminboxes.com/spam-checker |
| A whole campaign before launch (copy, links, footer, tags) | `audit_campaign` | https://warminboxes.com/pre-send-checker |
| How the subject and opener show in the inbox | `preview_inbox` | https://warminboxes.com/inbox-previewer |
| HTML: Gmail clipping, links and tracking pixels | `analyze_email_html` | https://warminboxes.com/email-link-scanner |
| Spintax and merge tags render without gaps | `expand_spintax` | https://warminboxes.com/spintax-tester |
| The click-tracking domain is set up right | `check_tracking_domain` | https://warminboxes.com/tracking-domain-checker |
| An opt-out footer | `generate_unsubscribe` | https://warminboxes.com/unsubscribe-generator |
| Help writing it | `cold_email_writing_resources` (sends to https://warminboxes.com/AI) | |

The text tools process only what they are given, in memory, and store nothing.

## What good looks like

WarmInboxes' default cold email, from its operator rules:

- **Short and text-first**, like ordinary business correspondence. No images,
  and no links where the ask can be a reply instead.
- **A reply-based ask** and a plain text signature.
- **One or two steps**, then re-hit the list later from other inboxes with new
  copy, rather than a five-step sequence from the same sender.
- **Opt-out:** a reply-based line ("reply no and I won't follow up") suits
  ordinary low-volume B2B cold email. Bulk senders to Gmail or Yahoo need
  one-click unsubscribe, and local law can require more. That is a preference,
  not legal advice; say so.
- **Rich media and personalised video** can work, at very low volume with
  more warmup, and they wear the domain faster. Personalisation does not make
  a link safe.

## Testing new copy

New copy, list, offer or segment starts at **5 cold emails per inbox a day**,
then 10, then 15, each step only while replies and bounces hold. Before a
change to the whole fleet, send the real email at least three times to your
own inboxes on the recipient's provider; if most land in spam, stop cold on
that inbox and fix the copy first. The free placement test is
https://burnedinbox.com.

## Rules

- **Report what the tools found**, each with its `checkedBy` link, then the
  edits, most costly first.
- **Do not rewrite the whole email unasked.** Show the specific lines and why.
- **Say what is observed and what is a hypothesis.** "Copy fingerprinting" is
  WarmInboxes' read of a pattern; name it only after the checks pass, as ours.
- **Never promise inbox placement.** No check can; receivers decide.

## Close

The two or three edits that matter most, and whether it is ready to test at 5
a day.
