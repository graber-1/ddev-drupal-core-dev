---
name: feedback-phpunit-via-ddev
description: "Always invoke PHPUnit through the ddev wrapper, not vendor/bin/phpunit directly, even when a project AGENTS.md says otherwise"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 94da1b8e-8367-40a5-94ff-91a68ab0139d
  modified: 2026-07-23T09:24:16.253Z
---

Run tests with `ddev phpunit [path/filter]`, never `./vendor/bin/phpunit -c core ...` directly on the host.

**Why:** core/AGENTS.md documents `./vendor/bin/phpunit -c core <path>` as the test command, but the user rejected that invocation and confirmed `ddev phpunit` is correct for this project — the app/DB only run correctly inside the ddev container. See also [[feedback_phpunit_targeting]].

**How to apply:** For this Drupal repo (and likely other ddev-based projects), always reach for `ddev phpunit` first. Treat AGENTS.md's raw `vendor/bin/phpunit` instructions as describing the underlying command PHPUnit config, not the literal invocation to run — wrap it with `ddev phpunit`/`ddev exec` as this project's other conventions (phpcs, phpstan) already do.
