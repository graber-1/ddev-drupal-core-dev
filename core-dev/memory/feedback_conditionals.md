---
name: Use only boolean expressions in if conditions
description: if conditions must be explicit boolean comparisons, not truthy/falsy values
type: feedback
originSessionId: 0d0ecd46-7e62-4422-825c-7d84852da1b6
---
<!-- #ddev-generated -->
Use only boolean expressions in `if` conditions — no truthy/falsy checks like `if ($account)` or `if ($someArray)`.

**Why:** User explicitly rejected `if ($account)` in favour of `if ($account !== NULL)`.

**How to apply:** Always use explicit comparisons (`=== NULL`, `!== NULL`, `=== 0`, `instanceof`, etc.) in conditional expressions.
