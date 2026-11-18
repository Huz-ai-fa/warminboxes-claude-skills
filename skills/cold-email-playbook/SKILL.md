---
name: cold-email-playbook
description: Work any open-ended cold email problem the way WarmInboxes' operators do - reply rate dropped, landing in Gmail spam or Outlook junk, listed on a blocklist, bounces rising, is my domain burned, setting up or scaling - using the diagnostic playbook, the operator notes and WarmInboxes' published studies. Use when the question is broad ("why did my replies drop?", "what should I do?"), when someone asks for evidence or research, or when another skill's answer needs the reasoning behind it.
---

# Cold email playbook

Version 1.3.0 · Last updated 2026-11-11 · Method: https://warminboxes.com/plugins/playbook

The other skills each run one job. This one decides which jobs to run, in what
order, and what a finding is worth, from WarmInboxes' playbook, its operator
notes and its research. It is how to answer "why did my replies drop?" without
guessing.

## How to run it

With WarmInboxes Free MCP connected:

1. **`get_diagnostic_playbook`** with the symptom in the person's words. It
   matches one of the playbook's scenarios and returns the questions to ask,
   the checks to run cheapest first, what each finding has been measured to
   cost, when new infrastructure is and is not the answer, and the operator
   notes that bear on it (`operatorNotes`). Scenarios: reply rate dropped,
   listed on a blocklist, Outlook or Microsoft 365 junk, Gmail spam or
   Postmaster warnings, bounce rate rising, is my domain or inbox burned,
   running a placement test, setting up new domains, and scaling.
2. **Run the checks it names**, using the other skills: inbox-checker,
   domain-audit, bounce-diagnosis, cold-email-copy, lead-lists.
3. **`find_resources`** for evidence: `type: "note"` lists the operator notes
   (about 90, one per rule, each with its evidence kind and confidence),
   `type: "study"` the published studies, and plain queries search the guides.
   **`read_resource`** reads one in full (`note:<id>` for a note).

Page versions: https://warminboxes.com/plugins/playbook and
https://warminboxes.com/blog/cold-email-deliverability-questions.

## The order of work

From the operator decision tree:

1. **Authentication first:** SPF, DKIM, DMARC, MX.
2. **Then bounces:** a bad list is cleaned; a sender or reputation problem
   gets a full rest, not a slower send.
3. **Then replies, against the person's own baseline.** Ask for it: this
   server cannot read campaign history. Never invent a baseline.
4. **Then the checks,** then a low-volume test at 5 a day, a rest, or a
   replacement, in that order.

## The research

WarmInboxes' published studies, each with a stable ID, data files and a
citation (https://warminboxes.com/research, index at
https://warminboxes.com/research.json):

- **WI-RESEARCH-2026-001:** does a SURBL listing hurt cold email? 2,817
  sending domains tested.
- **WI-RESEARCH-2026-002:** every inbox placement test on prewarmed inboxes, monthly.
- **WI-RESEARCH-2026-003:** Google reputation refusals by top-level domain and sends.
- **WI-RESEARCH-2026-004:** Microsoft's Azure tenant crackdown and the 5.7.237 bounce, across three fleets.
- **WI-RESEARCH-2026-005:** top-level domains ranked by how often they recur on abuse lists.

Quote figures from the study (via `read_resource`), not from memory, and
link it.

## Rules

- **Weigh findings by what they have been measured to cost**, not by how
  alarming they sound. A SURBL listing is common and, in the study, did not
  move reply rate; DMARC `p=none` is a pass.
- **Say what each note is:** dataset, operator rule, operator observation,
  provider requirement or hypothesis, and its confidence. Do not present a
  rule or a hypothesis as measured.
- **Pass on each note's "do not" list** as plainly as its steps, and its
  sources as links the person can open.
- **Conclude with one outcome:** healthy, fixable, unclear, degraded or
  capacity. Only degraded and capacity lead to new infrastructure.

## Close

The outcome, the evidence behind it, and the single next action.
