# Design Critique — review rubric & scoring

Use this to audit an existing frontend and return **actionable, evidence-based**
feedback instead of vague taste opinions. Every finding must name *what*, *where*,
*why it matters*, and *the fix*.

## How to run a critique

1. **Look before scoring.** Screenshot or read the page at desktop **and** mobile
   width. Note first impression in one sentence before analyzing (it fades fast).
2. **Score each of the 8 dimensions** below 0–3.
3. **Attach evidence** to every score below 3: the specific element + the rule
   it breaks.
4. **Rank fixes** by impact × effort. Lead with the 3 highest-leverage changes.
5. **Never rewrite everything.** Respect existing intent; propose the minimum
   changes that move the needle most.

## Scoring scale (per dimension)

| Score | Meaning |
|---|---|
| 0 | Broken / absent — actively hurts the experience |
| 1 | Generic default — works but says nothing, "AI-slop" |
| 2 | Solid — intentional and correct, not memorable |
| 3 | Distinctive — deliberate, ownable, accessible |

**Total /24.** 0–8 rebuild the foundation · 9–15 refine · 16–20 polish · 21–24 ship.

## The 8 dimensions

### 1. Typography (0–3)
- Is there a real display/body pairing, or one default sans doing everything?
- Meaningful contrast in size/weight/width between levels?
- Line length 45–75ch for body? Line-height comfortable (~1.5 body)?
- **Slop tells:** system-ui everywhere; every heading the same weight; no scale.

### 2. Color & meaning (0–3)
- Do accents map to *roles* (affirm / urgency / info) or decorate randomly?
- Is there a restrained palette, or many unrelated hues?
- **Slop tells:** blue→purple gradient; one accent used for everything; pure
  `#000`/`#fff` with no considered neutrals.

### 3. Contrast & accessibility (0–3)
- Body text ≥ 4.5:1, large/UI ≥ 3:1 (verify, don't eyeball).
- Visible, non-default focus states on every interactive element?
- States distinguishable without color alone?
- **Auto-cap the total at 15 if any body text fails AA** — accessibility is a gate.

### 4. Spacing & rhythm (0–3)
- Consistent spacing scale, or arbitrary pixel values?
- Generous, intentional whitespace vs. cramped or aimlessly empty?
- Consistent max-width content column?
- **Slop tells:** everything 16px apart; no grouping via proximity.

### 5. Layout & hierarchy (0–3)
- Clear focal point per section; one dominant CTA?
- Repeatable section skeleton (eyebrow → headline → sub → action)?
- Alignment and grid discipline?

### 6. Components & states (0–3)
- Do interactive elements have hover/focus/active/disabled/loading/empty/error?
- Consistent radius/shadow/border language across components?
- Touch targets ≥ 44px?

### 7. Motion & interaction (0–3)
- Purposeful transitions (feedback, continuity) vs. decorative jitter?
- `prefers-reduced-motion` honored?
- Nothing blocks or delays the user unnecessarily.

### 8. Distinctiveness (0–3)
- Would you recognize this brand with the logo removed?
- Any ownable signature (a shape, a motion, a type treatment)?
- **This is the anti-slop dimension** — the whole reason the skill exists.

## Pattern-coverage check

Cross-reference [`patterns.md`](patterns.md): for the page's goal, which of the 15
patterns apply, and are they present + well-executed? (e.g. a pricing page with no
comparison table or good/better/best ladder is leaving conversion on the table.)

## Output format

```
First impression: <one sentence>
Score: NN/24

Top 3 fixes (impact × effort):
1. <what> — <where> — <why> — <fix>
2. ...
3. ...

By dimension:
Typography 2/3 — <evidence + fix>
Color 1/3 — ...
[...all 8...]

Patterns: present <list> · missing-but-relevant <list>
```
