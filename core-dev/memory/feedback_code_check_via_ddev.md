---
name: feedback-code-check-via-ddev
description: "Run core/scripts/dev/commit-code-check.sh (phpcs, phpstan, cspell, eslint) via the ddev code-check command, not directly on the host"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 3d4be8fc-2fe1-467d-acbe-c8dfbec543c6
  modified: 2026-09-09T08:35:04.971Z
---
<!-- #ddev-generated -->

Run `ddev code-check [flags]` (e.g. `ddev code-check --branch main`), never
`./core/scripts/dev/commit-code-check.sh` directly on the host.

**Why:** `yarn`/`node`/`cspell`/`eslint` only exist inside the ddev web
container (Node 24) — the host has no `yarn` at all, so CSpell/ESLint fail
outright on the host with "yarn: command not found". Separately, the host's
PHP CLI has a capped `memory_limit` (~128M), which crashes PHPStan on
anything but tiny diffs; the container's PHP CLI has `memory_limit = -1`
(unlimited) by default, so this is a non-issue once run in-container —
no need for `--memory-unlimited` when going through ddev. Same root cause as
[[feedback_phpunit_via_ddev]]: core/AGENTS.md's literal command works, but
only inside the ddev container, not on the host.

**How to apply:** A `ddev code-check` wrapper was added at
`.ddev/commands/web/code-check` — a thin passthrough to
`./core/scripts/dev/commit-code-check.sh "$@"` run inside the container (no
`#ddev-generated` marker: this is a hand-authored project command, not
ddev/addon-scaffolded — that marker is reserved for files ddev itself
generates and may overwrite). Prefer `ddev code-check --branch main` (or
`--cached`, etc. — any flag `commit-code-check.sh` supports) over reaching
for `ddev exec` + the raw script path.
