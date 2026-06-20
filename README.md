# skills

Custom agent skills for coding assistants.

Supports **Claude Code**, **Codex**, **Cursor**, **Gemini CLI**, **OpenCode**, and [67 more](https://github.com/vercel-labs/skills#supported-agents).

## Install

```bash
npx skills add gvieira18/skills
```

## Available Skills

| Skill | Description |
|-------|-------------|
| [artifact](skills/artifact/) | Generate self-contained HTML artifacts for visual communication — code flows, architecture diagrams, data dashboards, explainers, presentations |
| [commit-it](skills/commit-it/) | Commit changes using Conventional Commits with atomic, single-concern commits grouped by logical context |
| [php-checks](skills/php-checks/) | Run Rector, Pint, PHPStan, and Pest on Laravel projects — auto-fix everything, fail on what remains |
| [ship](skills/ship/) | Branch, commit, push, and open a pull request in one command |
| [waifu-it](skills/waifu-it/) | Upload a local file to WaifuVault and return a shareable link |

## Install a Specific Skill

```bash
npx skills add gvieira18/skills --skill artifact
```

## Install to a Specific Agent

```bash
npx skills add gvieira18/skills -a claude-code
npx skills add gvieira18/skills -a cursor
npx skills add gvieira18/skills -a codex
```

## Customizing the Palette

The `artifact` skill ships with a default dark deep-blue + violeta palette in
[`skills/artifact/references/palette.md`](skills/artifact/references/palette.md).

To use your own colors: replace that file keeping the same CSS variable names
(`--bg`, `--brand`, `--add`, `--del`, etc.). The taxonomy and visual tempero
references in `art-direction.md` use those variables, so they'll automatically
pick up your palette.

## Creating Skills

Each skill is a directory under `skills/` containing a `SKILL.md` with YAML frontmatter:

```markdown
---
name: my-skill
description: What this skill does and when to use it
---

# My Skill

Instructions for the agent to follow.
```

See the [Agent Skills specification](https://agentskills.io) for the full format.

## License

MIT
