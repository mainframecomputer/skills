# Mainframe skills

Agent skills from the [Mainframe](https://github.com/mainframecomputer) team, in the
[Claude Code skill format](https://code.claude.com/docs/en/skills) (a `SKILL.md` per
skill, plus any scripts it bundles).

## Skills

- **[review-as](skills/review-as/SKILL.md)** — learn how a GitHub user reviews code
  from up to a year of their real review comments, then review a change as that
  person. Works for your own style ("review this as me") or a teammate's ("how would
  `<login>` review this PR?"). It predicts what someone would likely flag — it never
  impersonates them anywhere, and the review stays in chat.

  See a [real example profile](examples/review-as/vincentmvdm/mainframecomputer--async.md)
  distilled from a year of review history — the shape every profile follows.

  What it reads and writes: it reads the reviewer's visible GitHub review activity
  through your authenticated `gh` token (which can include private repositories the
  token can see), writes the raw history to a temporary file, and distills a profile
  into the repository under review (`.claude/review-as/…`, committable and shareable
  with your team) or into `~/.claude/review-as/` for the cross-repo scope. It never
  posts to GitHub and never modifies the change under review.

## Using a skill

In Claude Code, install through the plugin marketplace:

```text
/plugin marketplace add mainframecomputer/skills
/plugin install review-as@mainframe-skills
```

With any agent supported by the [skills CLI](https://skills.sh):

```sh
npx skills add mainframecomputer/skills
```

Or copy (or symlink) a skill's folder into your agent's skill directory — for Claude
Code, `~/.claude/skills/` for personal use or `.claude/skills/` in a project.

Bundled scripts run with [bun](https://bun.sh) and install their own pinned
dependencies on first run — no setup step. `review-as` additionally needs an
authenticated [GitHub CLI](https://cli.github.com) (`gh auth login`).

## License

[MIT](LICENSE)
