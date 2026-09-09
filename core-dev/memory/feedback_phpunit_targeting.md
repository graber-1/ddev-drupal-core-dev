---
name: PHPUnit test targeting
description: How to run targeted PHPUnit tests instead of the whole module
type: feedback
originSessionId: 1090c3e8-39b1-40fb-8d45-205d3ad1c397
---
When running PHPUnit tests, always target the smallest scope that covers the failure:
1. Run a specific file: `ddev phpunit core/modules/views/tests/src/Unit/Plugin/area/ResultTest.php`
2. Run a specific test method: add `--filter testMethodName`
3. Only run the full module suite when explicitly asked.

**Why:** Running the entire module suite is slow and unnecessary when fixing specific failures.
**How to apply:** After any test failure, use the file path and/or --filter to rerun only the relevant test(s).
