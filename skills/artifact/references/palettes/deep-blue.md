# Palette (dark deep-blue + violeta) — DEFAULT
> _Metaphor:_ a midnight planetarium — a royal-indigo dome with violet and teal starlight. Authoritative, structural, calm. Let this image guide tint, glow, and hierarchy choices.

Drop this `:root` block into the artifact's `<style>`. To use your own
palette, replace this file — keep the same variable names so the taxonomy
and tempero references in `art-direction.md` still work.
```css
:root {
  /* base */
  --bg:#0a0911; --surface:#15121f; --surface-2:#1d1929; --border:#2c2740;
  --text:#f3f1f9; --muted:#9990ad; --faint:#5a5470;
  /* brand + accents */
  --brand:#1f109b; --brand-2:#782bf1; --brand-soft:#8f83ec;
  --accent-mag:#610e5c; --accent-mag-bright:#e359db;
  --teal:#084d6e; --teal-bright:#37b5f0;
  /* semantic: type/category colors */
  --t-pr:#782bf1; --t-issue:#37b5f0; --t-commit:#8f83ec;
  --t-review:#e359db; --t-comment:#37b5f0; --t-reaction:#e7746a;
  /* semantic: state */
  --st-merged:#9f6dee; --st-open:#4ceb47; --st-closed:#e7746a;
  /* semantic: diff */
  --add:#81d577; --del:#e7746a;
}
```
## Contrast rule
Dark saturated hexes (`--brand`, `--accent-mag`, `--teal`) are for
backgrounds and surfaces. For text on dark backgrounds, use their bright
siblings (`--brand-soft`, `--accent-mag-bright`, `--teal-bright`,
`--st-open`, `--add`). Never put `--brand` text directly on `--bg`.
## Extra deep hexes (surfaces/accents)
`#084d6e` `#3b1635` `#46093e` `#257e1a` `#066603` `#1f4919`
`#a82517` `#a2190d` `#a1120b` `#86110a` `#8146DC`
