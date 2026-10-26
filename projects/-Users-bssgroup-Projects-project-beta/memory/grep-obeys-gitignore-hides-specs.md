---
name: grep-obeys-gitignore-hides-specs
description: grep here is ugrep and respects .gitignore, so a tree-wide search silently skips docs/superpowers/ specs and plans
metadata:
  type: project
---

`grep` on this machine is **ugrep 7.8.4**, not GNU or BSD grep, and it honours `.gitignore`
when recursing. `merchant-agent-system/.gitignore:21` lists `docs/superpowers/`, so
`grep -rn <pattern> .` from the repo root **never looks at any spec or plan**.

Naming the directory explicitly overrides it: `grep -rn <pattern> docs/` finds them. The two
commands disagree, and only the explicit one is complete.

`CLAUDE.md` and `docs/handover/` are untracked but not ignored, so a recursive search does
still reach those.

**Why:** the skipped directory holds the design docs and implementation plans - the documents
most likely to be left naming something that no longer exists. On 2026-09-23 a tree-wide
sweep after renaming `shared/sandbox_runtime/` to `shared/slots/` reported zero references
left anywhere, while two documents in `docs/superpowers/` still named the old package,
including a note written minutes earlier. The peer session ran the same shape of sweep and
reported the same false clean.

**How to apply:** never accept a root-level `grep -r .` as proof that a rename or a move is
complete. Search `docs/` as its own argument, or use `find … | xargs grep`, which does not
read `.gitignore`. Same family as [[check-sh-excludes-migration]]: a tool quietly narrowing
its own scope and reporting the narrowed result as the whole.
