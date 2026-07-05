# Palette (light — warm stone + amber accent)
> _Metaphor:_ a stone gallery in afternoon light — warm taupe walls with amber and jewel-tone accents. Refined, tactile, unhurried. Let this image guide tint, glow, and hierarchy choices.

Drop this `:root` block into the artifact's `<style>`. To use your own
palette, replace this file — keep the same variable names so the taxonomy
and tempero references in `art-direction.md` still work.
```css
:root {
  /* base */
  --bg:#e8e4df; --surface:#f4f1ed; --surface-2:#ebe7e2; --border:#c5c0b8;
  --text:#292524; --muted:#57534e; --faint:#78716c;
  /* brand + accents */
  --brand:#b45309; --brand-2:#6b21a8; --brand-soft:#cc5e0a;
  --accent-mag:#6b21a8; --accent-mag-bright:#882ad5;
  --teal:#0f766e; --teal-bright:#0d6e64;
  /* semantic: type/category colors */
  --t-pr:#6b21a8; --t-issue:#854d0e; --t-commit:#b45309;
  --t-review:#0f766e; --t-comment:#166534; --t-reaction:#991b1b;
  /* semantic: state */
  --st-merged:#6b21a8; --st-open:#166534; --st-closed:#991b1b;
  /* semantic: diff */
  --add:#166534; --del:#991b1b;
}
```
## Contrast rule
LIGHT theme on a warm taupe `--bg`. The saturated jewel tones (`--brand`,
`--accent-mag`, `--teal`, `--st-*`) are TEXT/accent colors — they're already
deep enough to read on the pale surfaces. `--brand-soft` is a brighter amber for
hover/emphasis. In a light theme the `-bright` sibling of a mid-tone hue must go
*deeper*, not lighter, to stay legible — `--teal-bright` #0d6e64 is a deeper pine
for teal emphasis text (a lighter teal would wash out on the pale bg). Use the
`*-dim` rgba tints (8% of each accent) for callout and badge fills, with the solid
hex as the text on top.
## Extra surfaces / strong borders
`#ddd9d3` (inset) `#d6d2cc` (code bg) `#a8a29e` (strong border) `#854d0e` (yellow)
