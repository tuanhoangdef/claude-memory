---
name: project-intro-format
description: "When introducing a repo/project to study, always include a code-reading path and explain every underlying service/technology (e.g. Redis, Lua) for a learner"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 7c0d70fd-1801-42c8-bd74-50e89800919c
  modified: 2026-10-19T14:07:50.438Z
---

When introducing a project for the user to study (e.g. repos in data-engineering/), always include:
1. **A code-reading path**: where to start, which files in what order, what to look for in each, and which tests to read alongside.
2. **An explanation of every service/technology the project relies on or mentions** (e.g. Redis, Lua, Kafka), even when the project only references it and does not run it: what it is, an analogy, its core concepts, how it fits this project, and how to try it.

**Why:** the user pushed back on api-rate-limit-orchestrator: "bổ sung thêm luồng bắt đầu đọc code từ đâu nên đi như nào. Không thấy giới thiệu các service như Redis hay Lua? Tôi đang học cơ mà". They are learning, not just running demos.

**How to apply:** use this structure for each new project intro: problem → analogy → background tech intro → components → reading path → commands (the user runs them, see [[guide-dont-execute]]) → exercises → gotchas.
