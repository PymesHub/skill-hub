# skill-hub

Agent Skills for Claude Code, Codex, OpenCode and any client that follows the [Agent Skills spec](https://agentskills.io/specification).

## Skills

| Skill | What it does |
|---|---|
| [blast-radius](skills/blast-radius/SKILL.md) | Before editing code, rates what the change can break (high/medium/low) from evidence — git + grep only — and waits for your go. |

## Install

With [skills.sh](https://github.com/vercel-labs/skills):

```sh
npx skills add PymesHub/skill-hub
```

Manual: copy `skills/<name>/` into `~/.claude/skills/` (Claude Code), `~/.agents/skills/` (Codex) or `.opencode/skills/` (OpenCode).

## Layout

```
skills/
  <name>/
    SKILL.md   # frontmatter: name (= folder name), description (≤1024 chars), license
```

## Adding a skill

1. Create `skills/<name>/SKILL.md`; `name` lowercase, hyphens only, matches the folder.
2. `description` says what it does and when to use it, under 1024 characters.
3. Add a row to the table above.

## License

[MIT](LICENSE)
