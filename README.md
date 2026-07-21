# Mainframe skills

Agent skills from the [Mainframe](https://github.com/mainframecomputer) team, in the
open [Agent Skills format](https://agentskills.io) (a `SKILL.md` per skill, plus any
scripts it bundles) — usable from Claude Code, Codex, and other skill-aware agents.

## Skills

- [review-as](#review-as) — review a change the way you — or any teammate — would,
  learned from real review comments.

## review-as

Learn how a GitHub user reviews code from up to a year of their real review comments,
then review a change as that person ([SKILL.md](skills/review-as/SKILL.md)). Works for
your own style ("review this as me") or a teammate's ("how would `<login>` review this
PR?"). It predicts what someone would likely flag — it never impersonates them
anywhere, and the review stays in chat.

What it reads and writes: it reads the reviewer's visible GitHub review activity
through your authenticated `gh` token (which can include private repositories the
token can see), writes the raw history to a temporary file, and distills a profile
into the repository under review (`.claude/review-as/…`, committable and shareable
with your team) or into `~/.claude/review-as/` for the cross-repo scope. It never
posts to GitHub and never modifies the change under review.

### Example profile

What review-as produces: the real profile it built for
[@vincentmvdm](https://github.com/vincentmvdm) from a year of review history in
`mainframecomputer/async`, written to
`.claude/review-as/vincentmvdm/mainframecomputer--async.md` in that repo and
committed, so the whole team can review as him. Reviews then surface findings this
profile says he would raise, worded the way he would raise them.

```markdown
---
login: vincentmvdm
scope: mainframecomputer/async
evidence_through: 2026-07-21
evidence: 147 review submissions (146 approve, 1 comment, 0 request-changes), 525 inline review comments, 537 PR comments across 373 PRs in mainframecomputer/async; window 2025-07-21 → 2026-07-21 (activity spans 2026-03-22 → 2026-07-21). ~60% of raw items were automation posted under the login (@cursor review / bugbot / Graphite / video-bot triggers, agent-drafted thread replies) and were excluded from style evidence.
---

# How @vincentmvdm reviews

## What they always flag
- **Necessity first.** The single most common move: a short question challenging whether a line, dep, param, config, or the whole PR needs to exist — "is this needed?", "what's the value?", "why is this here?" (~50+ instances across every era).
- **Defensive hedging and optionality.** Fallbacks, `try`-swallowing, optional/nullable fields without justification — "fallbacks are evil", "why optional if it's actually required?", "why do we hedge?" (~20).
- **Illegal states.** Pushes discriminated unions, Zod-first schemas, grouped props over parallel nullables — "should this be a discriminated union y/n?" (~25).
- **Security and tenancy probing.** Who can call this, can a caller lie, is it org-scoped, "can someone set this to something nefarious or we gucci?"; asks for `/security-review` as a non-blocking follow-up on sensitive PRs (~26).
- **Canonical over clever.** Demands the approach match upstream docs / industry standard, not repo folklore — "is this the canonical way? double check the official docs" (~15).
- **Scope hygiene.** Irrelevant diffs get "undo changes to <file>, completely irrelevant"; "can we please just limit this pr to actual relevant changes" (~12).
- **Consistency with siblings.** New surfaces must work "*exactly* the same way" as the nearest existing feature (statuses, routes, ids, shared UI primitives, env-var names) (~20).
- **LLM slop and comment bloat.** "jumbo comment lol", "very llm generated", "Needed or just LLM bs?" — wants comments cut, plan-language scrubbed (~19).
- **`useEffect` challenges** with the react.dev "you might not need an effect" URL pasted bare (~10).
- **Evidence demands.** "can you record a video of this actually working?", "did we run an eval to prove it e2e?"; dev/test paths must mirror prod, no mock-heavy tests (~10).

## Severity and disposition
Effectively never blocks: 146 of 147 submissions are APPROVED (almost all empty-body), 1 COMMENTED (asking for video evidence), 0 CHANGES_REQUESTED. Approves even while raising real concerns — severity lives inside the comments, not the review state: "nit:", "non-blocking", "no need to address", "don't feel strongly" for the soft end; hard challenges arrive as blunt inline questions, mostly aimed at agent-written code. Sensitive changes get an approve plus "mind running a security review?".

## Voice
- Very short: 5–25 words, one thought per comment, fired in bursts (10 comments in ~5 minutes is normal).
- Overwhelmingly interrogative — even firm opinions end in "?", "or nah?", "or am i nuts?", "y/n?". Labels intent: "(genuine q)", "(non leading q)", "lazy question:", "nit:".
- Lowercase, typo-tolerant, slangy: "jank", "footgun", "gucci", "PITA", "idk", "wyd", "lol"; escalates to blunt with agents ("bullshit defensive hedging", "PLEASE") but heat targets code shape, not people.
- Almost no markdown or code blocks; argues by pasting a bare doc URL; signature verbs: "triple check", "double check".
- Paraphrased: "is this the canonical way? double check"; "why optional here (and elsewhere)?"; "we have this exact code somewhere else — move to shared?"

## Gaps
- No evidence of what a formal block looks like — they have never requested changes here; wording of a hard veto is extrapolation.
- Most review volume is their reviewing agent-written code on their own PRs; peer review of human teammates is a small minority (early 2026-03/04 era).
- Rarely comments on performance, accessibility, animation/design detail, or test internals; visual polish critique is largely absent from the record.
- Earliest month of history is ~98% bot triggers and contributes no style signal.
```

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
dependencies on first run — no setup step beyond having bun installed. `review-as`
additionally needs an authenticated [GitHub CLI](https://cli.github.com)
(`gh auth login`) and works against GitHub.com only — GitHub Enterprise hosts are not
supported yet.

## License

[MIT](LICENSE)
