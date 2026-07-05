# Palette (light — warm paper + blue accent)
> _Metaphor:_ a sunlit study desk — warm paper with a single blue fountain-pen line. Editorial, calm, made for sustained reading. Let this image guide tint, glow, and hierarchy choices.

Drop this `:root` block into the artifact's `<style>`. To use your own
palette, replace this file — keep the same variable names so the taxonomy
and tempero references in `art-direction.md` still work.
```css
:root {
  /* base */
  --bg:#fafaf8; --surface:#ffffff; --surface-2:#f3f2ee; --border:#e4e2dc;
  --text:#1a1a18; --muted:#5c5b56; --faint:#8a8880;
  /* brand + accents */
  --brand:#2563eb; --brand-2:#7c3aed; --brand-soft:#124ac4;
  --accent-mag:#7c3aed; --accent-mag-bright:#6a1feb;
  --teal:#0f766e; --teal-bright:#134fd2;
  /* semantic: type/category colors */
  --t-pr:#7c3aed; --t-issue:#d97706; --t-commit:#2563eb;
  --t-review:#0f766e; --t-comment:#2563eb; --t-reaction:#dc2626;
  /* semantic: state */
  --st-merged:#7c3aed; --st-open:#16a34a; --st-closed:#dc2626;
  /* semantic: diff */
  --add:#16a34a; --del:#dc2626;
}
```
## Contrast rule
This is a LIGHT theme — the logic inverts. Saturated hexes (`--brand`,
`--accent-mag`, `--st-*`) are the TEXT/accent colors on the pale `--bg`;
`--brand-soft` here is a *darker* sibling for stronger emphasis, not a lighter
one. Never put `--muted`/`--faint` text on `--surface-2` for body copy — too low
contrast. For diff/callout fills use the bg tints below, with the saturated hex
as text.
## Extra fill tints (callouts / diff backgrounds)
`#e6f9ed` (added) `#fde8e8` (removed) `#fff8e0` (changed)
`#dcfce7` `#fee2e2` `#fef3c7` `#ede9fe` `#dbeafe` `#ccc9c0` (strong border)
