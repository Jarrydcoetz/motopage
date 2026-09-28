# TODO — motopage skill (round 1)

One checkbox per deliverable. Tick with evidence; never delete.

## Phase A — scaffold (serial)
- [x] Create public GitHub repo — https://github.com/Jarrydcoetz/motopage (renamed from frontend-design)
- [x] `CONTEXT.md` written
- [x] `TODO.md` written
- [x] `README.md` — what it is + install instructions (56 lines)
- [x] `SKILL.md` skeleton — frontmatter + core principles + routing (83 lines)
- [x] `references/critique.md` — review rubric + scoring (98 lines, 8-dimension /24 rubric)

## Phase B — parallel sub-agents (non-overlapping files)
- [x] `references/aesthetics.md` (Agent 1) — 400 lines
- [x] `references/tokens-theming.md` (Agent 2) — 316 lines
- [x] `references/components.md` (Agent 3) — 399 lines
- [x] `references/patterns.md` + `examples/pattern-gallery.html` (Agent 4) — 648 + 686 lines

## Phase C — integrate (serial)
- [x] Wire SKILL.md routing to all references (SKILL.md links all 5 references + gallery)
- [x] Reconcile cross-references between files (all relative .md/.html links resolve — verified)
- [x] Rename local working dir + repo to `motopage`

## Checks / verification
- [x] SKILL.md frontmatter validates (name: motopage, description shape correct)
- [x] Every `references/*` link in SKILL.md resolves to a real file (grep check: none missing)
- [x] `pattern-gallery.html` self-contained — no external images/CDN/fonts (grep: NONE); tags balanced, doctype/title present
- [x] WCAG AA contrast spot-check passes — body 16:1 both themes; muted 6.6–8.1:1; accents 6.2–7.8:1 (all ≥ AA)
- [x] Critique rubric present & coherent (8 dims, evidence-required output format); cross-links patterns.md
- [x] Skill installed to `~/.claude/skills/motopage` and listed
- [x] Initial commit pushed; repo clean

Note: browser render check blocked by sandbox loopback restriction; verified statically instead (stronger for contrast — computed real WCAG ratios).
