# claude-code-skills

Personal [Claude Code](https://claude.com/claude-code) skills I wrote and use day to day.

A skill is a folder with a `SKILL.md` — YAML frontmatter (`name`, `description`) plus instructions Claude loads on demand when the task matches the description.

## Skills

| Skill | What it does |
|---|---|
| [`web-ui-design`](skills/web-ui-design) | My web design system and taste. Calm, premium fintech aesthetic — depth over flat, strict token discipline, tight geometry, color as accent not flood, honest states, light + dark. For Nuxt/Vue, React/Next, Tailwind, NuxtUI/shadcn, plain HTML/CSS. |
| [`mobile-ui-design`](skills/mobile-ui-design) | The native-mobile counterpart. Android Jetpack Compose now, SwiftUI later — the "calm card", honest affordance states, and the native substitutes for web-only effects. |
| [`ui-implement`](skills/ui-implement) | Builds UI *inside an existing product* so it matches the established aesthetic and covers every real-world state (loading, empty, error, overflow, permission). The opposite of greenfield design work. |
| [`presentation-maker`](skills/presentation-maker) | Generates project presentations — pitch decks, system introductions, investor decks — as HTML with optional PPTX export. |
| [`insurance-preview`](skills/insurance-preview) | Turns insurance product risk/coverage documents (.docx, HTML tables) into self-contained, responsive card-based HTML previews. No JavaScript — pure `<details>`/`<summary>`, so it renders inside a sandboxed iframe. |

## Install

Clone into your user skills directory:

```bash
git clone https://github.com/Sumyabazar00/claude-code-skills.git /tmp/claude-code-skills
cp -r /tmp/claude-code-skills/skills/* ~/.claude/skills/
```

Or symlink a single skill so it stays updatable:

```bash
git clone https://github.com/Sumyabazar00/claude-code-skills.git ~/src/claude-code-skills
ln -s ~/src/claude-code-skills/skills/web-ui-design ~/.claude/skills/web-ui-design
```

Project-scoped instead of user-scoped: use `.claude/skills/` inside the repo.

## Notes

`web-ui-design` and `mobile-ui-design` encode my personal taste, not a neutral style guide. Fork and edit the tokens rather than expecting them to match your brand.

`mobile-ui-design` previously lived in its own repo (`Sumyabazar00/mobile-ui-design`), now archived — this repo is its home.

## License

MIT
