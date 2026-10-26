---
name: pytest-db-name-collides-across-sessions
description: A shared mo_test_ database produces phantom failures across sessions - and a REUSED one hides real regressions; pass a fresh DB name per run
metadata: 
  node_type: memory
  type: project
  originSessionId: 3faf2478-8588-443f-afd1-62d5e62b71b1
  modified: 2026-09-21T04:15:23.724Z
---

`scripts/check.sh` derives the test database name from the **current branch**
(`mo_test_$(git branch --show-current | tr -c 'a-z0-9' '_')`). Several Claude sessions
and git worktrees work this repo at once, so two sessions on the same branch get the
same database — and each run drops and recreates it.

The failures this produces look like a regression in whatever you just changed. It cost
the peer session a hunt through reaper wiring for 11 errors in `test_analyze.py` that
did not exist; the same code alone was 655 passed. It is not specific to running two
different suites together — **the same suite twice collides just as hard.**

**Why:** the collision is in the database name, not the test selection, so nothing about
which tests you choose avoids it.

**How to apply:** when running pytest by hand alongside another session, pass your own
`DATABASE_URL`/`TEST_DATABASE_URL` with a session-specific `mo_test_<something>` name
rather than letting `check.sh` derive one. Before blaming a change for a surprising
failure on a shared branch, re-run it alone.

Related: the user's standing rule to namespace E2E data on the shared dev DB/app
(ports 8100/8101) is the same hazard one layer up.

**The same hazard runs the other way, and that direction is worse.** Reusing *your own*
`mo_test_<name>` across many runs leaves rows behind — singletons like
`migration_sandbox_settings` (`id='migration_sandbox_setting'`) survive, and later runs
either collide on the primary key or skip work they think is already done. On 2026-09-28 a
scoped suite reported **1234 passed** four times in a row on a reused database while a real
regression sat in it; a fresh database name showed the failure immediately. A green run on a
dirty database is not evidence.

**How to apply, updated:** use a **new** `mo_test_<something>` name for any run whose green
you intend to rely on, and lowercase — `src/merchant_ops/testing.py:77` rejects a name with a
capital in it (`ValueError: unsafe test database name`), and the resulting 900+ errors look
nothing like the cause. To decide whether a failure is yours, run the same selection from a
throwaway `git worktree` at the previous commit; that is what identified the commit at fault
here, after four green runs had pointed away from it.
