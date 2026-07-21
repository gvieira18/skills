---
name: quick-commit
description: >
  Use when the user wants to instantly commit ALREADY-STAGED changes with an
  auto-generated Conventional Commit message, with no questions and no plan —
  triggers include "quick-commit", "/quick-commit", "qc", "jc", "just-commit",
  "commit staged", "commit the staged files". Commits only the current index as
  a single commit. For careful, multi-commit, plan-first committing use
  commit-it instead.
model: sonnet
context: fork
---

Commit the **currently staged index** as a single Conventional Commit, with a
message generated from the staged diff. Do it silently, without asking or
assuming anything beyond what is staged.

## Non-negotiables

- **Staged index only.** Commit exactly what `git diff --cached` shows. NEVER
  run `git add`, `git add -A`, `git add .`, `git add -p`, or `git add -i`.
- **Never ask, never suppose.** Do not print a plan, do not request
  confirmation, do not infer intent beyond the staged diff.
- **One commit.** Everything staged goes into a single commit — never split.
- **No checks.** Do not run linters, formatters, or tests. Commit verbatim.
- **Never push.**
- **Never resurrect absent content.** A deleted/missing file in the index is a
  change to commit. Never pull content back from history, branches, stash, or
  reflog; never run history-recovery commands.

## Flow

1. `git diff --cached --stat` — read the staged index.
2. **If nothing is staged: stop. Produce no commit and no output.**
3. `git diff --cached` — read the staged changes.
4. Generate a Conventional Commit message from that diff (see below).
5. `git commit -m "<subject>"` (add `-m "<body>"` only if a body is warranted).
6. **On success:** print exactly one word — `Done` — in the user's language
   (English default; e.g. `Pronto` for Portuguese). Nothing else.
7. **On failure** (hook rejects, git error): print the error briefly so the
   user can fix it. Do not retry or work around it.

## Message rules (self-contained)

- Format: `type(scope): description`. Include scope when the diff makes it
  obvious; otherwise omit it.
- Subject: aim ≤50 chars, hard limit 72, lowercase, imperative, no trailing
  period. Count the full `type(scope): description` string.
- Body: subject-only by default. Add a short body (blank line after subject,
  1–3 lines, wrap 72) ONLY when the "why" is not obvious from subject + diff.
  Use a `BREAKING CHANGE: <explanation>` footer if the diff clearly breaks API.
- Breaking change: `type!:` or `type(scope)!:`.

| Type | Use for |
|------|---------|
| feat | New functionality |
| fix | Bug fix |
| docs | Documentation only |
| style | Formatting, whitespace (no logic change) |
| refactor | Code restructure (no behavior change) |
| perf | Performance improvement |
| test | Adding or correcting tests |
| build | Build system, dependencies |
| ci | CI/CD configuration |
| chore | Maintenance, configs, tooling |
