---
name: check-sh-excludes-migration
description: "scripts/check.sh's OTHER_TEAMS excludes migration/, so a Flow M change gets a green run that tested none of it"
metadata: 
  node_type: memory
  type: project
  originSessionId: afa5a35d-2d16-4c94-bf92-d7487de6f7b3
  modified: 2026-09-21T07:14:32.282Z
---

`scripts/check.sh` has a hardcoded `OTHER_TEAMS` list that it turns into `--ignore-glob`
arguments for pytest **and** `--exclude` for ruff. In this checkout that list starts with
`migration`, plus `admin/migration*`, `admin/tests/test_migration*` and
`admin/templates/migration*`. **So `check.sh full` runs no migration test at all** — it
reports `ok 8 pytest` having skipped every test belonging to Flow M, and `ok 2 ruff` having
read none of its files.

The lint half has compounded: `migration/` has 107 of 252 files failing `ruff format --check`
and 284 `F811`s, while `coding_agent/` (235 files), `shared/`, `tasks/` and `jobs/` are
completely clean. I first wrote this off as a `ruff` version bump in the `tools` image; the
peer session pushed back, and measuring per domain showed the other domains find nothing. An
unmeasured domain, not drift.

The list is written from the coding_agent side: "other teams" means migration. On
2026-09-21 the peer session removed `coding_agent` from it, widening their own coverage
and leaving migration the only excluded domain of the two.

**Why:** the exclusion is invisible in the output. `check.sh` prints a green `ok 8 pytest`
either way; the only place the ignores appear is the container's argv. I launched a full
run to verify an import change across 11 migration files and it would have come back green
without loading one of them.

**How to apply:** as the Flow M agent, `check.sh` is not your gate. Run pytest directly and
name the paths — `pytest -q src/merchant_ops/migration src/merchant_ops/shared
src/merchant_ops/admin` — with your own database per
[[pytest-db-name-collides-across-sessions]]. If you do run `check.sh`, confirm what it
actually executed with `docker inspect -f '{{.Config.Cmd}}' <tools-run container>` rather
than trusting the summary line.
