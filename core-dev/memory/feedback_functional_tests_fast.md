---
name: functional-tests-fast
description: "Functional tests here install profiles in seconds; a probe test installing Standard and Umami with ~150 requests took ~30s, so run such tests directly instead of handing them over"
metadata:
  type: feedback
---
<!-- #ddev-generated -->

In this ddev setup, BrowserTestBase tests install profiles in seconds (Standard ~2 s, demo_umami ~8 s). A test that installed both profiles and made ~150 requests finished in ~30 s in total.

**Why:** I estimated 5–10 minutes for that run and, following the long-running-commands convention, handed it to the user as a script. The user pointed out it took well under a minute (2026-11-12).

**How to apply:** Treat a single Kernel or Functional test run here, even one installing standard or demo_umami, as a short check: run it yourself with `ddev phpunit`. Hand over only genuinely long work, such as loops of many runs or multi-minute benchmarks. See [[feedback-phpunit-via-ddev]].
