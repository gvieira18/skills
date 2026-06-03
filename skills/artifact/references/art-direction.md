# Art Direction Reference

Default visual vocabulary for artifacts. These are defaults — override
when the briefing calls for a different mood, audience, or brand.

## Palette

Read `references/palette.md` for the CSS `:root` block and contrast rules.
To use a custom palette, replace that file — keep the same variable names.

## Typography

Do NOT use generic fonts (Inter, Roboto, Arial). Pair via Google Fonts CDN:
- **Display** (titles): characterful, memorable (e.g. Fraunces, Playfair Display, Syne)
- **Body** (prose): legible, clean (e.g. Hanken Grotesk, DM Sans, Source Sans 3)
- **Mono** (code/data): JetBrains Mono, Fira Code, or IBM Plex Mono

Vary font choices between artifact generations — never converge on the same pairing.

## Taxonomy of Information Forms

Repertoire, not templates. Mix, adapt, invent hybrids. The LLM picks the
form(s) that best serve the narrative.

| Communication need | Visual approach |
|---|---|
| Situate the reader | Category tag (mono, uppercase) + display title + thesis subtitle |
| Structure a long argument | Numbered chapters with context paragraphs |
| Show data/information flow | Numbered nodes with connections, branches |
| Show a change (before/after) | Side-by-side panels or toggle; before in `--del`, after in `--add` |
| Show point transformations | Cards with `from → to` and arrow |
| Record a decision with trade-offs | Options side-by-side with pros/cons; chosen = highlighted |
| Map architecture/contexts | Module cards with icon, namespace, dependencies |
| Guide a step-by-step | Numbered steps with affected files; optional vertical timeline |
| Highlight isolated info | Semantic callout (problem=`--del`, solution=`--st-open`, warning=`--teal-bright`) |
| Show data structure/code | Columns `name : type`, directory trees in mono |

Multiple forms can coexist in one artifact. Short prose connects them —
the artifact is narrative, not a diagram dump.

## Visual Tempero (use 2-4 per artifact max)

Decorative touches that add rhythm and identity — seasoning, not structure:

- **Badges/tags**: colored labels (`--st-open`=ok, `--del`=risk, `--muted`=neutral)
- **Atmosphere**: `radial-gradient` in `--brand`/`--accent-mag` + grain overlay (SVG `feTurbulence`, opacity .05)
- **Reveal on scroll**: fade-in + translate, staggered by section (only for long scrollable artifacts)
- **Avatar rings**: padding + `conic-gradient` from `--brand` → `--brand-2` (no CSS mask — breaks in some browsers). `onerror` → fallback with initials
- **Icons**: SVG inline or emoji, one per section title max
- **Staggered entrances**: `animation-delay` per element for orchestrated reveals
- **Micro-interactions**: hover effects on cards, links, interactive elements
