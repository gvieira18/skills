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

3. **Choose palette.** Present the palette menu to the user using `AskUserQuestion`.
   Read `references/palette.md` for the full catalog. Offer these options:

   | Option | Description |
   |--------|-------------|
   | **Deep Blue + Violeta** | Royal indigo, neon violet/teal — dark |
   | **Zinc + Emerald** | Neutral dark zinc, green/purple — dark |
   | **Community Purple** (default) | Deep purple, pink/teal (he4rt) — dark |
   | **Cyber Terminal** | Navy terminal, neon green/cyan — dark |
   | **Cyberpunk Neon** | Near-black, max neon glows — dark |
   | **Light Paper** | Warm off-white, blue/violet — light |
   | **Light Stone** | Warm taupe, amber/purple jewels — light |
   | **Auto** | Let the AI choose the best palette for this narrative |

   AskUserQuestion only supports 4 options — group them sensibly (e.g.
   "Dark: Community Purple (default)", "Dark: other...", "Light: pick one...",
   "Auto"). If the user picks a group, follow up with the specific choices.

   - If a specific palette is chosen → read it from
     `references/palettes/<file>.md`
   - If **Auto** is chosen → do NOT read any palette file yet. Instead,
     pass the full palette catalog table (from `references/palette.md`) to
     `frontend-design` with this instruction: "Choose the palette that best
     serves this narrative's mood and audience. Read the palette you pick
     from `references/palettes/<file>`. If none fits, invent a custom
     palette using the same CSS variable names."

4. **Prepare the build.**
   - Generate a kebab-case slug from the topic
   - `mkdir -p /tmp/artifacts`
   - Output path: `/tmp/artifacts/<YYYY-MM-DD>-<slug>.html`
   - Read the selected palette file (unless Auto — see step 3)
   - Read `references/art-direction.md` for taxonomy and visual vocabulary

5. **Invoke `frontend-design`.** Use the Skill tool passing this context:
   - The narrative brief from step 2
   - The GUARDRAILS block (below) — pass it verbatim
   - The palette (CSS variables + contrast rules) — either the specific
     palette chosen in step 3, or the full catalog + auto-selection
     instruction
   - The art-direction defaults from `references/art-direction.md`
     (taxonomy, typography, tempero) — note these are defaults that
     `frontend-design` may override if the briefing warrants
   - Instruction: "Choose the layout form that best serves this narrative.
     You are not restricted to any template."
   - Instruction: "Save the file to `<path>`"

6. **Deliver.** Print the path as a clickable link:
   `file:///tmp/artifacts/<YYYY-MM-DD>-<slug>.html`
   Never auto-open the browser.

7. **Approval gate.** Ask the user: approved, or "adjust: ...".
   - **"adjust: ..."** → re-invoke `frontend-design` with the delta appended
     to the original context. Overwrite the same file. Loop to step 6.
   - **Approved** → done. The file stays at `/tmp/artifacts/...`. Do NOT offer
     to upload — the user shares it manually when ready.

## GUARDRAILS (pass verbatim to `frontend-design`)

- Single `.html` file. No separate CSS/JS/image files.
- Tailwind via `<script src="https://cdn.tailwindcss.com"></script>`.
- Icons: SVG inline or emoji. Diagrams: HTML/CSS, inline SVG, or Mermaid
  via CDN.
- No relative paths (`./`, `../`) for assets. Fonts: system stack or
  Google Fonts CDN.
- NEVER save inside a git repository — always `/tmp/artifacts/`.

## Rules

- NEVER save the artifact inside a git repo.
- The artifact is always 1 self-contained `.html` file (guardrails invariant).
- Show narrative brief to user before generating — no silent generation.
