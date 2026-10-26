---
name: vinai-repo-gaps
description: VinAI/ holds 123 VinUni-AI20k repos + 36 self-made EXT- gap labs (all built & verified 2026-10); spec for making more is in _solutions/EXT_LAB_SPEC.md
metadata:
  type: project
---

`VinAI/` = 123 shallow-cloned VinUni-AI20k repos sorted into `NN-category/` folders, plus 36 extension labs prefixed `EXT-` (created 2026-10-02, all verified: scaffold 0/80, solution 80/80, tests green). `VinAI/README.md` has the catalog ("Repo mở rộng EXT-") and the gap list with ✅ links.

- Solutions live outside the labs in `VinAI/_solutions/EXT-<name>/` (user is a learner — don't spoil them unless asked).
- `VinAI/_solutions/EXT_LAB_SPEC.md` is the build spec (format, Python 3.9 / offline / mock-LLM constraints, verification protocol). Reference lab: `00-foundations/EXT-Lab-Linear-Regression-From-Scratch`.
- Each EXT repo: `git init` only, nothing committed or pushed.

**Why:** the user wanted gap labs in the same format as the originals, distinguishable by prefix.
**How to apply:** for new gap labs, follow the spec and add a row to the README gap list + catalog. Machine has only Python 3.9.6. Related: [[notion-linear-algebra-notes]].
