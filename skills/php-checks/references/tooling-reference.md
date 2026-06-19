# Tooling Reference

Technical reference for parsing tool output, understanding exit codes, and
locating configuration files across Laravel projects.

## Pao — Agent-Friendly JSON Output

Projects using `laravel/pao` (auto-activates via `laravel/agent-detector`)
get structured JSON output from Rector, PHPStan, and Pest. Pao hooks into
each binary at startup via Composer autoload, intercepts stdout, and emits
a single JSON line at shutdown.

Pint has its own built-in `--format agent` flag (not managed by Pao).

| Tool | JSON Provider | What Pao Does |
|------|---------------|---------------|
| Rector | `laravel/pao` | Forces `--output-format=json` |
| PHPStan | `laravel/pao` | Forces `--error-format=json` + `--no-progress` |
| Pest | `laravel/pao` | Captures test results via PHPUnit event subscribers |
| Pint | Built-in | Requires `--format agent` flag |

### Output Examples

```json
{"tool":"rector","result":"passed","totals":{"changed_files":0,"errors":0}}
{"tool":"pint","result":"passed"}
{"tool":"phpstan","result":"passed","errors":0}
{"tool":"pest","result":"passed","tests":9,"passed":9,"assertions":38,"duration_ms":14398}
```

When Pao is not installed, parse standard text output instead.

## Exit Codes

All four tools return exit code 0 on success, non-zero on failure.

| Tool | Exit 0 | Exit Non-Zero |
|------|--------|---------------|
| Rector | No changes needed, or changes applied | Errors during processing |
| Pint | No style violations | Style violations found (after fixing) |
| PHPStan | No errors | Type errors found |
| Pest | All tests pass | Test failures or errors |

## Config File Locations

| Tool | Config File | Notes |
|------|-------------|-------|
| Rector | `rector.php` | Rule sets and skip lists |
| Pint | `pint.json` | Preset + custom rules |
| PHPStan | `phpstan.neon` | Level, paths, ignoreErrors |
| Pest | `phpunit.xml` + `Pest.php` | Test config and hooks |

PHPStan config is often hierarchical — a root `phpstan.neon` may auto-include
module-level configs (e.g., `app-modules/*/phpstan.neon`). When running
project-wide, PHPStan loads the root config automatically.

## Pre-Commit Hooks

Some projects use `lint-staged` (via Husky) to run Rector and Pint on staged
`*.php` files at commit time. This means Rector and Pint may run again when
committing — this is expected and harmless (they are idempotent).

lint-staged typically does NOT run PHPStan or Pest. Those must be run manually
via this skill before committing.
