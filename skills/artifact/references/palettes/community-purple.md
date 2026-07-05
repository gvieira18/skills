# Palette (dark community purple — he4rt brand)
> _Metaphor:_ a late-night community hall lit purple — pink and teal neon washing over the crowd. Social, warm, energetic. Let this image guide tint, glow, and hierarchy choices.

Drop this `:root` block into the artifact's `<style>`. To use your own
palette, replace this file — keep the same variable names so the taxonomy
and tempero references in `art-direction.md` still work.
```css
:root {
  /* base */
  --bg:#0b0a10; --surface:#15131c; --surface-2:#1c1926; --border:#2c2838;
  --text:#f4f2f8; --muted:#9c95ab; --faint:#6b6379;
  /* brand + accents */
  --brand:#782bf1; --brand-2:#c44bff; --brand-soft:#b69bff;
  --accent-mag:#99002f; --accent-mag-bright:#ff5d8f;
  --teal:#2dd4bf; --teal-bright:#56dccb;
  /* semantic: type/category colors */
  --t-pr:#ff5d8f; --t-issue:#f6b73c; --t-commit:#b39bff;
  --t-review:#2dd4bf; --t-comment:#62a6ff; --t-reaction:#fb6f92;
  /* semantic: state */
  --st-merged:#a06bff; --st-open:#34d399; --st-closed:#f87171;
  /* semantic: diff */
  --add:#4ade80; --del:#f87171;
}
```
## Contrast rule
Dark theme. `--brand` #782bf1 works as an accent on `--bg` but is borderline for
small text — prefer `--brand-soft` #b69bff for text/titles on dark. `--accent-mag`
#99002f is a deep surface/border only; use `--accent-mag-bright` #ff5d8f for text.
The `--t-*` set is already tuned bright for legends and badges on dark.
## Extra accents (badges / legends)
`#c44bff` (brand-2) `#62a6ff` (comment blue) `#fb6f92` (reaction pink)
`#f6b73c` (issue amber) `#a06bff` (merged violet)
