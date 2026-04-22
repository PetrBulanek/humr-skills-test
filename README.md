# humr-skills-test

Scratch repository for exercising the [Humr](https://github.com/PetrBulanek) skills marketplace workflow (install, pin, uninstall, agent-callable MCP tools).

Not a curated library — expect churn.

## Layout

```
skills/
  <skill-name>/
    SKILL.md
```

Each skill is a directory under `skills/` containing a `SKILL.md` with YAML frontmatter (`name`, `description`) followed by the skill body.

## Skills

- [`add-changelog-entry`](skills/add-changelog-entry/SKILL.md) — append an entry to the `[Unreleased]` section of `CHANGELOG.md`.
