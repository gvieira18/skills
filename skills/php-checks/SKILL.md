---
name: php-checks
description: >
  Use when working on a PHP or Laravel project and you need to run quality
  checks — triggers include "run checks", "php-checks", "lint", "check my code",
  or before shipping PHP changes. Runs Rector, Pint, PHPStan, and Pest in
  sequence, auto-fixing everything possible.
model: sonnet
context: fork
---

Run Rector, Pint, PHPStan, and Pest in sequence across the entire project.
Auto-fix everything possible, report what remains.

## Flow

### 1. Detect Available Tools

Read `composer.json` (both `require` and `require-dev`) to determine which tools are installed:

| Package | Tool |
|---------|------|
| `rector/rector` | Rector |
| `laravel/pint` | Pint |
| `larastan/larastan` or `phpstan/phpstan` | PHPStan |
| `pestphp/pest` | Pest |

- Report which tools were detected and which are missing
- Skip missing tools — do not fail because a tool is absent
- If **none** are found, abort: "No PHP check tools found in composer.json"

### 2. Rector (if available)

Run project-wide in **apply mode** — fix code patterns directly:

```bash
vendor/bin/rector process
```

- Do NOT use `--dry-run` — apply fixes immediately
- Report the number of files changed
- If Rector exits non-zero with an error (not just changes), report the error and continue to the next tool

### 3. Pint (if available)

Run project-wide — fix code style directly:

```bash
vendor/bin/pint --format agent
```

- Do NOT use `--test` or `--dirty` — fix the entire project
- `--format agent` enables structured output for easier parsing
- Report the number of files fixed

### 4. PHPStan (if available)

Run static analysis project-wide:

```bash
vendor/bin/phpstan analyse --no-progress
```

If errors are found:

1. Read each error (file, line, message)
2. Open the file, understand the issue, apply a fix
3. Re-run PHPStan to verify the fix
4. Repeat up to **3 cycles** total

Common fixes:
- Add missing return type declarations
- Add `@property` PHPDoc blocks to Eloquent models for database columns
- Fix type mismatches (wrong parameter type, nullable vs non-nullable)
- Add missing method parameter types

If errors remain after 3 cycles, report them and continue to the next tool.

### 5. Pest (if available)

Run the full test suite:

```bash
php artisan test --compact
```

If failures are found:

1. Read each failure (test name, assertion, expected vs actual)
2. Analyze whether the failure is in the test or the implementation
3. Fix the root cause (prefer fixing implementation over adjusting tests)
4. Re-run the test suite to verify the fix
5. Repeat up to **3 cycles** total

If failures remain after 3 cycles, report them and stop.

### 6. Summary

Report the final status of each tool:

```
Rector:  ✓ passed (3 files fixed)
Pint:    ✓ passed (1 file fixed)
PHPStan: ✓ passed (2 errors fixed)
Pest:    ✓ passed (48 tests, 192 assertions)
```

- Use ✓ for passed, ✗ for failed, ○ for skipped (not installed)
- Include counts: files fixed, errors found/fixed, tests run/passed
- If all checks pass: "All checks passed — ready to ship."
- If any check failed: list the remaining errors

## Agent-Friendly Output

Some Laravel projects use `laravel/pao` (auto-detected via `laravel/agent-detector`)
which forces JSON output from Rector, PHPStan, and Pest automatically. Pint uses
its own `--format agent` flag.

When JSON output is detected, parse it for structured results instead of scraping
text output. See `references/tooling-reference.md` for output format examples.

## Rules

- ALWAYS run in this exact order: Rector → Pint → PHPStan → Pest
- ALWAYS detect tools from `composer.json` — never assume a tool is available
- ALWAYS run project-wide — this is a full quality gate, not a quick lint
- NEVER run Rector with `--dry-run` — apply fixes directly
- NEVER run Pint with `--test` — let it fix in place
- NEVER skip a detected tool unless the user explicitly asks
- NEVER exceed 3 fix-and-retry cycles per tool — report remaining errors instead
- If a tool fails after retries, report the remaining errors and continue to the next tool
- After all tools run, report the complete summary
