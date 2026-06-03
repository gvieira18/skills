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

## Auto mode

When the user picks **"auto"**, do NOT read any palette file yet. Instead,
pass this instruction to `frontend-design`:

> "Choose the palette that best serves this narrative's mood and audience.
> Available palettes are listed below — read the one you pick from
> `references/palettes/<file>`. If none fits, invent a custom palette
> using the same CSS variable names."

Then list the table above so `frontend-design` can make an informed choice.
