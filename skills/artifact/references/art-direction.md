# Art Direction Reference

Default visual vocabulary for artifacts. These are defaults — override
when the briefing calls for a different mood, audience, or brand.

## Palette

See `references/palette.md` for the full palette catalog. Individual palettes
live in `references/palettes/<name>.md` — each contains a CSS `:root` block,
contrast rules, and extra colors. All palettes use the same variable names, so
the taxonomy and tempero references below work with any of them.

## Typography

Do NOT use generic fonts (Inter, Roboto, Arial) or Syne (see `guardrails.md`).
Pair via Google Fonts CDN:
- **Display** (titles): characterful, memorable (e.g. Fraunces, Playfair Display, Sora)
- **Body** (prose): legible, clean (e.g. Hanken Grotesk, DM Sans, Source Sans 3)
- **Mono** (code/data): JetBrains Mono, Fira Code, or IBM Plex Mono

Vary font choices between artifact generations — never converge on the same pairing.

**Example pairing** (ops/infra report with Cyber Terminal palette):
- Display: **Space Mono** — monospace display font reinforces terminal aesthetic
- Body: **Outfit** — clean geometric sans, readable, doesn't compete with mono accents
- Code: **JetBrains Mono** — industry standard, familiar to dev audience

## Layout

Default the main content container to **`max-w-6xl`** (1152px), not the
narrower `max-w-3xl` (768px). The wider measure gives code blocks, commit
lists, data tables, and module bars room to breathe without feeling cramped.

- Go narrower (`max-w-3xl`/`max-w-4xl`) only for prose-dominant artifacts where
  a tight measure aids readability
- Go wider (`max-w-7xl` / full-bleed) for dashboards or dense multi-column grids
- Keep generous horizontal padding so content never touches the viewport edge

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

## Color Semantics

Use color consistently throughout the artifact to encode meaning. Map
semantic variables to a **fixed vocabulary** so the reader never has to
re-learn what a color means mid-document.

| Meaning | CSS vars | Usage |
|---|---|---|
| Correct, fixed, new, success | `--add`, `--brand-2`, `--st-open` | "After" panels, positive badges, resolved states |
| Wrong, broken, removed, problem | `--del`, `--accent-mag` | "Before" panels, error badges, risk callouts |
| Neutral highlight, category, structure | `--brand`, `--teal` | Borders, section labels, table headers |
| Warning, caution, attention | `--t-issue` (amber) | Warning callouts, medium-priority categories |
| Secondary, skipped, not applicable | `--faint`, `--muted` | De-emphasized text, disabled states |

Once a color-to-meaning mapping is established in section 1, maintain it
through the entire artifact. Inconsistent color semantics break scannability.

**Color is never the *only* cue (WCAG 1.4.1).** Whenever a hue encodes state, pair
it with a redundant non-color signal — a text label, an icon, or a shape — so the
meaning survives for colorblind readers. A before/after pair carries "Before"/"After"
text, not just red/green panels; a badge holds a word or icon, not a bare colored
dot. The color reinforces the meaning; it never carries it alone. (See `guardrails.md`
→ Accessibility.)

## Component Patterns

Reusable building blocks. Combine with taxonomy forms — these are
implementation patterns, not layouts.

| Component | Good for | Pattern |
|---|---|---|
| **Flow node** | Strategy diagrams, category cards, architecture blocks | Surface bg + border + optional glow (`box-shadow: 0 0 20px rgba(...)`) |
| **Terminal block** | Production procedures, CLI instructions, shell output | Fake terminal chrome (dot bar + title) + colored output lines (`--add`/`--del`) |
| **Data table** | Module breakdowns, comparison stats, metrics | Monospace font, `--brand` headers, last-row bold for totals |
| **Timeline** | Context history, chronological events, changelogs | `border-left` with colored dots, color-coded by era/status |
| **Commit list** | Git history, changelog sections | Hash in mono + type badge (`--t-commit`) + description |
| **Before/After/Step** | Migration flows, transformation explanations | 3-column layout: before (`--del`) → step → after (`--add`) |

### Design Heuristics (from production artifacts)

**What works well:**
- Terminal blocks for operational procedures — makes steps feel concrete and executable
- Before/After flow diagrams — 3-column layout more effective than prose for transformations
- Color-coded category badges — consistent color mapping makes classification scannable at a glance
- Numbered chapter layout for long-form reports — enables "check section 06" referencing

**Watch out for:**
- Multi-column layouts break on narrow screens — provide a stacked fallback
- Long data tables benefit from collapsible sections
- Raw stats tables can often be replaced with horizontal bar charts for faster visual comparison

### Deriving tints — the `color-mix` idiom

Don't hand-pick pastel background hexes for panels, callouts, and banners. Derive
them from a semantic hue mixed into the page background, so every tint tracks the
palette automatically (and stays correct if the palette is swapped):

```css
/* tinted panel: 12% of the accent over the page bg */
background: color-mix(in srgb, var(--st-open) 12%, var(--bg));   /* success banner  */
background: color-mix(in srgb, var(--t-issue) 8%,  var(--bg));   /* warning strip   */
background: color-mix(in srgb, var(--del)     6%,  var(--bg));   /* risk callout    */
```

Keep the solid hue as the text/border on top of its own tint (e.g. `--del` text on
a `--del`-tinted panel). One accent hue thus yields a matched border + fill + text
trio without a second variable. Works in both dark and light palettes.

## Anti-Generic Discipline (surface, motion, radius)

Three heuristics that fight the templated-SaaS look harder than font choice alone.
Defaults — override when a palette's metaphor calls for something else (e.g.
Cyberpunk Neon *is* a glow-and-shadow aesthetic).

- **Paper on ink, not a card stack.** Separate surfaces with a 1px border, not a
  drop shadow. Reserve shadows for genuinely *floating* elements (tooltips, menus,
  modals) — never on buttons or in-flow cards. Stacked shadows read as generic
  dashboard chrome. *(Cyberpunk/Cyber Terminal glows are the deliberate exception —
  those are atmosphere, not elevation.)*
- **Fades over moves. No boing.** Transitions are opacity/color fades and small
  translates on a calm ease (`cubic-bezier(0.2,0.6,0.2,1)`), 120–320ms. No bounce,
  spring, or elastic easing — it reads as toy-like and cheapens the piece. Hover =
  raise emphasis (brighten text, lift a border), not a scale-up pop.
- **Round generously, or not at all.** Commit to a radius scale (e.g. 8 / 10 / 12 /
  14px + full-pill for chips) and use it consistently. Avoid the timid `4px` corner
  that reads as an un-considered default — small enough to notice, too small to feel
  intentional.

## Visual Tempero (use 2-4 per artifact max)

Decorative touches that add rhythm and identity — seasoning, not structure:

- **Badges/tags**: colored labels (`--st-open`=ok, `--del`=risk, `--muted`=neutral)
- **Atmosphere**: `radial-gradient` in `--brand`/`--accent-mag` + grain overlay (SVG `feTurbulence`, opacity .05)
- **Reveal on scroll**: fade-in + translate, staggered by section (only for long scrollable artifacts)
- **Avatar rings**: padding + `conic-gradient` from `--brand` → `--brand-2` (no CSS mask — breaks in some browsers). `onerror` → fallback with initials
- **Icons**: SVG inline or emoji, one per section title max
- **Staggered entrances**: `animation-delay` per element for orchestrated reveals
- **Micro-interactions**: hover effects on cards, links, interactive elements
