# Memory Index

- [PHPUnit test targeting](feedback_phpunit_targeting.md) — target specific file or --filter method, not whole module suite
- [Run PHPUnit via ddev](feedback_phpunit_via_ddev.md) — use `ddev phpunit`, not vendor/bin/phpunit, despite AGENTS.md
- [Run code-check via ddev](feedback_code_check_via_ddev.md) — use `ddev code-check --branch main`, not the raw script on host
- [Use only boolean expressions in if conditions](feedback_conditionals.md) — explicit comparisons only, no truthy/falsy checks
- [Do not use empty()](feedback_no_empty.md) — check the actual return type and write an explicit comparison instead
