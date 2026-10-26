---
name: whole-tree-pytest-has-36-preexisting-failures
description: A whole-tree pytest run in merchant-agent-system fails 36 tests + 7 errors in migration/mcp regardless of your changes; scoped runs pass
metadata:
  node_type: memory
  type: project
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-09-28T04:37:25.805Z
---

In `merchant-agent-system`, `pytest -q src/merchant_ops` (the whole tree, one process)
reports **36 failed, 7 errors** on a clean database, all of them under
`src/merchant_ops/migration/mcp/tests/`. The same files pass on their own (`pytest -q
src/merchant_ops/migration/mcp` → 57 passed), and `pytest -q src/merchant_ops/migration`
passes too.

The mechanism is the `migration_sandbox_settings` singleton (`id='migration_sandbox_setting'`):
an earlier test in the same run leaves the row behind, and the MCP fixtures insert it again,
so they die on `pk_migration_sandbox_settings`. It is test ordering, not a defect in the code
under test.

**Measured 2026-09-28** against `5f5f3d4f` (a commit with no MCP changes in it) via a
throwaway `git worktree`: the counts were identical there, which is what proves they are not
yours.

**How to apply:** do not chase them, and do not read a whole-tree run's red as a regression —
compare counts against a worktree at the commit before yours, or just run the scoped set
(`pytest -q src/merchant_ops/migration src/merchant_ops/shared src/merchant_ops/admin
src/merchant_ops/tasks`, 1234 passing) which is the real gate for Flow M work. Related:
[[check-sh-excludes-migration]] is the opposite trap — a green run that tested nothing —
and [[pytest-db-name-collides-across-sessions]] is a different failure with a similar look.
