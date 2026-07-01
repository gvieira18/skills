# GUARDRAILS

Invariant generation rules for every artifact. The `artifact` skill reads this
file and passes it **verbatim** to `frontend-design` (workflow step 5). These are
hard constraints on the generated HTML — never relax them, regardless of branch
or briefing.

## Output

- Single self-contained `.html` file. No separate CSS/JS/image files.
- NEVER save inside a git repository — always the durable base dir resolved in
  step 4 (`$CLAUDE_ARTIFACTS_DIR` if set, else `$HOME/.local/share/claude/artifacts`).

## Assets & delivery

- Tailwind via `<script src="https://cdn.tailwindcss.com"></script>`.
- Icons: SVG inline or emoji. Diagrams: HTML/CSS, inline SVG, or Mermaid via CDN.
- No relative paths (`./`, `../`) for assets. Fonts: system stack or Google Fonts CDN.

## Typography

- NEVER use the **Syne** font — nor any similarly ultra-condensed/elongated
  ("extended") display face. It hurts legibility, especially for dyslexia and ADHD.
- Also avoid the generic defaults: Inter, Roboto, Arial.
- Vary the display face to fit the brief (e.g. Space Grotesk, Fraunces, Sora) —
  never converge on the same pairing. Use **Lexend** when accessibility is a priority.

## Backgrounds

- NEVER use the crossed `linear-gradient` "graph paper" grid as a page background
  (e.g. `linear-gradient(var(--line) 1px, transparent 1px)` plus a `90deg` twin).
  It renders oversized squares that fight the content and hurt readability,
  especially in light themes. Prefer a plain or softly-tinted background; a subtle
  radial glow is fine.
