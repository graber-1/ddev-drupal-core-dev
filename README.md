# ddev-drupal-core-dev

This is a DDEV addon for doing Drupal core development.

Forked from [justafish/ddev-drupal-core-dev](https://github.com/justafish/ddev-drupal-core-dev) —
see [Changes from upstream](#changes-from-upstream) below for what's different and why.

## Installation
```
git clone https://git.drupalcode.org/project/drupal.git drupal
cd drupal
ddev config --omit-containers=db --disable-settings-management
ddev composer install
ddev add-on get graber-1/ddev-drupal-core-dev

# See included commands
ddev dr list
```

## Examples
```
# Drupal core's own CLI (cache:rebuild, site:install, quick-start, theme:generate, server, ...)
ddev dr list
ddev dr cache:rebuild
ddev dr site:install standard

# Run PHPUnit tests
ddev phpunit core/modules/announcements_feed

# Run Nightwatch tests (currently only runs on Chrome)
ddev nightwatch --tag core

# Run core's code quality checks (phpcs, phpstan, cspell, eslint) against changed files
ddev code-check --branch main

# Manage contrib modules for development without touching composer by hand
ddev add-module token
ddev update-module token
ddev remove-module token
```

## Nightwatch Examples

You can watch Nightwatch running in real time at https://drupal.ddev.site:7900
for Chrome and https://drupal.ddev.site:7901 for Firefox. The password is
"secret". YMMV using Firefox as core tests don't currently run on it.

Only core tests
```
ddev nightwatch --tag core
```

Skip running core tests
```
ddev nightwatch --skiptags core
```

Run a single test
```
ddev nightwatch tests/Drupal/Nightwatch/Tests/a11yTestAdmin.js
```

a11y tests for both the admin and default themes
```
ddev nightwatch --tag a11y
```

a11y tests for the admin theme only
```
ddev nightwatch --tag a11y:admin
```

a11y tests for the default theme only
```
ddev nightwatch --tag a11y:default
```

a11y test for a custom theme used as the default theme
```
ddev nightwatch --tag a11y:default --defaultTheme bartik
```

a11y test for a custom admin theme
```
ddev nightwatch --tag a11y:admin --adminTheme seven
```

## Code quality checks

`ddev code-check` wraps core's own `core/scripts/dev/commit-code-check.sh` (phpcs, phpstan,
cspell, eslint), scoped to changed files for speed:

```
ddev code-check                 # files changed in the working tree
ddev code-check --branch main   # everything changed vs. main
ddev code-check --cached        # staged files
```

## Managing contrib modules

`ddev add-module`/`ddev update-module`/`ddev remove-module` clone/require/remove a contrib module
as a Composer path repository, so you can develop against a real git checkout without drush:

```
ddev add-module token              # auto-detects the module's default branch
ddev add-module token 2.0.x        # or pin a branch
ddev add-module --https token      # clone over HTTPS instead of SSH

# after switching the module's own git branch:
ddev update-module token

ddev remove-module token
```

## Changes from upstream

This fork drops the custom PHP Symfony-console command layer (the old `drupal` command and its
`Lint*`/`Test*`/`Cache`/`Uninstall`/`ModuleInstall`/`AdminLogin` subcommands), because:

- Drupal core now ships its own CLI (`vendor/bin/dr`, `Drupal\Core\Command\DrupalApplication`)
  with `cache:rebuild` (aliases `cr`/`rebuild`), `site:install`, `quick-start`, `theme:generate`,
  and `server` built in — no addon code needed. `ddev dr` is a one-line passthrough to it.
- The `Lint*` commands are superseded by `ddev code-check`, which runs the same tools
  incrementally against changed files instead of the whole codebase.
- `TestCommand` and `TestBrowserCommand` are dropped: the former's own description said "you
  probably don't want to do this — instead run PHPUnit directly," and the latter depended on
  `.ddev/phpunit-{chrome,firefox}.xml` templates that never existed in this repo either.
- `ModuleInstallCommand`/`AdminLoginCommand`/`UninstallCommand`/`TestExtensionsCommand` filled
  real gaps (drush-free module install/login/reset), but weren't in active use — dropped for now.
  If you need one of these again, `ddev exec dr <command>` plus a small custom `.services.yml`
  tagged `console.command` is the more idiomatic way to add it today (core auto-discovers
  module-provided console commands), rather than a parallel Symfony `Application`.

Also added `ddev add-module`/`ddev update-module`/`ddev remove-module` (contrib module management
without drush) and `ddev code-check`, both carried over from local project tooling that predated
this fork.

**Known limitation carried over as-is:** `web-build/Dockerfile.chromium` pins Chromium via a
dated Debian snapshot repository — it will need bumping periodically as that snapshot ages out.
