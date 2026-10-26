---
name: stage-5-access-after-transfer-is-unsettled
description: "BSS keeps collaborator access and the app install after a client transfer, so stage 5 order stands — but an unapproved public app cannot install on a client-transfer store"
metadata:
  node_type: memory
  type: project
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-10-07T02:00:49.939Z
---
**Resolved 2026-10-07, on Shopify support's word.** After a completed client transfer BSS
**keeps collaborator access by default** — the store leaves the Partner organization but stays
in the Dev Dashboard as a collaboration — and the migration **public app stays installed with
its OAuth grant intact**, needing no reinstall or re-authorisation. So Flow M's stage order
stands: **5b before 5c, no reordering.** Two earlier re-sequencing designs were built on the
opposite premise and withdrawn.

Caveats worth keeping:

- The same chat first said the opposite (*"the client has to grant it"*) before a human agent
  corrected it. Nobody has yet watched a completed claim end to end, so this is attested, not
  observed.
- The **merchant controls the permission scope** as the new owner. If a 5c operator stage ever
  reports blocked on access, that is the thing to check before suspecting the cutover code.

**Separate blocker found in the same conversation:** an **unapproved public app cannot be
installed on a client-transfer store** while it sits in the Partner org — only free and
partner-friendly apps can. It installs fine on a plain development store, which is why this
went unnoticed. `store_pool/oauth.py` installs exactly that app onto exactly those stores, so
**the pool cannot be filled until Shopify approves the app.** Approval also does not disturb
stores that already have it installed.

Only two of the eight cutover stages (`domain_switch`, `live_traffic`) genuinely need a paid
plan, which is why the claim has to precede go-live at all.
