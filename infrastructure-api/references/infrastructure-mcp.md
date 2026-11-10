# WarmInboxes Infrastructure MCP: quick reference

Condensed from the WarmInboxes API reference (https://docs.warminboxes.com,
plain text at https://warminboxes.com/transactional-api.md, published
2026-10-15). The docs are authoritative; fetch them for request bodies, the
checkout format and any route not listed here.

## Surfaces

| | |
|---|---|
| MCP | `https://api.warminboxes.com/mcp`, stateless Streamable HTTP, key as bearer token. Protocol versions 2025-11-25, 2025-06-18, 2025-03-26. One JSON-RPC message per request; batches refused. |
| REST | `https://api.warminboxes.com/v1/…`, JSON in and out, `Authorization: Bearer wib_…` |
| CLI | `curl -fsSL https://api.warminboxes.com/v1/cli -o wib && chmod +x wib`, key in `WIB_API_KEY` |
| App | https://app.warminboxes.com (keys, cancellations, team, first orders) |

## Scopes

| Scope | Allows |
|---|---|
| `read` | every GET, and order preview |
| `write` | every change under `/v1/portal/…`: folders, names, sequencer choice, monitors, forwarding, renames, requests, exports |
| `orders:place` | placing orders (charges the card), checkouts, card set-up link |

A tool the key's scopes do not allow is not shown to the assistant at all.

## Tools

| Tool | Scope | Does |
|---|---|---|
| `get_dashboard` | read | every order with its domains and build state, plus totals |
| `list_orders` | read | flat list of orders, newest first, cancelled included |
| `get_billing` | read | invoices, spend, what is due next |
| `get_deliverability` | read | sending figures per domain; `period` is `7d`, `30d` or `90d` |
| `list_catalogue` | read | what this account can order, at its price |
| `check_payment_method` | read | whether a card is saved |
| `preview_order` | read | checks and prices an order; creates nothing |
| `list_sequencer_keys` | read | saved sequencer logins, never the secrets |
| `list_exports` | read | how each export of an order went; `subscription_id` |
| `get_nameservers` | read | the pair each domain should point at, and whether it does; `subscription_id` |
| `export_to_sequencer` | write | push a built order into Instantly, Smartlead, PlusVibe or Email Bison; `subscription_id`, `sequencer`, optional `workspace`, `key_id` or `label` |
| `create_card_setup_link` | orders:place | a Stripe page where a person saves a card; `success_url`, `cancel_url` |
| `place_order` | orders:place | place the order and charge the card; the order plus `idempotency_key` |

There is no checkout tool (a checkout ends on a payment page a person must
open) and no tool that saves a sequencer login (it would put the password in
the conversation).

## Orders, by how the plan is ordered

The catalogue's `orders_by` says which shape an order takes.

- `domains` (Google, Microsoft 365): `{"sku", "domains": [{"domain", "forward_to"?, "mailboxes": [{"first_name", "last_name", "local_part"}]}]}`. Up to 500 domains and 2,000 inboxes. The first inbox on a domain is its admin.
- `tenants` (Azure): `{"sku", "domains": [...], "inboxes_per_domain": 25 | 50 | 99, "usernames": [exactly that many], "domain_access": {"method": "nameservers"}}`. One tenant per domain, 1 to 99.
- `quantity` (prewarmed): `{"sku", "quantity": 1-200}` and no `domains`; WarmInboxes assigns the prewarmed domains.

Optional on most: `sequencer`, `forward_to`, `workspace_id`, `name`, `notes`,
`promotion_code`; Azure and Microsoft 365 also take `buy_domains` and
`domain_access`. A price field in the body is refused.

## Order statuses

`pending` (checkout open) · `expired` (never paid) · `new_order` (paid, waiting
for the team) · `update_requested` · `build_in_process` ·
`uploading_in_process` · `setup_completed` · `live` · `cancelling` (ends at
period close) · `cancelled` · `payment_failed` (outranks the rest).

## Domain statuses

`setting_up` · `waiting_on_you` (point the nameservers at the pair given) ·
`live` · `failed` (`detail` may say why).

## Errors

Every error is `{"detail": "<one sentence for a person>"}`; ordering refusals
add `problems`.

| Code | Means |
|---|---|
| 400 | refused as sent; `detail` says what to change |
| 401 | key missing, not recognised or revoked |
| 402 | the card was not charged (declined, or needed the cardholder) |
| 403 | the key lacks the scope, or only a person in the app may do this |
| 404 | no such record on this account |
| 409 | not possible in the current state (no card, price changed, nothing built); nothing changed |
| 413 | body over 4 MB |
| 422 | a field missing or the wrong shape (return URL, sequencer login, workspace) |
| 429 | too many requests; wait `Retry-After` seconds |
| 500 | broke on WarmInboxes' side; logged |
| 502 | Stripe or a sequencer refused or was unreachable; retry orders with the same idempotency key |
| 503 | sign-in checks briefly unavailable; retry |

## Rate limits

- 240 a minute per key for reads and every MCP message.
- 60 a minute per key for changes.
- 12 an hour per account for orders and checkouts together, across API, MCP and CLI.
- 900 a minute per network address for everything.

## Sandbox (test keys)

A `wib_test_…` key answers from a pretend copy of the account: nothing is
charged, built, sent or changed, and answers carry `X-Sandbox: true`. All of an
account's test keys share one sandbox.

- Starts with five orders: `sub_test_acme_outbound` (finished Google),
  `sub_test_globex_prewarmed`, `sub_test_initech_build` (Microsoft 365, one
  domain `waiting_on_you`), `sub_test_hooli_azure` (finished Azure),
  `sub_test_umbrella_ended` (cancelled). A test card ending 4242 is saved.
- Time moves: a placed order is `new_order` for a minute, building from minute
  1, uploading from minute 3, `setup_completed` from minute 5. Exports finish
  after a minute.
- Promotion codes `SANDBOX10` and `SANDBOX100` work on the first invoice.
- Domains ending `-taken.com` belong to another account; for Azure and
  Microsoft 365, `-unregistered.com`, `-in-tenant.com`, `-not-pointed.com`,
  `-no-nameservers.com` and `-unknown.com` each trigger their named case.
- REST and CLI can reset it (`POST /v1/sandbox/reset`, `wib sandbox reset`) and
  force a status (`POST /v1/sandbox/orders/{id}/status`). The
  `X-Sandbox-Simulate` header (`rate_limited`, `server_error`,
  `card_declined`, `no_card`, `sequencer_down`) rehearses failures.

## Not yet

No webhooks: poll. No pagination: lists come back whole.
