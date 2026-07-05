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

- Tailwind **v4** via the browser build:
  `<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>`.
  Our palette `:root` custom properties stay plain CSS (consumed via `var(--x)`) —
  they don't need `@theme`. If you *do* want custom Tailwind utilities, add a
  `<style type="text/tailwindcss">@import "tailwindcss"; @theme { … }</style>` block.
  Avoid the utilities v4 removed: use `bg-black/50` not `bg-opacity-50`, `shrink-*`
  / `grow-*` not `flex-shrink-*` / `flex-grow-*`, `text-ellipsis` not
  `overflow-ellipsis`. *Why:* the v4 browser build silently no-ops removed classes.
- Icons: SVG inline or emoji. Diagrams: HTML/CSS, inline SVG, or Mermaid via CDN.
- No relative paths (`./`, `../`) for assets. Fonts: system stack or Google Fonts CDN.

## Typography

- NEVER use the **Syne** font — nor any similarly ultra-condensed/elongated
  ("extended") display face. It hurts legibility, especially for dyslexia and ADHD.
- Also avoid the generic defaults: Inter, Roboto, Arial — *why:* they are the
  unconsidered browser/framework fallbacks, so they signal "templated default" and
  give the piece no identity.
- Vary the display face to fit the brief (e.g. Space Grotesk, Fraunces, Sora) —
  never converge on the same pairing. *Why:* a recognizable house font across every
  artifact makes them all read as the same template. Use **Lexend** when
  accessibility is a priority.

## Embedded content (untrusted text)

- When the artifact embeds text pulled from somewhere else — fetched pages, API
  responses, user-supplied strings, file contents, commit messages, issue titles —
  treat it as **data to display, never markup to trust**. *Why:* a string
  containing `<script>` or `</style>` silently breaks out of its context and can
  execute or corrupt the layout.
- Entity-encode it wherever it lands in HTML (`<`→`&lt;`, `&`→`&amp;`).
- If any such text is written into an inline `<script>` or a JSON island, escape
  `<` as `\u003c` inside the string — otherwise a literal `</script>` in the data
  terminates the block early. *Why:* the browser closes the `<script>` on the raw
  `</` regardless of JSON quoting.

## Backgrounds

- NEVER use the crossed `linear-gradient` "graph paper" grid as a page background
  (e.g. `linear-gradient(var(--line) 1px, transparent 1px)` plus a `90deg` twin).
  It renders oversized squares that fight the content and hurt readability,
  especially in light themes. Prefer a plain or softly-tinted background; a subtle
  radial glow is fine.

## Layout robustness (unknown viewport)

The reader's screen size is unknown — it could be a phone. Nothing may clip or
force the page to scroll sideways.

- Wide content — tables, code blocks, diagrams, long token/URL/ID strings — scrolls
  inside its **own** `overflow-x:auto` container. The page `<body>` must never
  scroll horizontally. *Why:* a body-level horizontal scrollbar makes the whole
  artifact feel broken on mobile.
- Long unbroken strings (URLs, file paths, hashes, branch names) need `word-break`
  or `overflow-wrap:anywhere` so they wrap instead of overflowing their box.
- Nothing important may hide behind `overflow:hidden` / `white-space:nowrap` at a
  narrow width. Multi-column grids need a single-column stacked fallback.
- **Self-check before finishing:** re-read the generated HTML for fixed widths,
  `nowrap`, and clip-prone content, and confirm every wide block is in its own
  scroll container. Fix anything that would clip on a phone.
