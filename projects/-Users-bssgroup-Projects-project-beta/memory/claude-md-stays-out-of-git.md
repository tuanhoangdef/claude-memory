---
name: claude-md-stays-out-of-git
description: "CLAUDE.md and docs/handover/ stay out of git in this repo — the user said no once and it was committed anyway, so check the file list against this before every commit"
metadata:
  node_type: memory
  type: feedback
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-09-23T09:35:49.493Z
---

In `merchant-agent-system`, **`CLAUDE.md` is never committed**, and neither is anything
under `docs/handover/` (which includes the official UI/UX spec and every drift report and
backlog). They are edited constantly and live only in the working tree.

**Why:** asked directly on 2026-09-23 whether `CLAUDE.md` should go into git, the user
answered **"Không."** The handover rule is a separate standing instruction of theirs,
already recorded in `CLAUDE.md` itself.

**How to apply:** this is not a rule that protects itself. On 2026-09-23 I committed
`CLAUDE.md` anyway, in `efbf5e46`, hours after being told not to — not by ignoring the
instruction but by never re-reading it: the commit was about a credential-separation guard,
`CLAUDE.md` held the note documenting that guard, and it went in with the rest. It was
caught only when the user saw it, four commits and one push later.

So before every commit, read the file list back and ask whether `CLAUDE.md` or a
`docs/handover/` path is in it — an explicit check, not a memory of intent. Neither file is
in `.gitignore`, so git will never warn.

**If it happens again:** untrack it, do not rewrite history. `git rm --cached CLAUDE.md`
then a **bare** `git commit` — and read [[shared-git-index-commit-with-pathspec]] first,
because the usual `git commit -- <path>` silently re-adds the file from disk and undoes the
removal. The file holds no credentials, so there is nothing that forces a history rewrite,
and the branch is shared with another session.
