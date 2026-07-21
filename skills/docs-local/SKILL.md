---
name: docs-local
description: >
  Use when the user wants a private, never-committed docs folder in the current
  repo — triggers include "docs-local", "/docs-local", "create a local docs
  folder", "add docs.local", "gitignored docs folder". Creates a `docs.local/`
  directory whose `.gitignore` blanket-ignores everything (a single `*`), so the
  folder and every file dropped inside stays fully local and never gets
  committed (personal notes, scratchpads, AI context, drafts).
model: haiku
context: fork
---

Create a `docs.local/` directory containing a single `.gitignore` whose entire
content is one line: `*`. This blanket-ignores everything in the folder — the
files you drop inside AND the `.gitignore` itself — so the whole folder stays
local and never enters git history.

## What it produces

Exactly one file, `docs.local/.gitignore`, whose entire content is a single
line:

```gitignore
*
```

Nothing else. No second line, no `!.gitignore` exception, no comments. Just `*`.

`*` ignores every path in the folder, including the `.gitignore` file itself.
The result: `git status` never shows anything under `docs.local/`, and the
folder is invisible to git — purely local.

## Non-negotiables

- **Blanket ignore, single line.** The file content is exactly `*` and nothing
  more. Do NOT add `!.gitignore`, comments, or any other pattern.
- **Writes only inside `docs.local/`.** The one and only file this skill may
  create or modify is `docs.local/.gitignore`. Never touch any file outside the
  folder — in particular, never edit the repo's root `.gitignore` or any other
  ignore file.
- **Never clobber.** If `docs.local/.gitignore` already exists, do NOT overwrite
  it blindly. Read it first (see Flow).
- **Never touch existing docs.** Files already inside `docs.local/` are the
  user's local content — never read, move, or delete them.
- **Never commit.** Creating the file is the whole job. Do not run `git add` or
  `git commit`. If the user wants anything committed, they'll ask.
- **Repo root, unless told otherwise.** Create `docs.local/` at the current
  working directory. If the user names a different location, use that.

## Flow

1. Check whether `docs.local/.gitignore` already exists.
2. **If it exists and its content is already exactly `*`** (ignoring trailing
   blank lines): tell the user it's already configured and stop. Change nothing.
3. **If it exists but its content differs:** show the current content, explain
   the intended content is a single `*` line, and ask before overwriting. Do NOT
   overwrite without explicit approval.
4. **Otherwise (does not exist):** create the `docs.local/` directory if
   missing, then write `docs.local/.gitignore` with exactly one line: `*`.
5. Confirm in one line what was created — e.g. `Created docs.local/.gitignore`.
