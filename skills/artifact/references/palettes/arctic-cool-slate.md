# Palette (light — arctic cool slate + cobalt)
> _Metaphor:_ a bright ski-lodge window at altitude — cool blue-white daylight, cobalt and glacier-cyan on snow. Crisp, corporate, tech-forward. Let this image guide tint, glow, and hierarchy choices.

Drop this `:root` block into the artifact's `<style>`. To use your own
palette, replace this file — keep the same variable names so the taxonomy
and tempero references in `art-direction.md` still work.
```css
:root {
  /* base */
  --bg:#f6f8fb; --surface:#ffffff; --surface-2:#eef2f7; --border:#d8e0ea;
  --text:#0f1b2d; --muted:#4a5a6e; --faint:#7c8aa0;
  /* brand + accents */
  --brand:#1d4ed8; --brand-2:#0891b2; --brand-soft:#1e3a8a;
  --accent-mag:#be185d; --accent-mag-bright:#9d174d;
  --teal:#0e7490; --teal-bright:#155e75;
  /* semantic: type/category colors */
  --t-pr:#1d4ed8; --t-issue:#b45309; --t-commit:#0e7490;
  --t-review:#be185d; --t-comment:#4338ca; --t-reaction:#dc2626;
  /* semantic: state */
  --st-merged:#6d28d9; --st-open:#15803d; --st-closed:#dc2626;
  /* semantic: diff */
  --add:#15803d; --del:#dc2626;
}
```
## Contrast rule
LIGHT theme on a cool blue-white `--bg`. The saturated hexes (`--brand` #1d4ed8,
`--accent-mag`, `--teal`, `--st-*`) are the TEXT/accent colors — deep enough to read
on the pale surfaces. `--brand-soft` #1e3a8a is a *deeper* cobalt for stronger
emphasis (in a light theme the emphasis sibling darkens, it doesn't lighten).
`--brand-2` #0891b2 is a brighter glacier-cyan — reserve it for large accents,
badges, and thin borders, not body-size text. For diff/callout fills use the pale
tints below, with the saturated hex as the text on top.
## Extra fill tints (callouts / diff backgrounds)
`#dcfce7` (added) `#fee2e2` (removed) `#fef3c7` (changed)
`#dbeafe` (brand) `#cffafe` (cyan) `#ede9fe` (violet)
`#c3ccd9` (strong border) `#e8edf3` (code bg)
