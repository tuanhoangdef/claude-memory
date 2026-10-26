---
name: check-sh-other-teams-scratch-toggle
description: "The uncommitted OTHER_TEAMS edit is a scratch toggle, never committed — it lives in scripts/validate/checks.sh since the 2026-09-23 develop merge, and this user discards it rather than stashing"
metadata:
  node_type: memory
  type: project
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-09-23T09:19:59.311Z
---

**The file moved on 2026-09-23.** `scripts/check.sh` is now a 148-line runner (one run at
a time, detached, logged); the checks themselves are `scripts/validate/checks.sh`, and
`OTHER_TEAMS` is at about line 115 of that. Edit and never-commit **that** file now.

**The list is no longer symmetric.** Before the merge it named both `migration` and
`coding_agent`. develop's version drops `coding_agent`, so `check.sh` now measures the
Coding Agent by default and still excludes `migration` plus the migration-owned `admin/`
files. Only the migration side still needs the toggle. develop also wires the list into
`limits.py` (`LIMITS_ARGS="--exempt $OTHER_TEAMS"`), not just ruff and pytest.

Each team deletes its own entries in the working tree when it wants to be measured, then
leaves the edit uncommitted.

This is a **shared convention, not one team's quirk** — both users gave the same
instruction independently on 2026-09-21. Mine: *"Không commit file đó, mục đích gỡ ra để
validate code thôi."* The migration side's: *"gỡ khỏi other_teams nhưng không được commit
file check.sh."*

**Why it needs writing down:** a tracked file modified for hours with no commit explaining
it looks exactly like an unfinished change, so the reflex is to fold it into the next
commit. It is not unfinished, and a reader who knows only one team's half may still
"tidy" the other's line back in.

**How to apply:** never `git add` `scripts/validate/checks.sh`, and never `git add -A` at the repo
root — several sessions share this one working tree, so a broad stage sweeps up other
people's uncommitted work too (this has already happened in both directions). Stage
explicit paths. If `check.sh fast` comes back RED on your own domain's findings, that is
the toggle working, not a regression.

**It blocks `git pull`, and the user's answer is to throw it away.** `git pull origin
develop` aborts with *"Your local changes to the following files would be overwritten by
merge"*. On 2026-09-23 the user said they would discard the edit rather than stash it — so
**do not offer `git stash`, and do not preserve the diff**; the toggle is cheap to retype
and a stash is one more thing to forget on a shared tree. Discard with
`git checkout -- <that file>`, pull, then re-apply by hand if you still need it.

The migration-side form of the toggle, for retyping: replace the whole `OTHER_TEAMS`
value with just `admin/templates/coding_*`, which measures everything of ours and
still leaves the Coding Agent's console templates out.

Related: [[pytest-db-name-collides-across-sessions]] is the same shared-tree hazard on the
database side, and [[shared-git-index-commit-with-pathspec]] is why the "stage explicit
paths" rule above is not optional.
