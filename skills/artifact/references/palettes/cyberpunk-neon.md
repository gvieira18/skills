# Palette (cyberpunk — near-black + neon cyan/magenta/orange)
> _Metaphor:_ a rain-slick neon alley — near-black wet asphalt under buzzing cyan and magenta signs. High-energy, hype, maximal. Let this image guide tint, glow, and hierarchy choices.

Drop this `:root` block into the artifact's `<style>`. To use your own
palette, replace this file — keep the same variable names so the taxonomy
and tempero references in `art-direction.md` still work.
```css
:root {
  /* base */
  --bg:#06060c; --surface:#0c0c18; --surface-2:#12122a; --border:#0d2d3a;
  --text:#e8e8f0; --muted:#8888aa; --faint:#555577;
  /* brand + accents */
  --brand:#00e5ff; --brand-2:#ff00e5; --brand-soft:#7af1ff;
  --accent-mag:#ff00e5; --accent-mag-bright:#ff52ed;
  --teal:#00e5ff; --teal-bright:#52edff;
  /* semantic: type/category colors */
  --t-pr:#ff00e5; --t-issue:#ffe100; --t-commit:#7af1ff;
  --t-review:#ff6b00; --t-comment:#00e5ff; --t-reaction:#ff2d55;
  /* semantic: state */
  --st-merged:#ff00e5; --st-open:#00ff88; --st-closed:#ff2d55;
  /* semantic: diff */
  --add:#00ff88; --del:#ff2d55;
}
```
## Contrast rule
Maximal bright-on-black. Every accent is a neon and reads as text on `--bg`.
The signature move is glow, not fill: `box-shadow:0 0 20px rgba(0,229,255,.3),
0 0 40px rgba(0,229,255,.1)` for cyan (swap the rgba for magenta/orange). Use a
faint cyan grid background via `--border` at low alpha. Never fill a large area
with a neon — it vibrates; keep neons to text, 1px borders, and glows.
## Extra atmosphere values
grid `rgba(0,229,255,0.04)` · grid-line `rgba(0,229,255,0.08)` ·
border-subtle `rgba(0,229,255,0.15)` · neon-yellow `#ffe100` · neon-orange `#ff6b00`
