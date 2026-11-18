---
name: infrastructure-api
description: Guide someone from the free WarmInboxes tools to their own WarmInboxes account through an API key - making the key, choosing its scopes, trying it on a test key, and connecting WarmInboxes Infrastructure MCP (api.warminboxes.com/mcp), the REST API or the wib CLI - then work the account safely, above all when an order would charge their card. Use when someone is ready to order inboxes from Claude, asks how to connect or authenticate their WarmInboxes account, mentions a wib_ key, api.warminboxes.com or the wib CLI, wants to check an order's build state, nameservers or billing, or wants a built order's inboxes exported into Instantly, Smartlead, PlusVibe or Email Bison.
---

# WarmInboxes Infrastructure MCP and API

Version 1.3.0 · Last updated 2026-11-11 · Reference: https://docs.warminboxes.com

This is the step after the free tools. WarmInboxes Free MCP, which this plugin
connects, is read-only and needs no key: it diagnoses domains, sizes fleets and
prices them. **WarmInboxes Infrastructure MCP** at
`https://api.warminboxes.com/mcp` acts on a person's own WarmInboxes account:
it reads their orders, domains, build state, billing and deliverability, changes
folders, names, forwarding and monitors, exports built inboxes into their
sequencer, and places orders that **charge the card on file**. It needs an
account and an API key. The REST API (`https://api.warminboxes.com/v1`) and the
`wib` command line are the same thing by other routes, with the same key and
rules.

The full reference is https://docs.warminboxes.com, as plain text at
https://docs.warminboxes.com/llms.txt and mirrored at
https://warminboxes.com/transactional-api.md. When a detail here and the docs
differ, the docs are right; fetch the text version when you need a field, a
status or a route this file does not cover.
`references/infrastructure-mcp.md` beside this file holds the tools, scopes,
statuses, errors, limits and sandbox values in one place.

## Where the person is

Work out which of these is true before giving steps. Ask if it is not clear.

1. **No account yet.** A first order is placed by the person in the app at
   https://app.warminboxes.com, not through the API. If they planned it with
   the free tools, the sizing result's `order.plan.url` is a saved plan whose
   order link carries it into the app. Do not walk them through API keys yet:
   there is nothing for a key to act on.
2. **Has an account, no key.** Walk them through making one (below).
3. **Has a key, not connected.** Help them connect it (below).
4. **Connected.** The Infrastructure MCP tools (`get_dashboard`,
   `preview_order`, `place_order` and the rest) are in the tool list. Work the
   account with the rules under "Working the account".

## The key never enters the conversation

This is the one rule that does not bend.

- **Never ask for the key, and never ask the person to paste it here.** Tell
  them where it goes instead: their terminal, their client's config file, or an
  environment variable.
- **If they paste one anyway**, do not repeat it, store it or use it. Tell them
  plainly that it is now in this conversation's history, and that they should
  revoke it in the app (Settings → API keys) and make a new one. Revoking takes
  effect on the next request.
- **Do not run the connect command for them with the key in it**, and do not
  echo, `cat` or print a config file or environment variable that holds one.
  Give the command; they run it in their own terminal.
- **A key is a password.** Anyone holding it can do what its scopes allow on
  the account. It never goes in a web page, a mobile app or a public
  repository (a committed `.mcp.json` included); it lives in their shell's
  environment, a server, or a client config that stays on their machine. The
  key list in the app shows when each key was last used: one nobody has used
  in months is one to revoke.
- The same goes for **sequencer passwords and registrar passwords**. There is
  deliberately no MCP tool for saving a sequencer login, so the password never
  passes through an assistant. Logins are saved in the app, or by the person
  with `PUT /v1/portal/sequencer-keys` or `wib sequencer-key-save -` (stdin, so
  it stays out of shell history).

## Making the key

In the app at https://app.warminboxes.com: **Settings → API keys**. Only the
account owner can make one.

- **Scopes** are fixed when the key is made and do not imply each other:
  - `read`: every read, plus `preview_order` (prices an order, creates nothing).
  - `write`: folders, order names, sequencer choice, monitors, forwarding,
    inbox renames, change requests, exports.
  - `orders:place`: placing an order (charges the card), opening a checkout,
    and the card set-up link.
- **Suggest the smallest key that does the job.** Reporting or "where is my
  order" needs only `read`. Exports and tidying need `read` + `write`. Only
  add `orders:place` if they want Claude to place orders; a key without it can
  still quote every order exactly.
- **Start with a test key.** Switch on "Test key" when making it. A
  `wib_test_…` key works on the same URL, tools and rules but answers from a
  pretend copy of the account: nothing is charged, built or changed, and every
  answer says it is the sandbox. Swap in a live key once the flow works.
- The full key is **shown once**. Only a hash is kept, so a lost key cannot be
  recovered: revoke it and make another.

A live key looks like `wib_<prefix>_<secret>`; a test key `wib_test_<prefix>_<secret>`.

## Connecting it

The server id `warminboxes-infrastructure` is the conventional one; keep it,
so it sits beside `warminboxes-free` without confusion.

**Claude Code: the account plugin (preferred).** The same marketplace has a
second plugin, `warminboxes-account`, that connects WarmInboxes Infrastructure
MCP and asks for the key itself. The person runs, in Claude Code:

```
/plugin install warminboxes-account@warminboxes
```

