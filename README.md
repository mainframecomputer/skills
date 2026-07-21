# Mainframe skills

Agent skills from the [Mainframe](https://github.com/mainframecomputer) team, in the
[Claude Code skill format](https://code.claude.com/docs/en/skills) (a `SKILL.md` per
skill, plus any scripts it bundles).

## Skills

- **[review-as](skills/review-as/SKILL.md)** — learn how a GitHub user reviews code
  from a year of their real review comments, then review a change as that person.
  Works for your own style ("review this as me") or a teammate's ("how would
  `<login>` review this PR?"). The review stays in chat — nothing is ever posted to
  GitHub.

## Using a skill

Copy (or symlink) a skill's folder into your agent's skill directory — for Claude
Code, `~/.claude/skills/` for personal use or `.claude/skills/` in a project.

Bundled scripts run with [bun](https://bun.sh) and install their own pinned
dependencies on first run — no setup step. `review-as` additionally needs an
authenticated [GitHub CLI](https://cli.github.com) (`gh auth login`).

## License

[MIT](LICENSE)
