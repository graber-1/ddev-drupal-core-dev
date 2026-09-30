---
name: Do not use empty() — check the actual return type
description: avoid empty() in favour of explicit type-aware comparisons
type: feedback
originSessionId: 0d0ecd46-7e62-4422-825c-7d84852da1b6
---
<!-- #ddev-generated -->
Do not use `empty()`. Instead, look up what the function actually returns and write an explicit comparison.

**Why:** User rejected `empty($entity->getOwnerId())` and asked to check what `getOwnerId()` returns (it returns `int|null`), leading to `($entity->getOwnerId() ?? 0) === 0`.

**How to apply:** Before writing `empty($x)`, check the declared return type of `$x` and write the corresponding strict comparison.
