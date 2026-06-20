---
name: waifu-it
description: >
  Use when the user wants to upload a local file and get a shareable link —
  triggers include "upload this", "share this file", "waifuvault", "get a link
  for this", or after producing a file (artifact, export, log) the user wants to
  hand off. Uploads via WaifuVault and returns the URL.
---

Upload a local file to WaifuVault and return a shareable URL.

This skill is self-contained: it bundles its own uploader at `scripts/waifu-it`
(relative to this skill's directory). The script wraps the WaifuVault REST API
with error handling and URL extraction, so you never reconstruct `curl` by
hand. It needs `curl` and `jq` on `PATH` (the script checks and reports if
either is missing).

## Flow

1. **Confirm intent.** NEVER upload without explicit user approval — uploading
   publishes the file to a public URL anyone with the link can fetch. If the
   user hasn't already asked to upload, ask first.
2. **Run** the bundled script with `bash` (don't rely on the exec bit, which an
   install may strip), passing the **absolute** file path. Resolve the path
   relative to this skill's base directory — e.g. with `$CLAUDE_PLUGIN_ROOT`
   when set, otherwise the directory shown when the skill loaded:
   ```bash
   bash "<skill-dir>/scripts/waifu-it" /abs/path/to/file
   ```
3. **On success** it prints a single line on stdout — the URL. Show that clean
   link to the user.
4. **On failure** (no `.url` in the response, or `curl` errored) it prints
   `Upload failed.` plus the raw response on stderr and returns non-zero.
   Surface the error and remind the user the file is still available locally.

## Options

Pass these before the file path. Map the user's intent to the right flag —
don't set any unless they ask.

| Flag | Effect |
|---|---|
| `-e, --expires <dur>` | Retention as `<number><unit>` — `m`/`h`/`d` (e.g. `30m`, `1h`, `1d`). Omit for the server's default retention policy. |
| `-p, --password <pass>` | Encrypts the file server-side; fetching it later requires the `x-password` header. Use for sensitive files. |
| `-H, --hide-filename` | Keeps the original filename out of the returned URL. |
| `-1, --one-time` | File is deleted the moment it is first accessed. |

```bash
waifu-it -e 1d --one-time /path/to/file      # expires in a day, burn after read
waifu-it -p "s3cret" /path/to/file           # password-encrypted
```

## Rules

- NEVER upload without explicit user approval.
- Pass an absolute path — relative paths break when the shell's cwd differs.
- Show only the final URL on success; don't dump the full JSON response.
- Don't delete or move the local file after upload unless asked.
