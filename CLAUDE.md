# Gabriel's Skills Collection

Custom agent skills for coding assistants (Claude Code, Cursor, Codex, Gemini CLI, etc.).

## Project Structure

```
.claude-plugin/plugin.json   # Plugin manifest — registers all skills
skills/
  <skill-name>/
    SKILL.md                  # Required: YAML frontmatter + markdown instructions
    references/               # Optional: supporting docs, templates, resources
```

## Adding a New Skill

1. Create `skills/<name>/SKILL.md` with YAML frontmatter (`name`, `description`)
2. Register it in `.claude-plugin/plugin.json` under the `skills` array
3. Add a row to the "Available Skills" table in `README.md`
4. Commit with `feat(<name>): add <name> skill`

## Conventions

- Skills are pure markdown instruction sets — no code, no scripts
- Factor reusable or verbatim-passed content into a single `references/<topic>.md` file and point to it from SKILL.md — don't inline it (e.g. `artifact`'s palettes, art-direction, guardrails)
- SKILL.md frontmatter uses `name` and `description` fields
- Commit messages follow Conventional Commits with the 50/72 rule
- Skill names are lowercase kebab-case
- The `commit-it` skill is the source of truth for commit quality — other skills delegate to it rather than reimplementing commit logic
