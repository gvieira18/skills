# Palette Catalog

Available palettes for artifacts. Each file in `references/palettes/` contains
a full `:root` CSS block, contrast rules, and extra color values — all using the
same variable names so `art-direction.md` references work unchanged.

## Dark palettes

| # | Name | File | Mood |
|---|------|------|------|
| 1 | **Deep Blue + Violeta** | `palettes/deep-blue.md` | Royal indigo, neon violet/teal accents |
| 2 | **Zinc + Emerald** | `palettes/zinc-emerald.md` | Neutral dark zinc, green/purple accents |
| 3 | **Community Purple** ★ default | `palettes/community-purple.md` | Deep purple, pink/teal accents (he4rt brand) |
| 4 | **Cyber Terminal** | `palettes/cyber-terminal.md` | Navy terminal, neon green/cyan/red |
| 5 | **Cyberpunk Neon** | `palettes/cyberpunk-neon.md` | Near-black, max neon cyan/magenta/orange glows |

## Light palettes

| # | Name | File | Mood |
|---|------|------|------|
| 6 | **Light Paper** | `palettes/light-paper.md` | Warm off-white, blue/violet accents |
| 7 | **Light Stone** | `palettes/light-stone.md` | Warm taupe, amber/purple jewel-tone accents |

## Selection Heuristics

Match palette to **content domain**, not personal preference. The palette
should feel native to the narrative's subject matter.

| Content domain | Recommended | Why |
|---|---|---|
| Database, infrastructure, DevOps, CLI, terminal output | **Cyber Terminal** | Navy + neon green/cyan makes code blocks and data tables feel native |
| Community dashboards, social features, contributor reports | **Community Purple** | Purple/pink/teal carries a social, community-oriented tone |
| Design/UX reports, documentation, editorial content | **Light Paper** or **Light Stone** | Light backgrounds suit reading-heavy, non-technical audiences |
| High-energy announcements, launches, hype content | **Cyberpunk Neon** | Max neon glows convey energy and excitement |
| Architecture, system design, technical deep-dives | **Deep Blue + Violeta** | Royal indigo feels authoritative for structural content |
| Neutral dashboards, metrics, observability | **Zinc + Emerald** | Subdued zinc keeps data front-and-center without distraction |

When in doubt: if the content is **code-heavy**, pick a dark palette with
monospace-friendly accents. If the content is **prose-heavy**, consider a
light palette for sustained reading comfort.

## Auto mode

When the user picks **"auto"**, do NOT read any palette file yet. Instead,
pass this instruction to `frontend-design`:

> "Choose the palette that best serves this narrative's mood and audience.
> Available palettes are listed below — read the one you pick from
> `references/palettes/<file>`. If none fits, invent a custom palette
> using the same CSS variable names."

Then list the table above so `frontend-design` can make an informed choice.

## Wildcard mode

When the user picks **"wildcard"**, deliberately diverge — the goal is novelty, not
fit. Do NOT read a palette file. Pass this instruction to `frontend-design`:

> "Invent a fresh, cohesive palette using the same CSS variable names as our
> catalog, paired with a display/body/mono font trio you have not used recently. Do
> not default to Community Purple. Choose an unexpected mood that still serves the
> narrative's audience."

Vary the result on every run — the point of this mode is to never converge on the
same look twice.
