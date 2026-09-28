---
name: motopage
description: >-
  Frontend design craft — visual aesthetics, design tokens & theming, component
  patterns, and design critique. Use when the task is how an interface should
  LOOK and FEEL: designing or restyling a page or component, making UI feel less
  generic / "AI-slop", choosing typography, color, spacing or motion, setting up
  design tokens or light/dark theming, designing robust component states, or
  reviewing/critiquing an existing frontend's visual design. For project
  scaffolding and coding conventions use frontend-dev; for landing-page copy and
  conversion structure use landing-page; for charts use dataviz.
---

# motopage — frontend design craft

Design craft is the layer above "does it work." Working-but-generic is the
default failure mode of AI-built UI: safe fonts, purple gradients, evenly gray
everything, no point of view. This skill exists to push past that toward
interfaces that are **intentional, distinctive, and accessible**.

## When to use this vs. sibling skills

| If the request is about… | Use |
|---|---|
| How it should look/feel; restyle; "make it nicer"; critique a UI | **motopage** (this) |
| Scaffolding, stack, file structure, coding conventions | `frontend-dev` |
| Landing-page copy, section order, conversion structure | `landing-page` |
| Charts, graphs, dashboards, data viz color | `dataviz` |
| Design *inside* a Claude artifact | `artifact-design` |

These compose — a landing page often needs `landing-page` (copy) **and**
`motopage` (visual craft). Load both.

## Core principles

1. **Distinctiveness over defaults.** Every generic choice (system font stack,
   a blue→purple gradient, `#f3f4f6` cards, uniform border radius everywhere) is
   a signal that no one decided anything. Make deliberate, ownable choices.
2. **Color carries meaning.** Reserve accent colors for roles (affirmative
   action vs. urgency vs. neutral). An accent used everywhere means nothing.
3. **Type does the heavy lifting.** A strong display/body pairing creates more
   personality than any decoration. Contrast in weight, size, and width.
4. **Rhythm beats density.** Consistent spacing scale + a repeatable section
   skeleton makes dense information feel calm and scannable.
5. **Motion rewards, never distracts.** Subtle, purposeful, respects
   `prefers-reduced-motion`.
6. **Accessibility is a constraint, not a phase.** WCAG AA contrast, visible
   focus, real states, keyboard paths — designed in from the start.

## How to work

**Building or restyling?** Establish tokens first, then aesthetics, then
components — in that order, so decisions cascade instead of fighting.

1. **Tokens & theming** → [`references/tokens-theming.md`](references/tokens-theming.md)
   Set primitive → semantic → component tokens and the light/dark contract
   before styling anything.
2. **Aesthetics** → [`references/aesthetics.md`](references/aesthetics.md)
   Type pairing, color roles, spacing scale, motion, the anti-AI-slop checklist.
3. **Components** → [`references/components.md`](references/components.md)
   Component anatomy, every state (hover/focus/disabled/loading/empty/error),
   layout primitives.
4. **Patterns** → [`references/patterns.md`](references/patterns.md)
   15 named, reusable patterns (hero scrims, stat rows, comparison tables,
   pricing ladders, segmented controls, working footers, and more) — each with
   when/how/accessibility/misuse.

**Reviewing or critiquing?** Go straight to
[`references/critique.md`](references/critique.md) — a scoring rubric that turns
the principles and patterns into an actionable, evidence-based audit.

**Want to see the patterns rendered?**
[`examples/pattern-gallery.html`](examples/pattern-gallery.html) is a
self-contained demo (light + dark, zero external images).

## Non-negotiables (check before you call it done)

- [ ] Text meets **WCAG AA** contrast (4.5:1 body, 3:1 large/UI).
- [ ] Every interactive element has a **visible focus state**.
- [ ] Interactive components handle **all states**, not just the happy path.
- [ ] Dark mode (if present) is defined via tokens, not one-off overrides.
- [ ] Motion respects **`prefers-reduced-motion`**.
- [ ] No unlicensed / self-sourced images — use only what the user provides.
