---
name: ship
description: >
  Use when the user wants to ship their work — branch, commit, push, and
  open a pull request in one command. Triggers: "ship", "ship it",
  "open a PR", "publish this", or when implementation is complete and
  ready for review.
---

Branch, commit, push, and open a pull request in one flow.

## Flow

### 1. Branch Detection

Detect the default branch and current branch:

- `git symbolic-ref --quiet refs/remotes/origin/HEAD | sed 's#.*/##'` → default branch name
  - Fallback: if that fails, run `git remote set-head origin -a` first, then retry
  - Last resort: assume `main`
- `git branch --show-current` → current branch

If current branch **IS** the default branch:

- Derive a branch name from the uncommitted/staged changes:
  - Analyze `git diff --stat` and `git diff --cached --stat`
  - Pick the dominant conventional commit type (feat, fix, refactor, etc.)
  - Extract a short scope from the most-changed directory or module
  - Format: `type/short-kebab-description` (e.g., `feat/add-palette-heuristics`)
  - Max ~50 chars, lowercase, kebab-case
- Create: `git checkout -b <branch-name>`

If current branch is **NOT** the default branch:

- Skip — use the existing branch as-is

### 2. Commit

Check for uncommitted changes: `git status --porcelain`

If changes exist (staged or unstaged):

- Invoke the **commit-it** skill to handle committing
- commit-it will: inspect, group by concern, plan, stage (whole-file or hunk-level), verify, commit
- Wait for commit-it to complete all its commits

If no uncommitted changes:

- Skip — existing commits on the branch are sufficient

### 3. Validate

Count commits ahead of default branch:

- `git log <default-branch>..<current-branch> --oneline | wc -l`

If 0 commits ahead:

- Abort with message: "Nothing to ship — no commits ahead of `<default-branch>`"

If 1+ commits ahead:

- Continue to push

### 4. Push

- `git push -u origin <current-branch>`
- If the branch already has an upstream and is up to date, skip

### 5. Open Pull Request

Analyze all commits on the branch:

- `git log <default-branch>..<current-branch> --format="%s%n%b---"`

**PR Title** — follows the same 50/72 rule as commit subjects:

- Single commit: use its subject line as the PR title
- Multiple commits: synthesize a summary title
  - Find the dominant type across commits
  - Derive scope from the most common scope or the top-level module
  - Write a description that captures the overall intent
  - Format: `type(scope): description`
- **Aim for ≤50 chars, hard limit 72 chars**
- Lowercase, imperative mood, no trailing period

**PR Body**:

```
## Summary
- concise bullet per logical change (not per commit)
- focus on WHAT changed and WHY

## Test plan (only when relevant)
- [ ] steps to verify the changes
```

- Include Test plan only when changes involve UI, complex logic, or behavior that benefits from explicit verification steps
- Skip Test plan for docs-only, config, refactoring, or simple changes

Create: `gh pr create --title "..." --body "..."`

Output the PR URL.

## Rules

- ALWAYS detect the default branch dynamically — never hardcode `main`
- ALWAYS delegate committing to the commit-it skill — never commit directly
- NEVER modify working-tree content — ship only branches, commits (via
  commit-it), pushes, and opens a PR. The working tree is the source of truth:
  never recover absent files from history (see commit-it's Fidelity section). If
  content looks surprisingly missing, stop and ask
- ALWAYS follow the 50/72 rule for PR titles
- ALWAYS push with `-u` to set upstream tracking
- NEVER force-push
- NEVER create a PR if there are 0 commits ahead of default
- NEVER skip the commit-it step when uncommitted changes exist
- Output the PR URL as the final action
