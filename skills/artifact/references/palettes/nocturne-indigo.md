# Palette (dark nocturne indigo — purple/cyan aurora)
> _Metaphor:_ a night-shift control room — cool indigo panels lit by a purple-to-cyan aurora that sweeps across every heading, stat, and progress bar. Balanced, calm, data-forward. Let this image guide tint, glow, and hierarchy choices.

Ported from a Claude Code insights report. Signature move: a
`linear-gradient(92deg, --brand-2, --teal-bright)` on titles, stat values,
and bar fills, over a cool navy base with five balanced accents
(purple / cyan / green / amber / red) instead of a single hot brand.

Drop this `:root` block into the artifact's `<style>`. To use your own
palette, replace this file — keep the same variable names so the taxonomy
and tempero references in `art-direction.md` still work.
```css
:root {
  /* base */
  --bg:#0a0e16; --surface:#101728; --surface-2:#131b30; --border:#1e2a44;
  --text:#e8ecf4; --muted:#93a0b4; --faint:#5f6c82;
  /* brand + accents */
  --brand:#6a58e6; --brand-2:#8b7cff; --brand-soft:#8b7cff;
  --accent-mag:#d4404f; --accent-mag-bright:#ff6b7a;
  --teal:#0aa9bd; --teal-bright:#43d7e8;
  /* semantic: type/category colors */
  --t-pr:#8b7cff; --t-issue:#f2b94b; --t-commit:#8b7cff;
  --t-review:#43d7e8; --t-comment:#43d7e8; --t-reaction:#ff6b7a;
  /* semantic: state */
  --st-merged:#8b7cff; --st-open:#3ddc97; --st-closed:#ff6b7a;
  /* semantic: diff */
  --add:#3ddc97; --del:#ff6b7a;
}
```
## Contrast rule
Dark theme. `--brand` #6a58e6 is a deep surface/border/fill hex — for purple
text or titles on dark use `--brand-2` / `--brand-soft` #8b7cff. Same for the
red pair: `--accent-mag` #d4404f is deep-only; use `--accent-mag-bright`
#ff6b7a for text. The cyan pair follows the rule too — `--teal` #0aa9bd for
fills, `--teal-bright` #43d7e8 for text and links. The `--t-*`, `--st-*`, and
diff colors are already tuned bright for legends and badges on dark.
## Signature gradient
Titles, stat values, and bar fills use `linear-gradient(92deg,#8b7cff,#43d7e8)`
clipped to text (`-webkit-background-clip:text`). Header glow:
`radial-gradient(ellipse 60% 90% at 15% 0%, rgba(139,124,255,.12), transparent 60%)`
plus a cyan echo at 85%. Give gradient-clipped text `display:inline-block` +
`padding:.04em .02em .1em` + `line-height:≥1.15` so the line-box never chops the
glyphs, and paint the header glow on a full-bleed element (inner max-width
container) so it fades out instead of ending at the wrapper's padding seam.
See `guardrails.md` → Layout robustness.
## Extra accents (badges / legends)
`#f2b94b` (amber) `#3ddc97` (green) `#ff6b7a` (red) `#43d7e8` (cyan)
`#2a3a5e` (border-strong, hover) `#0d1220` (bg-inset, bars)
## Light variant
The source theme ships a matching light mode — same variable names, swap the
`:root` block for this to build a theme toggle:
```css
:root {
  --bg:#f4f6fb; --surface:#ffffff; --surface-2:#f7f9fe; --border:#dde3f0;
  --text:#1a2233; --muted:#5a6478; --faint:#8b96ab;
  --brand:#6a58e6; --brand-2:#6a58e6; --brand-soft:#6a58e6;
  --accent-mag:#d4404f; --accent-mag-bright:#d4404f;
  --teal:#0aa9bd; --teal-bright:#0aa9bd;
  --t-pr:#6a58e6; --t-issue:#b57e12; --t-commit:#6a58e6;
  --t-review:#0aa9bd; --t-comment:#0aa9bd; --t-reaction:#d4404f;
  --st-merged:#6a58e6; --st-open:#149a63; --st-closed:#d4404f;
  --add:#149a63; --del:#d4404f;
}
```
