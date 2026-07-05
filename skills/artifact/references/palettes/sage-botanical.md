# Palette (light — sage botanical + forest green)
> _Metaphor:_ a herbarium page in daylight — soft green-tinted paper, deep forest ink, pressed-clay accents. Organic, calm, editorial-fresh. Let this image guide tint, glow, and hierarchy choices.

Drop this `:root` block into the artifact's `<style>`. To use your own
palette, replace this file — keep the same variable names so the taxonomy
and tempero references in `art-direction.md` still work.
```css
:root {
  /* base */
  --bg:#f3f5ef; --surface:#fbfcf9; --surface-2:#e9ede2; --border:#d3dac6;
  --text:#1c2418; --muted:#4d5645; --faint:#7f8a72;
  /* brand + accents */
  --brand:#2f6b3f; --brand-2:#9a4a24; --brand-soft:#1f5130;
  --accent-mag:#9d3466; --accent-mag-bright:#7c2751;
  --teal:#0f766e; --teal-bright:#115e56;
  /* semantic: type/category colors */
  --t-pr:#2f6b3f; --t-issue:#b45309; --t-commit:#4d7c0f;
  --t-review:#9d3466; --t-comment:#0f766e; --t-reaction:#b91c1c;
  /* semantic: state */
  --st-merged:#6b21a8; --st-open:#2f6b3f; --st-closed:#b91c1c;
  /* semantic: diff */
  --add:#2f6b3f; --del:#b91c1c;
}
```
## Contrast rule
LIGHT theme on a green-tinted paper `--bg`. The deep botanical hexes (`--brand`
#2f6b3f forest, `--accent-mag` plum-berry, `--teal` pine, `--st-*`) are the
TEXT/accent colors — already deep enough to read on the pale surfaces.
`--brand-soft` #1f5130 is a *deeper* forest for stronger emphasis; `--brand-2`
#9a4a24 is a pressed-clay secondary that pairs with the green without competing.
For diff/callout fills use the pale tints below, with the saturated hex as the text
on top.
## Extra fill tints (callouts / diff backgrounds)
`#dcefe0` (added) `#f7dede` (removed) `#f5ead0` (changed)
`#e0ebe2` (brand) `#f0e2ea` (plum) `#efe1d4` (clay)
`#c1cbb2` (strong border) `#e4e9db` (code bg)
