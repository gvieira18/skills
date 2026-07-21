---
name: docs-local
description: >
  Use when the user wants a private, never-committed docs folder in the current
  repo — triggers include "docs-local", "/docs-local", "create a local docs
  folder", "add docs.local", "gitignored docs folder". Creates a `docs.local/`
  directory tracked by git only through its own `.gitignore`, so every file
  dropped inside stays local (personal notes, scratchpads, AI context, drafts).
model: haiku
context: fork
---

Create a `docs.local/` directory whose contents are ignored by git but whose
`.gitignore` is committed — so the folder pattern lives in the repo while every
file you put inside stays local and never gets committed.

## What it produces

A single file, `docs.local/.gitignore`, with exactly:

```gitignore
*
!.gitignore
```

`*` ignores everything in the folder; `!.gitignore` re-includes the ignore file
itself, so the directory is tracked (via that one file) but its contents never
are.

## Non-negotiables

- **Self-ignoring, always.** The folder must always ignore its own contents
  while keeping its `.gitignore` tracked — the `*` + `!.gitignore` pair is the
  whole point and must never be weakened.
- **Writes only inside `docs.local/`.** The one and only file this skill may
  create or modify is `docs.local/.gitignore`. Never touch any file outside the
  folder — in particular, never edit the repo's root `.gitignore` or any other
  ignore file.
- **Never clobber.** If `docs.local/.gitignore` already exists, do NOT overwrite
  it. Read it; if it already ignores everything except itself, report that it's
  already set up and stop. If it differs, show the user the difference and ask
  before changing anything.
- **Never touch existing docs.** Files already inside `docs.local/` are the
  user's local content — never read, move, or delete them.
- **Never commit.** Creating the file is the whole job. Do not run `git add` or
  `git commit` — the point is that this folder stays out of history. If the user
  wants the `.gitignore` committed, they'll ask (or use commit-it / quick-commit).
- **Repo root, unless told otherwise.** Create `docs.local/` at the current
  working directory. If the user names a different location, use that.

## Flow

1. Check whether `docs.local/.gitignore` already exists.
2. **If it exists and already matches** the pattern above: tell the user it's
   already configured and stop.
3. **If it exists but differs:** show the current contents, explain the intended
   pattern, and ask before overwriting.
4. **Otherwise:** create `docs.local/` (if missing) and write `.gitignore` with
   the two lines above.
5. Confirm in one line what was created — e.g. `Created docs.local/.gitignore`.
