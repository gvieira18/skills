---
name: commit-it
description: >
  Use when the user wants to commit changes to git — triggers include "commit",
  "commit this", "save my changes", "commit-it", or when implementation work is
  complete and changes need to be persisted to git history.
---

Commit staged and unstaged changes using Conventional Commits with atomic,
single-concern commits.

## Fidelity — the working tree is the source of truth

A commit records exactly what is in the working tree right now. Your job is to
faithfully persist the current state — never to decide what *should* be there.

- **Absence is intentional.** A deleted, moved, or missing file is a change to
  be committed, not a problem to fix. Stage deletions like any other change.
- **Never reintroduce absent content.** Do not pull files or hunks back from
  git history, other branches, the stash, or dropped/reflog commits during a
  commit flow.
- **Surprise → ask, don't recover.** If something looks surprisingly missing
  (e.g. files you recall creating earlier this session are gone), STOP and ask
  the user. Treat the absence as deliberate until they say otherwise.

## Flow

1. **Inspect**: `git status`, `git diff`, `git diff --cached`
2. **Group**: Analyze ALL changed files, group by logical concern
3. **Print plan**: Show each planned commit (type, scope, message, files) — then proceed
4. **For each group** (in dependency order):
   a. Stage files — whole-file or hunk-level (see Hunk Staging below)
   b. **Verify**: `git diff --cached --stat` — if unexpected files are staged, `git reset` and re-stage
   c. Commit
   d. Show: hash + message + files

## Subject Line

- **Aim for ≤50 chars** — GitHub truncates beyond this in list views
- **Never exceed 72 chars** — hard limit
- Count the FULL string: `type(scope): description` — type and scope eat into the budget
- If over 50: shorten description first, then abbreviate scope, then consider moving detail to body
- Lowercase, imperative mood, no trailing period

## Commit Body

Use a body (blank line after subject) ONLY when:
- Breaking change → `BREAKING CHANGE: <explanation>` footer
- The "why" isn't obvious from subject + diff
- Issue reference → `Closes #123` or `Refs #456`

Body: 1–3 lines max. Wrap at 72 chars. If you need more, the commit isn't atomic enough.

## Atomic Commits

Every commit is a single, self-contained logical change.

- **One concern per commit**: if the message needs "and", split it
- **Self-contained**: repo builds and passes tests at every commit
- **Revertible**: safe to `git revert` without pulling unrelated changes
- **Minimal but complete**: code + tests + docs for that one concern
- **No mixed concerns**: don't bundle formatting with logic, refactor with feature
- **Order matters**: if B depends on A, land A first. Prefer refactor → feature → test

## Hunk Staging

When a single file has changes belonging to multiple concerns:

1. `git diff <file>` — identify all hunks
2. Map each hunk to a concern
3. For each concern's hunks:
   - Extract a filtered patch: keep diff header (`diff --git`, `index`, `---`, `+++`) + selected `@@` blocks
   - Write to temp file: `/tmp/staging-<concern>.patch`
   - Apply: `git apply --cached /tmp/staging-<concern>.patch`
4. Verify: `git diff --cached` shows only the intended changes
5. Commit, then repeat for the next concern

**Fallback**: if hunks are interleaved on adjacent lines and can't be cleanly separated, stage the whole file with the most relevant commit and note it in the commit body.

## Type Reference

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

## Rules

- NEVER reintroduce content absent from the working tree, and NEVER run
  history-recovery commands during a commit flow: `git checkout <ref> -- <path>`,
  `git restore --source=<ref>`, `git revert`, `git reset --hard`,
  `git cherry-pick`, `git stash pop`/`apply`, or `git apply` of a patch sourced
  from history
- NEVER auto-recover surprisingly missing content — stop and ask the user
- NEVER use `git add .` or `git add -A`
- NEVER use `git add -p` or `git add -i` (interactive mode not supported)
- NEVER create a single commit when changes span multiple concerns
- NEVER `git push`
- ALWAYS stage specific files (or hunks) per commit
- ALWAYS verify staging with `git diff --cached --stat` before committing
- ALWAYS print the commit plan before executing
- Include scope when possible: `type(scope): description`
- Breaking changes: `type!:` or `type(scope)!:`
