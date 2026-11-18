---
name: warm-rotation
description: Set up warm rotation for cold email inboxes - split them into two tagged batches on the same campaign, one at full limits and one on follow-ups only, swap on a cycle set by sequence length, and rotate lead lists the same way. Use when someone wants inboxes and domains to last longer, asks how to rotate inboxes, works a saturated niche such as ecommerce, SaaS or healthcare, or asks how often to swap.
---

# Warm rotation

Version 1.3.0 · Last updated 2026-11-11 · Method: https://warminboxes.com/warm-rotation

Half the inboxes carry new outreach while the other half rest on follow-ups,
then they swap. It is the simplest way WarmInboxes has found to keep inboxes
and domains sending for longer, and it takes two tags and a limit change in
the sequencer.

## How to run it

With WarmInboxes Free MCP connected, `plan_rotation` builds it:

```
plan_rotation { "inboxes": 60, "sendsPerDay": 300, "sequenceDays": 12 }
```

It returns the two batches, the swap cycle and why, the steps and list
rotation. `cadenceWeeks` overrides the cycle if the person wants one.
The page version is https://warminboxes.com/inbox-rotation-planner, and the
method in full is https://warminboxes.com/warm-rotation.

## The method

1. Divide the inboxes in half and tag them **Batch 1** and **Batch 2**.
2. Put **both batches on the campaign**. Nothing is added or removed at a swap.
3. The active batch runs at **full daily limits**; the resting batch at
   **follow-ups only**. A resting batch keeps warming and its follow-ups keep
   going out, so nothing it started goes quiet.
4. **Swap** by selecting the other tag in the sequencer's inboxes section and
   raising or lowering limits.

**How often**, by sequence length, so a batch rests for at least one full pass
of the sequence:

| Sequence runs | Swap |
|---|---|
| up to 7 days (about 5) | weekly |
| up to 14 days (about 12) | every 2 weeks |
| 15 days or more | monthly |

**Rotate lead lists the same way:** no list is hit by the same senders twice
in a row.

**Where it matters most:** saturated niches such as ecommerce, SaaS and
healthcare, which sit behind strict filters and whose recipients report spam
more than most.

## What it does, honestly

In WarmInboxes' fleets warm rotation has made inboxes and domains last up to
twice as long as sending from the same ones every day. That is an operator
observation, not a published measurement; the twelve-month study behind it is
being written up. It carries more inboxes for the same volume. It keeps
reputation; it does not guarantee placement, which copy, list and pace still
decide.

## Rules

- **Use `plan_rotation`'s numbers**, not your own arithmetic, and cite it.
- **Say "up to twice as long" as WarmInboxes' observation**, never as a
  measured result.
- **If the fleet is too small to split** into two useful batches, say so,
  and size what the volume needs with the infrastructure-planner skill.

## Close

The two batches (how many inboxes each, limits each), the swap cycle and the
first swap date.
