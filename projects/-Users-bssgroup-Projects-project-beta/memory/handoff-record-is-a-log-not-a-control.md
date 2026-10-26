---
name: handoff-record-is-a-log-not-a-control
description: MigrationHandOffRecord is only a log — item state and MerchantTask drive the work; never gate an action on a record being open
metadata:
  node_type: memory
  type: feedback
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-09-30T11:27:07.939Z
---

`MigrationHandOffRecord` records that a hand-off happened. It is **not** the work and must not be the thing the system steers by. The work lives on the `MerchantTask`; the item's own `processing_state` is its state.

**Why:** the user has said this more than once, and the code still contradicts it — `create_hand_off_for_item` is what sets `processing_state="held"`, `mark_item_not_migratable_by_operator` takes a `hand_off_id` and refuses without one, and the console builds an item's action URLs only for items carrying an **open** record. The consequence is real: an item whose records were all resolved while it stayed `held` has no action left but Retry, and `held` is what the 5a gate reads as "not processed", so the migration cannot go live.

**How to apply:** an operator action on an item takes the item, not a `hand_off_id`. Resolving whatever record is open is a side effect of doing the work, written after, never the precondition. When a screen decides which controls to render, branch on the item's state — offering a control only where a log row happens to be open is the bug, not the guard.
