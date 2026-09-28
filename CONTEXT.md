# CONTEXT.md — motopage skill

> Rewritable snapshot of the whole project. Read at the start of every session; refresh after any major decision or status change.

## What this is
A **Claude Code Skill** (`SKILL.md` + reference files) that loads design-craft guidance whenever Claude builds or reviews a frontend. It teaches four things:
1. **Visual aesthetics** — typography, color, spacing, motion, anti-"AI-slop", WCAG AA.
2. **Design tokens & theming** — token architecture, light/dark, brand adaptation.
3. **Component patterns** — robust component anatomy, all states, layout primitives.
4. **Design critique** — a scoring rubric to audit any page and give actionable feedback.

## Why it exists (gap it fills)
Sits alongside existing skills without overlapping:
- `frontend-dev` → scaffolding & coding conventions (defer to it).
- `landing-page` → copy & conversion structure (defer to it).
- `artifact-design` → artifact-specific design.
- `dataviz` → charts.
- **`motopage` (this) → visual design craft, tokens/theming, component design, critique.**

## Origin / inspiration
Seeded by a teardown of **themotoacademy.com** (analyzed 2026-09-28), distilled into 15 named, reusable patterns (see `references/patterns.md`). The teardown covered imagery, counters, offer/option display, and layout.

## Key decisions
- Repo: https://github.com/Jarrydcoetz/motopage — **public**, owner `Jarrydcoetz`.
- Skill name (folder + frontmatter): `motopage` (renamed from `frontend-design` 2026-09-28).
- Local working dir still `/Users/jarryd/frontend-design` during build; rename to `motopage` in Phase C after agents finish.
- Progressive disclosure: `SKILL.md` stays short, routes to `references/*`.
- Ships **zero stock images** (golden rule: no self-sourced images). Examples use CSS/gradients/placeholders only.
- Install target: `~/.claude/skills/motopage`.

## Structure
```
SKILL.md                       entry point + routing
references/aesthetics.md       type, color, spacing, motion, a11y
references/tokens-theming.md   token architecture, light/dark, brand
references/components.md        component anatomy, states, layout
references/patterns.md          the 15 teardown patterns
references/critique.md          review rubric + scoring
examples/pattern-gallery.html  standalone demo, no images
```

## Status
**Round 1 shipped (2026-09-28).** All deliverables written, verified, committed, and pushed to `main`; skill symlinked into `~/.claude/skills/motopage`. See `TODO.md` for the ticked checklist with evidence.

Verification highlights: all cross-links resolve; gallery is fully self-contained (no external images/CDN/fonts); WCAG AA passes in both themes (body 16:1, muted 6.6–8.1:1, accents 6.2–7.8:1). Browser render check was blocked by the sandbox loopback restriction; verified statically instead.

Open question for next round: confirm scope — general 4-pillar design skill (current) vs. a narrower "build moto-style high-converting landing pages" skill.