Claude Code opens a masked field for the key and keeps it in the system's
secure credential store, not in a settings file and not in this chat. To
change it later: `/plugin configure warminboxes-account@warminboxes`. Then
`/mcp` should show `warminboxes-infrastructure` as connected. Never offer to
type the key in for them.

**Cursor, Claude Desktop and other clients with a JSON config that takes a URL
and headers:** the person pastes their key in place of the placeholder in
their own config file:

```json
{
  "mcpServers": {
    "warminboxes-infrastructure": {
      "url": "https://api.warminboxes.com/mcp",
      "headers": { "Authorization": "Bearer wib_your_key_here" }
    }
  }
}
```

**Connector settings that only offer a sign-in button** (OAuth) have no field
for an API key, so the Infrastructure MCP cannot be added there. Say so, and
offer Claude Code, a JSON-config client or the app itself.

**Other routes** (the `wib` command line, a client that only runs
command-based servers): point the person to https://docs.warminboxes.com,
which has the current steps. Do not run downloads or installs for them.

**Checking it worked:** call `get_dashboard` (or `GET /v1/portal/dashboard`). A 401 means the key was not sent, not
recognised or revoked: have them check the `Bearer ` prefix and that the whole
key was copied. A 403 names the scope the key lacks. A test key's answers say
they come from the sandbox, `place_order` included; say so when you report
them.

The docs name the server `warminboxes` in their snippets; the id is only a
local label, so either works. Use `warminboxes-infrastructure` here so it
cannot be confused with this plugin's `warminboxes-free`.

The two servers do different jobs. Keep diagnosing with WarmInboxes Free MCP
(`check_deliverability`, `plan_infrastructure` and the rest) and act on the
account with the Infrastructure MCP. They share no routes.

## Working the account

**Reading** (`get_dashboard`, `list_orders`, `get_billing`,
`get_deliverability`, `get_nameservers`, `list_exports`) is safe to do when it
answers the question. Report what came back, in the API's own terms:

- An order's `status` runs `new_order` → `build_in_process` →
  `uploading_in_process` → `setup_completed` → `live`. `payment_failed`
  outranks everything.
- A domain at `waiting_on_you` needs its nameservers pointed at the pair in
  `nameservers`. That is the most common thing holding a build up, and
  `get_nameservers` says exactly what to set at the registrar.
- In `get_deliverability`, read `available` first. When it is false the zeros
  mean "not measured yet", not "no replies". `opens` is not tracked and
  `health` is not graded yet; do not report either as a finding.

**Ordering charges a card.** Follow this every time, with no shortcuts:

1. `list_catalogue` to see what this account can order. A plan the account is
   not on is not listed; it is bought the first time through a checkout a
   person pays in the app, not by Claude. `check_payment_method` before
   placing anything.
2. `preview_order` with the exact order. It runs every check placing runs and
   creates nothing.
3. **Show the person the preview**: product, domains, inboxes, the recurring
   `total` per interval, the `first_invoice`, any discount and how long it
   lasts (`discount.applies_to`), every item in `problems`, and the `note`.
4. **Place only on an explicit yes to that preview, in this conversation.**
   "Sounds good, go ahead" after the numbers counts; a plan discussed earlier,
   or a yes to a different order, does not. If anything in the order changes,
   preview again.
5. `place_order` with the same order and an `idempotency_key` you choose once
   and keep (8 to 64 characters: letters, digits, `.` `-` `:` `_`). If the call
   times out or fails with 502, **retry with the same key**: you get the
   original order back with `replayed: true`, never a second charge. A new key
   is a new order.
6. Report the `subscription_id`, what was charged, and that a person at
   WarmInboxes checks every order before it is built, so it starts as
   `new_order`.

Never put a price in an order body (it is refused), and never invent a sku:
take it from `list_catalogue`. Twelve orders an hour per account is the limit,
counted whether or not the card was accepted.

**Exporting** a built order to a sequencer (`export_to_sequencer`) uses a
login the person saved beforehand. If the answer says a login, key or
workspace is missing, tell them where to add it (the app, or
`PUT /v1/portal/sequencer-keys`) and stop; never ask for the password. An
inbox exports to the same sequencer once a day, so fix the cause before
retrying. Follow progress with `list_exports` every 30 to 60 seconds.

**What a key cannot do** (refused with 403, and pointless to retry): manage API
keys, cancel an order or undo a cancellation, change the team, the referral
set-up or the account email. Those stay with a person in the app. A program can
*ask* for a cancellation with a change request of kind `cancel`, which a
person then handles.

**A refused call** comes back as a normal result with `isError: true` and one
sentence written for a person. Read it and pass it on; do not retry as if the
connection dropped. 4xx answers fail the same way when retried unchanged;
429, 502 and 503 are worth retrying after the pause they name.

## Rules

- **Never handle a secret.** No API keys, sequencer passwords or registrar
  passwords in the conversation, in commands you run, or in files you print.
- **Preview, show, explicit yes, then place.** Every order, every time.
- **Say which key you are on.** Test-key answers are the sandbox; never report
  them as the real account.
- **Report what the API returned**, in its terms, and nothing it did not
  return. No build dates it did not give: a person checks each order first and
  timing depends on the order.
- **The docs win.** For anything not here, fetch
  https://warminboxes.com/transactional-api.md or point to
  https://docs.warminboxes.com.

## Close

End with the one next step: make the key, run the connect command, point the
nameservers, confirm the preview, or nothing, if the question was answered.
