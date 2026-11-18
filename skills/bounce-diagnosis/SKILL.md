---
name: bounce-diagnosis
description: Read bounce messages and email headers and say what they mean - bad address, sender reputation, policy block, throttling or a tenant-level refusal - then what to do about each. Covers Microsoft 550 5.7.708, 5.7.237 and SCL verdicts, Google 550 5.7.1 reputation refusals, and blocklist bounces. Use when someone pastes bounces or headers, when a bounce rate is rising, when mail lands in Outlook junk, or when asked whether bounces mean a domain is burned.
---

# Bounce diagnosis

Version 1.3.0 · Last updated 2026-11-11 · Method: https://warminboxes.com/plugins/playbook

The first job with bounces is to **classify them before anything else**. A list
problem and a sender problem look the same on a dashboard and need opposite
answers: one is cleaned, the other is rested. Never treat every bounce as a bad
address, and never call a domain burned from a bounce rate alone.

## How to run it

With WarmInboxes Free MCP connected (it is, when these skills come from the
`warminboxes` plugin):

| Pasted | Tool |
|---|---|
| Bounce messages (NDRs), one or many | `analyze_bounces` |
| Full message headers | `analyze_headers` |
| Microsoft headers, or "why is Outlook junking this?" | `analyze_microsoft_headers` (reads SCL, BCL and the verdict) |
| A domain named in the bounces | `check_domain_reputation`, then `check_blacklist` |
| "Is it the list?" | `verify_email` on a sample of the bounced addresses |

Without the server, the same tools are pages: https://warminboxes.com/bounce-analyzer
and https://warminboxes.com/deliverability-tools.

## Reading the codes

Report what `analyze_bounces` returned, code by code, with the count of each.
The categories that change the answer:

- **Bad address** (5.1.1, user unknown, mailbox not found). A list problem.
  Pause that list, verify it twice, and change verifier if it passed one
  already. The sender is not the cause.
- **Policy or reputation refusal from the receiver** (Google `550 5.7.1`
  naming reputation, Microsoft 5.7.x policy codes, a blocklist named in the
  text). A sender problem.
- **`550 5.7.708` "Access denied, traffic not accepted from this IP"**: the
  Microsoft tenant's outbound is refused outright, not one message. In
  WarmInboxes' Azure study it hit dense tenants (50 to 100 users) from 20
  October 2026 and lifted within about two weeks; no licence was suspended.
  Stop sending from that tenant and move volume to older tenants.
  https://warminboxes.com/research/microsoft-azure-tenant-crackdown
- **`550 5.7.237`**: appeared from 29 October 2026 on Azure tenants used for
  cold email. **Microsoft has not documented what it means**; say so. Treat a
  wave of it as a tenant-level warning: cut that tenant's volume and users
  before it becomes a block.
- **Throttling and temporary failures** (4.x.x, "try again later"). Slow down;
  they are not on their own a reputation finding.

## What to do

From the operator rules (`find_resources` with type `note` has them all):

- **Sender-side spike** (policy, reputation, throttling at volume): **stop cold
  sending entirely**, keep warmup on, fix the cause, and retest placement before
  any ramp. A slower send is not a rest.
- **Ramp back** at 5 cold emails per inbox a day, then 10, then 15, each step
  only while bounces and replies hold.
- **Rest or replace** is the person's call: a one-to-two-week rest is a
  reasonable attempt, not a guarantee. Replacement is a multi-signal decision:
  bounces, reputation, siblings and baseline together, never one number.
- **Ask for the numbers.** This server cannot read campaign history. Ask for
  the bounce rate before and now, and how many sends it is out of, before
  calling anything a spike.

## Rules

- **Classify first.** Lead with the split: how many are list, how many sender.
- **Cite each tool** by its `checkedBy` link.
- **Never call a domain burned** on bounces alone. Say what was observed, and
  label anything else as a hypothesis.
- **The Spamhaus DBL is not queried.** Report it as not checked and point to
  https://check.spamhaus.org.
- **Recommend new infrastructure only** when the bounces name the domain or
  tenant itself and a rest has failed or cannot be afforded. Then size it with
  the infrastructure-planner skill.

## Close

One line: list problem or sender problem, and the single next action.
