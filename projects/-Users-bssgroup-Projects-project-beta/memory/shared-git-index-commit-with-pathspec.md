---
name: shared-git-index-commit-with-pathspec
description: "Concurrent sessions share one working tree AND one git index — always `git commit -- <your paths>`, never a bare commit"
metadata: 
  node_type: memory
  type: project
  originSessionId: 3faf2478-8588-443f-afd1-62d5e62b71b1
  modified: 2026-09-21T08:53:38.822Z
---

Several Claude sessions work this checkout at once. One working tree means one `.git`,
which means **one staging area between all of them**. A bare `git commit` commits whatever
is in that index, including files another session staged.

This is not hypothetical: on 2026-09-21 a peer session found 13 of this session's files
already staged when it went to commit (a `git mv` stages its results), and this session had
earlier swept a peer's uncommitted `slot_reaper.py` into a commit. Both directions, one day.

**Why `git add -A` discipline is not enough:** avoiding a broad `add` stops you sweeping the
*working tree*. It does nothing about what is already in the shared *index* when you commit.

**How to apply:**

- `git commit -- <your paths>` always. With a pathspec, git commits those paths from the
  working tree and leaves everything else in the index untouched — a foreign staged file
  cannot reach your commit even if you fail to notice it.
- Stage explicit paths, never `git add -A` / `git add .` at the repo root.
- Before committing, ask the **inverted** question: "is every staged path on my list?"
  (`git diff --cached --name-only`). "Is anything outside my scope staged?" fails open —
  a path in neither person's list passes it.
- Commit by the same glob you staged with. Never retype the list of files a glob matches;
  that is where a name that breaks the pattern gets dropped.
- New untracked files still need `git add` first, so keep that window short.

**The one case where the pathspec is wrong, and it fails silently.** `git commit -- <path>`
commits that path **from the working tree**, not from the index. So when the staged change
is a *removal of a file that still exists on disk* — `git rm --cached CLAUDE.md`, the whole
point of which is to keep the file — the pathspec form re-reads it from disk and **commits
the file back**, throwing the staged deletion away. It reports success, and `git status`
afterwards looks right because nothing is left staged. This happened on 2026-09-23: a
commit meant to untrack a file instead committed 44 new lines of it.

For an untracking commit, run `git rm --cached <path>`, check `git diff --cached
--name-status` shows **only** your one `D` line, then `git commit` with **no** pathspec.
The inverted check is what makes the bare commit safe here; it is not a licence to drop the
pathspec anywhere else.

Related: [[pytest-db-name-collides-across-sessions]] and
[[check-sh-other-teams-scratch-toggle]] are the same shared-tree hazard on the database and
the tooling side.
