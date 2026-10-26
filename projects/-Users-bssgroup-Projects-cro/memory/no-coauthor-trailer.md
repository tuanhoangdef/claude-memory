---
name: no-coauthor-trailer
description: "Commit message của user không kèm dòng Co-Authored-By Claude; format \"CE-xxx - <tóm tắt tiếng Anh ngắn>\""
metadata:
  node_type: memory
  type: feedback
  originSessionId: f1d49c2c-f934-44c4-be9d-49bdb19dd987
  modified: 2026-10-22T08:37:39.743Z
---

Không thêm dòng `Co-Authored-By: Claude …` vào commit message. Format commit user dùng: `CE-<số> - <các thay đổi, tiếng Anh ngắn gọn>`.

**Why:** User yêu cầu bỏ trailer này khi commit CE-123 (22/10/2026).

**How to apply:** Mọi commit trong repo cro-prj: chỉ một dòng tiêu đề theo format trên, không trailer attribution. Khi amend message, dùng `git commit --amend --only` để không kéo theo file user đang stage dở.
