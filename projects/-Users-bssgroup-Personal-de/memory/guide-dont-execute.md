---
name: guide-dont-execute
description: "In the de-zero-to-hero course, explain/instruct only; never run setup, demos or tests on the user's behalf or leave generated files"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 7c0d70fd-1801-42c8-bd74-50e89800919c
  modified: 2026-10-08T01:58:30.531Z
---

User is self-studying the DE Zero to Hero course (de-zero-to-hero/buoi-*) and learns the concepts before touching each demo. Give instructions and explanations; do not run `uv sync`, `make ...`, tests or demos in the project, and do not create .venv, uv.lock, out/, caches, etc.

**Why:** I ran setup + run-sample + tests in buoi-02-python/demo after the user only said they'd installed uv/Python; they were angry — they want to do it themselves and not have junk left behind.

**How to apply:** Read-only inspection (cat, ls, version checks) is fine. If verifying something by running code is truly useful, ask first and do it only in the scratchpad, then clean up. Related: [[user-learning-style]]
