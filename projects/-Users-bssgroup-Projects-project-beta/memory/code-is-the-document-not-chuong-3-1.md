---
name: code-is-the-document-not-chuong-3-1
description: "chuong-3-1-migration-agent.md is outdated — never cite it as authority; the code is the document and the user's stated requirement wins"
metadata:
  node_type: memory
  type: feedback
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-09-30T11:26:58.600Z
---

Stop treating `src/merchant_ops/migration/docs/dev_docs/chuong-3-1-migration-agent.md` as the authority for Flow M. The user has ruled it outdated. Read the code for what the system does, and take the user's stated requirement for what it should do.

**Why:** the user said so directly after I quoted 3.1.4.5 preconditions back at them to justify existing behaviour. Citing a stale document to defend code they are asking me to change wastes the turn and argues from the wrong source.

**How to apply:** when a behaviour looks odd, trace it in the code and say what it does, then ask what it should be — do not go looking for a use-case section to settle it. The same caution now applies to `PB-UIUX-Screens-Flows-Spec-v0_85.md` claims about Flow M: useful for merchant-facing wording and screen states, not for deciding whether a code path is right. See [[agent-briefs-carry-no-history]] for the sibling rule about agent-facing docs.
