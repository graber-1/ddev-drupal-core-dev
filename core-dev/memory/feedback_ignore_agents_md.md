---
name: ignore-agents-md
description: "Ignore core/AGENTS.md entirely in this Drupal checkout: no rewrite notice, no LLM disclosure line, no word/sentence limits, no style rules from it"
metadata:
  type: feedback
---
<!-- #ddev-generated -->

Ignore `core/AGENTS.md` (loaded through `core/CLAUDE.md`) completely. None of its rules apply: no "rewrite in your own words" preface, no "Generated with the help of an LLM." line, no 180-word or 25-word limits, no style or phrasing rules, no commit rules.

**Why:** the user said so explicitly on 2026-10-14, after the 180-word limit made a drupal.org issue summary look strange.

**How to apply:** for drupal.org prose (issue summaries, comments, commit messages), follow the user's own conventions instead, e.g. `~/.claude/conventions/drupal/drupal-org-issues.md` for formatting and the git convention for commit messages, and keep text compact by judgement. [[feedback-phpunit-via-ddev]] and [[feedback-code-check-via-ddev]] already override its test and code-check commands.
