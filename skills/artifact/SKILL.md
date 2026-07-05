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

3. **Choose palette or mode.** Present the menu to the user using `AskUserQuestion`.
   Read `references/palette.md` for the full catalog. Offer the 10 named palettes
   (5 dark, 5 light) plus two smart modes:

   | Option | Description |
   |--------|-------------|
   | **Deep Blue + Violeta** | Royal indigo, neon violet/teal — dark |
   | **Zinc + Emerald** | Neutral dark zinc, green/purple — dark |
   | **Community Purple** (default) | Deep purple, pink/teal (he4rt) — dark |
   | **Cyber Terminal** | Navy terminal, neon green/cyan — dark |
   | **Cyberpunk Neon** | Near-black, max neon glows — dark |
   | **Light Paper** | Warm off-white, blue/violet — light |
   | **Light Stone** | Warm taupe, amber/purple jewels — light |
   | **Arctic Cool Slate** | Cool blue-white, cobalt + glacier-cyan — light |
   | **Sage Botanical** | Green-tinted paper, forest green + clay — light |
   | **Blush Editorial Rose** | Rose-cream, burgundy + antique-gold — light |
   | **Auto** | AI picks the best-*fitting* catalog palette for this narrative |
   | **Wildcard** | Deliberately *ignore* the default; invent a fresh look |

   AskUserQuestion only supports 4 options — group them sensibly (e.g.
   "Dark palette…", "Light palette…", "Auto", "Wildcard") and follow up on a group
   with its specific choices. Note **Community Purple** is the default dark pick.

   Handling per choice:
   - **Specific palette** → read it from `references/palettes/<file>.md`.
   - **Auto** → do NOT read any palette file yet. Pass the full catalog table
     (from `references/palette.md`) to `frontend-design`: "Choose the palette that
     best serves this narrative's mood and audience. Read the palette you pick from
     `references/palettes/<file>`. If none fits, invent a custom palette using the
     same CSS variable names."
   - **Wildcard** → deliberately diverge. Do NOT read a palette file. Instruct
     `frontend-design`: "Invent a fresh, cohesive palette using our CSS variable
     names and a font pairing you have not used recently. Don't default to Community
     Purple. Pick an unexpected mood that still serves the narrative." Vary it every
     run — never converge.

4. **Prepare the build.**
   - **Resolve the durable base dir** — two cases only:
     - If `$CLAUDE_ARTIFACTS_DIR` is set → use it verbatim (explicit override).
     - Otherwise → `$HOME/.local/share/claude/artifacts` (fixed default).
     Create it if needed. The home-based default is outside any repo, so the
     "never inside a git repo" guardrail holds automatically.
   - **Build a meaningful name.** `<YYYY-MM-DD>-<kebab-title>-<shortid>.html`:
     - `kebab-title` — 3–5 kebab-case words derived from the **thesis subject**
       (step 2), audience-meaningful — e.g. `he4rt-faculdade-censo`, not
       `colleges-survey-results`.
     - `shortid` — a 4-char base36 id from a hash of the title + current
       timestamp, so two runs on the same topic never clobber each other.
   - **Output path:** `<base>/<YYYY-MM-DD>-<kebab-title>-<shortid>.html`.
     Compute it **once here** and reuse it for the whole session (the "adjust:"
     loop in step 7 must not recompute a new shortid).
   - Read the selected palette file (unless Auto — see step 3)
   - Read `references/art-direction.md` for taxonomy and visual vocabulary
   - Read `references/skeleton.html` — the invariant scaffold to build from

5. **Invoke `frontend-design`.** Use the Skill tool passing this context:
   - The narrative brief from step 2
   - The GUARDRAILS from `references/guardrails.md` — read the file and pass
     it verbatim
   - The palette (CSS variables + contrast rules) — either the specific
     palette chosen in step 3, or the full catalog + auto-selection
     instruction
   - The art-direction defaults from `references/art-direction.md`
     (taxonomy, typography, tempero) — note these are defaults that
     `frontend-design` may override if the briefing warrants
   - The invariant scaffold from `references/skeleton.html` — pass it with:
     "Start from this skeleton. Fill the SLOTs (title, fonts, palette `:root`,
     `<body>`) freely and build whatever layout serves the narrative, but keep
     the lines marked *invariant* (viewport, Tailwind v4 CDN, box-sizing,
     overflow discipline, reduced-motion). It is a scaffold, not a look — do not
     imitate its structure."
   - Instruction: "Choose the layout form that best serves this narrative.
     You are not restricted to any template."
   - Instruction: "Save the file to `<path>`"

6. **Deliver.** Before presenting, do a quick robustness pass on the generated
   file (per `guardrails.md` → *Layout robustness*): no horizontal body scroll,
   wide blocks scroll in their own container, long strings wrap, nothing clips at
   a narrow width. Then print the resolved output path from step 4 as a clickable
   link: `file://<base>/<YYYY-MM-DD>-<kebab-title>-<shortid>.html`
   Never auto-open the browser.

7. **Approval gate.** Ask the user: approved, or "adjust: ...".
   - **"adjust: ..."** → re-invoke `frontend-design` with the delta appended
     to the original context. Overwrite the **same path** from step 4 (keep the
     same shortid — do not recompute). Loop to step 6.
   - **Approved** → done. The file stays in the durable base dir. Append one
     discovery line to `<base>/index.md`:
     `- [<human title>](<filename>) — <YYYY-MM-DD> — <one-line thesis>`
     Do NOT offer to upload — the user shares it manually when ready.

## GUARDRAILS

The invariant generation rules live in `references/guardrails.md` — a single
file passed **verbatim** to `frontend-design` (step 5). Highlights: one
self-contained `.html`, never inside a git repo, no **Syne** font, no
graph-paper grid backgrounds, no clipping on unknown/phone viewports (wide
content scrolls in its own container), any embedded fetched/user text is
escaped, and the a11y floor holds (WCAG 2.2 AA — visible keyboard focus, meaning
never carried by color alone, native controls, semantic headings). Read the file
for the full list.

## Rules

- NEVER save the artifact inside a git repo.
- The artifact is always 1 self-contained `.html` file (guardrails invariant).
- Show narrative brief to user before generating — no silent generation.
