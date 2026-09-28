# motopage

A [Claude Code](https://claude.com/claude-code) **skill** for frontend *design craft* — the layer above "does it work": making interfaces that look intentional, distinctive, and accessible. Seeded by a teardown of a real high-converting site, distilled into reusable patterns.

It covers four things:

1. **Visual aesthetics** — typography, color, spacing, motion, and how to avoid generic "AI-slop" UI. → [`references/aesthetics.md`](references/aesthetics.md)
2. **Design tokens & theming** — token architecture, light/dark, brand adaptation. → [`references/tokens-theming.md`](references/tokens-theming.md)
3. **Component patterns** — robust component anatomy, every state, layout primitives. → [`references/components.md`](references/components.md)
4. **Design critique** — a scoring rubric to audit any page and give actionable feedback. → [`references/critique.md`](references/critique.md)

Plus a library of **15 named, reusable patterns** distilled from real high-converting sites. → [`references/patterns.md`](references/patterns.md)

## How it relates to other skills

| Skill | Owns |
|---|---|
| `frontend-dev` | Project scaffolding & coding conventions |
| `landing-page` | Copywriting & conversion structure |
| `artifact-design` | Design *inside* Claude artifacts |
| `dataviz` | Charts & data visualization |
| **`motopage`** | **Visual craft, tokens/theming, component design, critique** |

Use `motopage` when the question is *"how should this look and feel?"* — not how to wire it up (that's `frontend-dev`) or what the words should say (that's `landing-page`).

## Install

Copy or symlink this repo into your personal Claude Code skills directory:

```bash
# symlink (keeps it updated as you pull)
ln -s "$(pwd)" ~/.claude/skills/motopage

# or copy
cp -R . ~/.claude/skills/motopage
```

Claude Code discovers it automatically on the next session. It triggers on requests like "design this page," "make it look less generic," "set up design tokens," "review this UI."

## Structure

```
SKILL.md                       entry point + routing
references/
  aesthetics.md                type, color, spacing, motion, a11y
  tokens-theming.md            token architecture, light/dark, brand
  components.md                 component anatomy, states, layout
  patterns.md                  the 15 patterns
  critique.md                  review rubric + scoring
examples/
  pattern-gallery.html         standalone demo (no images)
```

## License

MIT
