---
name: merchant-words-never-reach-a-rerun
description: "A data item held by a build hand-off gives the merchant nowhere to speak that the next run will hear - agreed design, not yet built"
metadata:
  node_type: memory
  type: project
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-09-29T08:03:48.594Z
---

Agreed 2026-09-29, deferred. A `kind=data` item has two doors out of Build, and only one
carries the merchant's words into the next run:

| Door | Item state | Merchant speaks via | Agent hears it |
|---|---|---|---|
| sample passed, merchant says it is wrong | back at Step 3 | `confirm-sample-off` note | yes — `_merchant_feedback_section` |
| sample **failed** | `held` + hand-off | comment on the MerchantTask | **no** |
| full build failed | `held` + hand-off | comment on the MerchantTask | **no** |

**The design that was settled on** (after two wrong turns): read `MerchantTaskEvent` rows —
they already carry `body`, `actor`, `created_at` and `proposes_change` ("a comment that asks
for something else"), and `POST /stores/{id}/tasks/{task_id}/comments` is already open to the
merchant. Find the task through `MigrationHandOffRecord.task_id`, NOT `MigrationItem.task_id`
(that one is the coding task of a `kind=code` item). Filter to events created after the newest
`SampleTestResult.ran_at`, so an answered complaint is not replayed.

Rejected on the way: copying `resolution_note` into `merchant_feedback_note` (conflates an
operator's note with the merchant's, and loses which failure it answers), and a new
`merchant_feedback_at` column (unnecessary once the channel is task events, which are already
timestamped).

**Why:** `MigrationHandOffRecord` is a log — "the task is the row an Operator sees and works
from; this record is the history of it". A log is not where people talk. Without this, a
merchant saying "products with no source image are acceptable, proceed" writes it somewhere
the next run never reads, and that run fails again for the same reason.

**How to apply:** one section in `build/data_task_brief.py`, a matching rule in
`shopify-migration-write`'s SKILL.md saying that merchant permission is the one thing that
widens the completeness rule (skip that field, do not fail the unit), plus tests. No model
change, no alembic, no new UI. Related: [[agent-skills-live-in-a-separate-repo]].
