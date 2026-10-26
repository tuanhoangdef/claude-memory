---
name: agent-skills-live-in-a-separate-repo
description: "Behaviour changes for the sandbox agents live in two places - the brief in merchant-agent-system and the skill in coding_agent_skills, which the user pushes separately"
metadata:
  node_type: memory
  type: feedback
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-09-29T04:41:14.114Z
---

When changing what a sandbox agent knows or does, update the skill source at
`/Users/bssgroup/Projects/project-beta/coding_agent_skills/skills/<pack>/` as well as the
TASK.md generator in `merchant-agent-system`. It is a separate git repo
(`https://git.bssdev.cloud/tuanha/coding_agent_skills.git`) that the user pushes to remote
themselves.

`merchant-agent-system/data/skill-packs/sources/agent-skills/` is a synced copy, not the
source — editing only that leaves the real repo behind and the change never reaches remote.

**Why:** the brief (TASK.md) and the skills are two halves of what the agent reads. A rule
written into the brief but not the skill is re-learned every run and contradicted by the
skill's own text; the skill is where durable field-by-field mapping belongs.

**How to apply:** after a brief change lands, ask whether the same knowledge belongs in the
skill pack, edit it in `coding_agent_skills`, and tell the user it is ready to push - do not
push it yourself. Related: [[agent-briefs-carry-no-history]].
