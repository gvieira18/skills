# Palette (cyber terminal — navy + neon green/cyan)
Drop this `:root` block into the artifact's `<style>`. To use your own
palette, replace this file — keep the same variable names so the taxonomy
and tempero references in `art-direction.md` still work.
```css
:root {
  /* base */
  --bg:#0a0e14; --surface:#111827; --surface-2:#172135; --border:#1e293b;
  --text:#e2e8f0; --muted:#94a3b8; --faint:#6b7280;
  /* brand + accents */
  --brand:#00d4ff; --brand-2:#00ff9d; --brand-soft:#70e7ff;
  --accent-mag:#ff3b5c; --accent-mag-bright:#ff5c77;
  --teal:#00d4ff; --teal-bright:#3ddeff;
  /* semantic: type/category colors */
  --t-pr:#00ff9d; --t-issue:#f59e0b; --t-commit:#70e7ff;
  --t-review:#ff5c77; --t-comment:#00d4ff; --t-reaction:#ff3b5c;
  /* semantic: state */
  --st-merged:#70e7ff; --st-open:#00ff9d; --st-closed:#ff3b5c;
  /* semantic: diff */
  --add:#00ff9d; --del:#ff3b5c;
}
```
## Contrast rule
This palette is already bright-on-dark: neon accents (`--brand`, `--brand-2`,
`--accent-mag`) read fine as text on `--bg`/`--surface`. Keep neon for text and
thin borders, never as a large fill behind body text. Optional glow:
`box-shadow:0 0 20px rgba(0,212,255,.3)` on key elements — use sparingly.
## Extra deep hexes (surfaces/accents)
`#151b28` `#1a3a4a` `#00cc7d` `#0099bb` `#cc2f4a`
