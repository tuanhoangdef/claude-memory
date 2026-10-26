---
name: code-flow-from-main
description: "When asked to explain a code flow, trace it in full from the entry point (main / __main__) through every call, with code snippets and line numbers — never a short summary"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 7c0d70fd-1801-42c8-bd74-50e89800919c
  modified: 2026-10-21T15:47:51.115Z
---

When the user asks to "giải thích luồng code" (explain the code flow), trace the execution **from the entry point**: `if __name__ == "__main__"` → `main()` → each line of main in order → into every constructor and function it calls. Show the real code snippet with line numbers at each step, what it creates or changes (objects, queues, tasks, DB rows), and what runs when. For async or threaded code, explain when each task actually starts running and where it yields. Then map the user's log lines onto that trace. Don't stop at a summary table or a "who runs what" overview. Put the full explanation in the chat, not only in the notes file.

**Why:** on multi-pipeline-backpressure-mesh (2026-10-21) the user complained: "sao giải thích ngắn gọn vậy? Đi cụ thể từ hàm main ra đã nhắc bao nhiêu lần rồi main". This had already been asked several times (exactly-once-pipeline: "giải thích kèm phần chỉ trong code", "Cứ in hết phần giải thích ra đây").

**How to apply:** any "luồng code" / "luồng chạy" request in a data-engineering repo. Concepts still come first per [[concepts-before-code]].
