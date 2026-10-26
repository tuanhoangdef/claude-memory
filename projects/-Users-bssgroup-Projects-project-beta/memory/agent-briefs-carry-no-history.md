---
name: agent-briefs-carry-no-history
description: "Anything an in-sandbox agent reads — task brief, its CLAUDE.md, skill files — states only what to do now, never what the codebase used to do"
metadata:
  node_type: memory
  type: feedback
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-09-28T02:03:59.467Z
---

In `merchant-agent-system`, text that reaches an **agent** carries no changelog. No "this
existed until <date>", no "renamed from", no "used to be". Only what to do now.

That covers each stage's `docs/settings-defaults/<stage>/CLAUDE.md`, the `TASK.md` strings
built in `analyze/output_contract.py` / `*_task_brief.py`, and the skill bundles.

**Why:** the user pulled me up on this on 2026-09-28, on a line I had just written — *"A
`cluster_id` parameter existed until 2026-09-28 and is gone"*. An agent has no use for it:
it cannot act on the past, and every sentence it reads costs attention it should be spending
on the store. My reasoning for adding it — that an agent might have seen an older brief —
was wrong anyway, since each run is handed the current brief and nothing else.

**Where history does belong:** Python docstrings and comments, commit messages, and
`docs/handover/flow-m-drift-report.md`. Those have human readers who benefit from knowing
what changed and why. The same sentence is valuable in one place and noise in the other, so
the test is who reads the file, not whether the fact is true.

**How to apply:** before writing into a settings-defaults `CLAUDE.md` or a TASK.md string,
ask whether a competent stranger doing the task today needs the sentence. Dates, removals
and renames almost never survive that. `migration/analyze/tests/test_sandbox_settings.py`
now asserts the analyze brief never contains the word "cluster" at all, which is the shape
this rule takes when it can be enforced.

Related: [[claude-md-stays-out-of-git]] is about the other CLAUDE.md — the repo-root one for
humans, which is the opposite case and *should* record history.
