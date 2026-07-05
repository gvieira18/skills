# Palette (light — blush editorial rose + burgundy)
> _Metaphor:_ a letterpress wedding suite — soft rose-cream stock, burgundy ink, dusty-rose and gold flourishes. Elegant, warm, editorial. Let this image guide tint, glow, and hierarchy choices.

Drop this `:root` block into the artifact's `<style>`. To use your own
palette, replace this file — keep the same variable names so the taxonomy
and tempero references in `art-direction.md` still work.
```css
:root {
  /* base */
  --bg:#fbf5f4; --surface:#fffdfc; --surface-2:#f4e9e7; --border:#e6d2cf;
  --text:#2a1b1e; --muted:#6b504f; --faint:#9c807e;
  /* brand + accents */
  --brand:#9d1f4a; --brand-2:#a16207; --brand-soft:#7c1638;
  --accent-mag:#be185d; --accent-mag-bright:#9d174d;
  --teal:#0f766e; --teal-bright:#115e56;
  /* semantic: type/category colors */
  --t-pr:#9d1f4a; --t-issue:#a16207; --t-commit:#86198f;
  --t-review:#be185d; --t-comment:#0f766e; --t-reaction:#b91c1c;
  /* semantic: state */
  --st-merged:#86198f; --st-open:#15803d; --st-closed:#b91c1c;
  /* semantic: diff */
  --add:#15803d; --del:#b91c1c;
}
```
## Contrast rule
LIGHT theme on a rose-cream `--bg`. The deep hexes (`--brand` #9d1f4a burgundy,
`--accent-mag` rose-magenta, `--st-*`) are the TEXT/accent colors — deep enough to
read on the pale surfaces. `--brand-soft` #7c1638 is a *deeper* burgundy for
stronger emphasis; `--brand-2` #a16207 is an antique-gold secondary. `--teal`
#0f766e is the one cool note — use it sparingly as a contrast pop against the warm
field. For diff/callout fills use the pale tints below, with the saturated hex as
the text on top.
## Extra fill tints (callouts / diff backgrounds)
`#dcf0e2` (added) `#f9dede` (removed) `#f6ecd6` (changed/gold)
`#f7e0ea` (rose) `#f0e2ef` (plum) `#dcefec` (teal)
`#d9c4c1` (strong border) `#f2e7e5` (code bg)
