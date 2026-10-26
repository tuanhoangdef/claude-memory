---
name: concepts-before-code
description: "Explain every new term from zero (what/analogy/concrete example, no code) BEFORE showing how the repo implements it; never use a term before defining it"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 7c0d70fd-1801-42c8-bd74-50e89800919c
  modified: 2026-10-21T04:09:47.780Z
---

When teaching a repo or answering concept questions, explain the terms for a beginner first and only then show the implementation:
1. **Part A, concepts with no code**: order the terms so each one uses only terms already explained. For each term give what it is, an everyday analogy, and a concrete worked example (a timeline table with t1, t2…, or a before/after state). Define the prerequisite terms too, even "basic" ones (e.g. Kafka broker/topic/partition/offset/consumer group before read_committed; DB transaction/isolation before outbox).
2. **Part B, implementation**: a concept → file/line mapping, the flow, and the reading path.
3. Then gotchas/bugs, commands/labs (the user runs them), exercises, links, and the list of questions asked mapped to sections.

Other rules: don't point at repo code to explain a concept; when one word means several things (commit ×3, coordinator ×3), separate them explicitly in a table; when the user states their understanding, say what is right or wrong about it.

**Why:** on exactly-once-pipeline (2026-10-21) the user objected: "giải thích đừng có chỉ vào trong code hiện tại. Bạn phải giải thích cho người mới các thuật ngữ trước, còn việc triển khai như nào thì sau đó… Consumer group là cái gì? quá khó hiểu". They had me delete the notes and rewrite them, and asked me to remember this style.

**How to apply:** use this for every repo intro in data-engineering/ and for its lesson-learned file. The reference example is `data-engineering-lesson-learned/exactly-once-pipeline.md`. This extends [[project-intro-format]]; still follow [[guide-dont-execute]].
