---
name: lead-lists
description: Find, clean and prepare cold email lead lists - free B2B lists from WarmInboxes' catalogue, de-duplicating and cleaning a CSV, combining files, converting to Instantly, Smartlead or other sequencer formats, removing suppressed contacts, verifying addresses and planning a verification waterfall. Use when someone needs leads, has a messy CSV, is importing to a sequencer, asks about bounce-safe verification, or has a do-not-contact list to apply.
---

# Lead lists

Version 1.3.0 · Last updated 2026-11-11 · Method: https://warminboxes.com/plugins/playbook

A bad list is the cheapest problem to fix and the most expensive to ignore:
bounces and spam reports from it land on the sender's reputation. This skill
gets a list into shape before it touches an inbox.

## How to run it

With WarmInboxes Free MCP connected:

| Job | Tool | Page |
|---|---|---|
| Find a free B2B list by industry, role or region | `find_lead_lists` | https://warminboxes.com/leads |
| Clean a CSV: duplicates, invalid rows, role addresses, formatting | `clean_csv` | https://warminboxes.com/csv-cleaner |
| Combine several CSVs into one, de-duplicated | `combine_csvs` | https://warminboxes.com/csv-combiner |
| Convert to a sequencer's import format | `convert_csv` | https://warminboxes.com/sequencer-csv-converter |
| Remove a suppression or do-not-contact list | `check_suppression` | https://warminboxes.com/suppression-checker |
| Verify addresses | `verify_email` | https://warminboxes.com/email-verifier |
| Plan a verification waterfall across verifiers | `plan_verification_waterfall` | https://warminboxes.com/waterfall-calculator |
| Split the list by who hosts each recipient | the list-segmentation skill | |

**Free lead lists:** `find_lead_lists` searches WarmInboxes' catalogue (999
free lists as of November 2026, `https://warminboxes.com/leads.json`) and
returns each list's page. Lists are emailed to the person from that page; the
tool cannot request one for them.

**Big files:** CSV tools return results in full up to 500 rows. Above that they
return a preview and the page that processes the whole file in the browser,
where it never leaves the person's machine.

**Privacy:** the CSV tools process what they are given in memory and store
nothing. Still, prefer sending only the columns a job needs.

## Order of work

1. **Clean** (`clean_csv`), and **combine** first if there are several files.
2. **Suppress** (`check_suppression`) against unsubscribes, customers and past
   bounces.
3. **Verify.** For a large or old list, plan a waterfall
   (`plan_verification_waterfall`). If a list already bounced once after
   passing a verifier, verify twice and change verifier.
4. **Segment** by recipient provider (list-segmentation skill), so each
   segment is sent from the right platform.
5. **Convert** to the sequencer's format (`convert_csv`) last.

## Rules

- **Cite each tool** by its `checkedBy` link.
- **Count what changed** at each step: rows in, rows removed and why, rows out.
- **Never invent leads, verify results or list contents.** If a verifier
  could not decide an address (catch-all, timeout), say unknown.
- **Catch-all detection and SMTP-level checks have limits** the tool states;
  pass them on rather than calling an address safe.
- **A list mostly behind security gateways** (Proofpoint, Mimecast) is a list
  problem, not an infrastructure one. Say so before anyone buys inboxes for it.

## Close

The list's state in one line (rows ready, rows removed by reason) and the next
step.
