---
name: no-email-queue-in-this-system
description: "Flow M has no outbound-mail suppression or queue; the spec's \"queued mail count\" is settled as out of scope — stop re-raising it"
metadata:
  node_type: memory
  type: project
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-10-06T02:36:50.450Z
---

There is **no outbound-mail suppression and no email queue** anywhere in
`merchant-agent-system`. The UI/UX spec asks for a held-mail count in two places — step 5a
`ann 4` (the "4,210 queued" copy) and step 5c `ann 25` (clear the queue before re-enabling
customer email) — and nothing implements either. The spec itself marks the mechanics as
unconfirmed with Shopify.

Settled by the user on 2026-10-06: **out of scope, do not build it, do not raise it again.**
Earlier passes kept rediscovering it as a gap in every stage-5 audit.

**Why:** absence is invisible to grep, so each fresh audit of 5a/5c finds the annotation,
finds no field behind it, and reports it as new. It is not new and it is not wanted.

**How to apply:** when auditing stage 5 or the readiness/cutover schemas, treat the missing
queued-mail count as a closed decision rather than a finding. The agent-side "pass each
mutation's own suppress field" rule is a different thing and is live — see
[[merchant-words-never-reach-a-rerun]] for the other settled-but-unbuilt item.
