---
name: keep-test-runs-narrow
description: "User stops full-suite pytest runs as wasted time — run only the touched tests, then verify live in the console"
metadata:
  node_type: memory
  type: feedback
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-09-30T10:31:08.141Z
---

Do not run whole-domain pytest sweeps (`pytest src/merchant_ops/migration src/merchant_ops/admin`) as a routine gate after every edit. Run only the test file or test id that covers the change, then rebuild and check the real screen.

**Why:** each containerised run costs 20+ seconds plus container startup, and the user is iterating on console UI where the browser is the faster and more convincing check. They interrupted a full run to say so.

**How to apply:** scoped run for the thing just changed; save the wide sweep for before a commit that touches shared code, or when asked. For console work, `docker compose build app && docker compose --profile dev up -d app worker` plus a Playwright look is the verification that matters — see [[verify-console-changes-in-the-browser]].
