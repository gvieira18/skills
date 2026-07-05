# Palette (dark zinc + emerald — the artefato default)
> _Metaphor:_ a dark instrument panel — brushed graphite with one emerald readout glowing. Subdued, data-forward, everything else steps back. Let this image guide tint, glow, and hierarchy choices.

Drop this `:root` block into the artifact's `<style>`. To use your own
palette, replace this file — keep the same variable names so the taxonomy
and tempero references in `art-direction.md` still work.
```css
:root {
  /* base */
  --bg:#09090b; --surface:#18181b; --surface-2:#1f1f23; --border:#27272a;
  --text:#f4f4f5; --muted:#a1a1aa; --faint:#71717a;
  /* brand + accents */
  --brand:#10b981; --brand-2:#22c55e; --brand-soft:#34d399;
  --accent-mag:#87069e; --accent-mag-bright:#f0abfc;
  --teal:#60a5fa; --teal-bright:#75b1fb;
  /* semantic: type/category colors */
  --t-pr:#10b981; --t-issue:#fbbf24; --t-commit:#34d399;
  --t-review:#f0abfc; --t-comment:#60a5fa; --t-reaction:#f87171;
  /* semantic: state */
  --st-merged:#e87ffa; --st-open:#34d399; --st-closed:#f87171;
  /* semantic: diff */
  --add:#6ee7b7; --del:#f87171;
}
```
## Contrast rule
Dark saturated hexes (`--brand` #10b981, `--accent-mag` #87069e) are fine as
accents but the deep magenta is for surfaces/borders only. For text on dark
backgrounds use the bright siblings (`--brand-soft`, `--accent-mag-bright`,
`--teal-bright`, `--st-open`, `--add`). The `--line` color `#3f3f46` suits
diagram strokes.
## Extra deep hexes (surfaces/accents)
`#0c0c0e` `#3f3f46` `#16a34a` `#facc15` `#fbbf24` `#f59e0b` `#ef4444`
