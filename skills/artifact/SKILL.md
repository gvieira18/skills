---
name: artifact
description: >
  Generate self-contained HTML artifacts for visual communication — code flows,
  architecture diagrams, data dashboards, explainers, presentations, comparisons.
  Use when user asks to create a visual artifact, HTML explainer, visual summary,
  or says "make an artifact" / "visualize this" / "explain visually".
---

# Artifact

A self-contained HTML file that communicates a topic visually. This skill
orchestrates briefing, narrative articulation, and HTML generation (via
`frontend-design`), then handles preview and optional sharing.

## Workflow

1. **Collect briefing.** If the user's prompt already contains enough context,
   proceed. Otherwise ask (2-3 targeted questions max):
   - What to communicate and to whom
   - What the reader should walk away with
   - Any visual preferences (diagrams, code, comparisons, data)

   Default language: **English**, unless briefing specifies otherwise.

2. **Articulate narrative.** Before any visual work, formulate explicitly:
   - **Thesis** (1 sentence): the one thing the reader must understand
   - **Evidence** (2-3 bullets): what supports the thesis
   - **Effect** (1 sentence): what the reader should feel/do after

   Show the brief to the user. If you can't formulate the thesis, ask for
   more context — don't proceed without clarity.

3. **Prepare the build.**
   - Generate a kebab-case slug from the topic
   - `mkdir -p /tmp/artifacts`
   - Output path: `/tmp/artifacts/<YYYY-MM-DD>-<slug>.html`
   - Read `references/palette.md` for the CSS `:root` block
   - Read `references/art-direction.md` for taxonomy and visual vocabulary

4. **Invoke `frontend-design`.** Use the Skill tool passing this context:
   - The narrative brief from step 2
   - The GUARDRAILS block (below) — pass it verbatim
   - The palette from `references/palette.md` (CSS variables + contrast rules)
   - The art-direction defaults from `references/art-direction.md`
     (taxonomy, typography, tempero) — note these are defaults that
     `frontend-design` may override if the briefing warrants
   - Instruction: "Choose the layout form that best serves this narrative.
     You are not restricted to any template."
   - Instruction: "Save the file to `<path>`"

5. **Deliver.** Print the path as a clickable link:
   `file:///tmp/artifacts/<YYYY-MM-DD>-<slug>.html`
   Never auto-open the browser.

6. **Approval gate.** Ask the user: approved, or "adjust: ...".
   - **"adjust: ..."** → re-invoke `frontend-design` with the delta appended
     to the original context. Overwrite the same file. Loop to step 5.
   - **Approved** → proceed to step 7.

7. **Upload offer.** After approval, ALWAYS ask: "Want me to upload to
   waifuvault for a shareable link?" This is a separate, explicit question —
   never skip it.
   - **Yes** → run the upload command below, show the URL.
   - **No** → done. The file stays at `/tmp/artifacts/...`.

   Upload command:
   ```bash
   curl -s --request PUT 'https://waifuvault.moe/rest' \
     --header 'Content-Type: multipart/form-data' \
     --form "file=@<path>"
   ```
   Extract URL with `jq -r '.url'`. Show the clean link.
   If curl fails or response has no `.url`: show the error and remind the
   user the file is still at `/tmp/artifacts/...` for manual upload.

## GUARDRAILS (pass verbatim to `frontend-design`)

- Single `.html` file. No separate CSS/JS/image files.
- Tailwind via `<script src="https://cdn.tailwindcss.com"></script>`.
- Icons: SVG inline or emoji. Diagrams: HTML/CSS, inline SVG, or Mermaid
  via CDN.
- No relative paths (`./`, `../`) for assets. Fonts: system stack or
  Google Fonts CDN.
- NEVER save inside a git repository — always `/tmp/artifacts/`.

## Rules

- NEVER upload without explicit user approval.
- NEVER save the artifact inside a git repo.
- The artifact is always 1 self-contained `.html` file (guardrails invariant).
- Show narrative brief to user before generating — no silent generation.
