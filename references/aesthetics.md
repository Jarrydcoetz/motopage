# Aesthetics — type, color, spacing, motion

The look-and-feel layer. This file is how you make the visual choices that
separate a deliberate interface from generic "AI-slop." Establish tokens first
([`tokens-theming.md`](tokens-theming.md)), then use this file to decide what the
values actually are, then wire up states in
[`components.md`](components.md).

The through-line: **every default is a decision you didn't make.** System font,
blue→purple gradient, `#f3f4f6` cards, 16px between everything — each says no one
had a point of view. Pick on purpose.

---

## 1. Typography

Type is the cheapest way to look intentional and the fastest tell when you don't.
A strong display/body pairing carries more personality than any gradient or
shadow.

### Picking a pairing

Aim for **one display face** (headings, hero, numerals with attitude) and **one
body face** (paragraphs, UI, labels). Two families is usually enough; three is a
ceiling, not a target.

Choose for **contrast**, not similarity — the pairing should differ on at least
one clear axis:

| Axis | Display | Body |
|---|---|---|
| Classification | Serif / condensed / geometric | Humanist sans |
| Weight | Heavy (600–800) | Regular (400–450) |
| Width | Condensed / expanded | Normal |
| Mood | Expressive, characterful | Neutral, legible |

Rule of thumb: **contrast the classification, harmonize the proportions.** A serif
display over a humanist sans works because they share comfortable x-heights; two
faces that are almost-but-not-quite the same just look like a mistake.

### Concrete pairings (all free via Google Fonts)

**1. Editorial / authoritative**
- Display: **Fraunces** (700, optical size high) — a "soft-serif" with real character.
- Body: **Inter** (400/500) — neutral, superb at small sizes.
- Why: expressive serif headlines over a workhorse UI sans; feels like a
  considered publication, not a template.

**2. Modern / technical**
- Display: **Space Grotesk** (500/700) — geometric with quirky details.
- Body: **IBM Plex Sans** (400/500) — humanist, engineered, wide language coverage.
- Numerals: **IBM Plex Mono** for data/code/stats.
- Why: reads as product-y and precise without defaulting to Helvetica-blandness.

**3. Warm / human**
- Display: **Bricolage Grotesque** (600/800) — friendly, slightly irregular.
- Body: **Source Sans 3** (400/450) — calm, highly legible.
- Why: approachable and distinctive; good for consumer apps and marketing.

> Prefer **variable fonts** where available — one file, all weights, and you can
> tune optical size / weight precisely. Subset to the weights you actually use.

### A modular type scale

Don't hand-pick sizes. Generate them from a **base × ratio** so relationships are
consistent. Body base **1rem (16px)**; a **1.25 (major third)** ratio reads well
for UI, **1.333 (perfect fourth)** for more editorial drama.

Scale below uses **1.25**, rounded to clean rems:

| Token | rem | ~px | Use |
|---|---|---|---|
| `xs` | 0.8 | 13 | captions, legal, overlines |
| `sm` | 0.875 | 14 | secondary UI, labels |
| `base` | 1 | 16 | body |
| `md` | 1.25 | 20 | lead paragraph, small headings |
| `lg` | 1.563 | 25 | h3 |
| `xl` | 1.953 | 31 | h2 |
| `2xl` | 2.441 | 39 | h1 |
| `3xl` | 3.052 | 49 | display / hero |

```css
:root {
  --text-xs: 0.8rem;   --text-base: 1rem;   --text-lg: 1.563rem;
  --text-sm: 0.875rem; --text-md: 1.25rem;  --text-xl: 1.953rem;
  --text-2xl: 2.441rem; --text-3xl: 3.052rem;
}
```

For fluid hero type, clamp between a mobile and desktop bound instead of a media
query:

```css
h1 { font-size: clamp(2.441rem, 1.8rem + 3.2vw, 3.815rem); }
```

### Weight, size, and width contrast

- **Establish hierarchy with more than size.** Jumping only in size looks timid.
  Pair a size jump with a weight jump (e.g. body 400 → h2 700) and/or a color/case
  change.
- **Don't give every heading the same weight** — that flattens hierarchy. A
  common tell of generic UI.
- **Condensed / uppercase** display (with `letter-spacing: 0.02em–0.08em`) suits
  eyebrows, overlines, and punchy hero labels. Uppercase needs tracking added
  back; never set uppercase at default tracking.
- **Humanist body** for anything read in sentences. Never set body copy in a
  condensed or display face.

### Line length & line-height

| Property | Target |
|---|---|
| Body line length | **45–75 characters** (`max-width: 65ch` is a safe default) |
| Body line-height | **1.5–1.65** |
| Heading line-height | **1.05–1.2** (tighter as size grows) |
| Letter-spacing (large display) | slightly negative, `-0.01em` to `-0.02em` |
| Letter-spacing (uppercase/small caps) | positive, `0.04em`–`0.08em` |

```css
.prose { max-width: 65ch; line-height: 1.6; }
h1, h2 { line-height: 1.1; letter-spacing: -0.02em; }
.overline { text-transform: uppercase; letter-spacing: 0.08em; font-size: var(--text-sm); }
```

---

## 2. Color

Color is meaning, not decoration. An accent used everywhere means nothing. Reserve
hue for **roles**, build everything else from a neutral ramp.

### Accent roles (semantic, not literal)

Name colors by **what they do**, not what they are (`--color-primary`, not
`--color-blue`). The literal→semantic→component mapping lives in
[`tokens-theming.md`](tokens-theming.md) — this is about which roles to define:

| Role | Purpose | Convention |
|---|---|---|
| **Primary / action** | The one main CTA, links, focus | Your brand accent |
| **Affirmative / success** | Confirmations, positive state | Green family |
| **Urgency / danger** | Destructive actions, errors | Red family |
| **Warning / caution** | Needs attention, not blocking | Amber family |
| **Info / neutral** | Passive notices | Blue/slate family |

Keep decorative accents distinct from these functional roles — if your brand
accent is green, pick a *different* green (or a different treatment) for "success"
so a green button doesn't read as a success message.

### Build from a neutral ramp + 1–2 accents

Most of a good interface is neutrals. The recipe:

1. **One neutral ramp**, 10–12 steps from near-white to near-black. Give it a
   subtle temperature (a hint of warm or cool) — pure gray is the "AI-slop" tell.
2. **One primary accent** with its own light→dark ramp for hover/active/subtle-bg.
3. **At most one secondary accent** for contrast/highlight.
4. **Functional colors** (success/danger/warning) — muted, not neon; they appear
   rarely and shouldn't scream.

### Avoid pure black and white

Pure `#000` on `#fff` vibrates and feels cheap. Use **near-black** text on
**off-white** backgrounds; tint them toward your accent's temperature for
cohesion.

### Worked example — a cool-slate palette

```css
:root {
  /* Neutral ramp (slightly cool) */
  --n-50:  #f8fafc;  /* off-white page bg   */
  --n-100: #eef2f6;
  --n-200: #dde3ea;
  --n-300: #c2ccd6;
  --n-500: #6b7785;  /* muted text          */
  --n-700: #384250;
  --n-900: #171c24;  /* near-black text      */

  /* Primary accent (indigo) + states */
  --accent:        #4f46e5;
  --accent-hover:  #4338ca;
  --accent-subtle: #eef0fe;  /* tinted bg for badges/callouts */

  /* Functional (muted) */
  --success: #15803d;
  --danger:  #b91c1c;
  --warning: #b45309;

  /* Surfaces & text */
  --bg:      var(--n-50);
  --fg:      var(--n-900);
  --fg-muted:var(--n-500);
}
```

Guidelines this palette follows: one tinted neutral ramp, a single distinctive
accent with hover/subtle variants, functional colors muted rather than saturated,
and no pure `#000`/`#fff`. Contrast: `--n-900` on `--n-50` ≈ 15:1, `--accent` on
white ≈ 6.9:1 — both pass AA (see §6).

> **Never** ship the blue→purple gradient. If you want depth, use a subtle
> single-hue gradient (accent → a darker/lighter shade of the *same* hue), a
> mesh/noise texture, or a duotone — anything with a point of view.

---

## 3. Spacing & rhythm

Consistent spacing is what makes dense information feel calm. Derive all spacing
from **one base unit** — never arbitrary pixel values.

### Spacing scale (4px base)

A `4px` base with a roughly-geometric progression covers every gap you need:

| Token | px | rem | Typical use |
|---|---|---|---|
| `1` | 4 | 0.25 | icon–label gap, tight insets |
| `2` | 8 | 0.5 | within a component |
| `3` | 12 | 0.75 | between related items |
| `4` | 16 | 1 | default element gap |
| `6` | 24 | 1.5 | between groups |
| `8` | 32 | 2 | card padding, small sections |
| `12`| 48 | 3 | section padding (mobile) |
| `16`| 64 | 4 | section padding (desktop) |
| `24`| 96 | 6 | major section breaks |

```css
:root {
  --space-1: 0.25rem; --space-2: 0.5rem;  --space-3: 0.75rem;
  --space-4: 1rem;    --space-6: 1.5rem;  --space-8: 2rem;
  --space-12: 3rem;   --space-16: 4rem;   --space-24: 6rem;
}
```

### Rhythm rules

- **Proximity groups meaning.** Related items sit closer; the gap *between* groups
  must be visibly larger than the gap *within* a group. Uniform 16px everywhere is
  the classic slop tell — nothing looks related to anything.
- **One content column, one max-width.** Pick a measure (e.g. `--content: 72rem`
  for full layouts, `65ch` for reading) and reuse it. Inconsistent widths read as
  accidental.
- **Whitespace is intentional, not leftover.** Generous section padding
  (`--space-16`+ on desktop) signals confidence. But empty ≠ airy — whitespace
  should frame content, not just be absence.
- **Vertical rhythm:** space sections and stacks with the scale, not one-off
  margins. Prefer a layout primitive (a `stack` with a `--gap`) over per-element
  margins — see [`components.md`](components.md).

---

## 4. Motion

Motion should **reward or inform** — confirm an action, preserve continuity, guide
the eye. If a viewer would notice the animation more than the content, cut it.

### Purposeful vs. decorative

| Purposeful (keep) | Decorative (cut) |
|---|---|
| Hover/press feedback on controls | Elements that bounce/pulse forever |
| Fade+rise as a section enters (once) | Everything sliding in from all sides |
| Layout shifts eased, not jumped | Parallax that fights scrolling |
| Loading/skeleton transitions | Spinning icons with no meaning |

### Duration & easing

| Interaction | Duration | Easing |
|---|---|---|
| Micro (hover, toggle, focus) | 120–180ms | `ease-out` |
| Standard (dropdown, tab, card) | 200–300ms | `cubic-bezier(0.2, 0, 0, 1)` |
| Large (modal, page transition) | 300–450ms | ease-out-ish, decelerate |

- **Enter fast, settle slow** (decelerating ease-out) feels responsive.
- **Animate cheap properties** — `transform` and `opacity` only. Avoid animating
  `width`, `height`, `top`, `box-shadow` (they trigger layout/paint and jank).

### Scroll-reveal, done tastefully

- Reveal **once** (don't re-animate on scroll-up).
- **Subtle** — a short fade with a small rise (`translateY(8–16px)`), never large
  slides or zooms.
- Small **stagger** (~60–80ms) for lists reads as intentional; more feels slow.
- Use `IntersectionObserver`, not scroll listeners.

### Honor `prefers-reduced-motion` (required)

Reduced motion is a gate, not a nicety. Provide instant, non-moving fallbacks —
content must still appear and function.

```css
/* Default: subtle rise-and-fade on reveal */
.reveal {
  opacity: 0;
  transform: translateY(12px);
  transition: opacity 300ms ease-out, transform 300ms ease-out;
}
.reveal.is-visible { opacity: 1; transform: none; }

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
  /* Reveal content immediately — never leave it stuck invisible */
  .reveal { opacity: 1; transform: none; }
}
```

> If reveal is driven by JS, also check
> `window.matchMedia('(prefers-reduced-motion: reduce)').matches` and set the
> final state immediately instead of animating.

---

## 5. Anti-"AI-slop" checklist

The tells of generic AI-generated UI, and the deliberate move for each:

- **System font stack** doing everything → pick a real **display/body pairing**
  (§1).
- **Blue→purple gradient** hero/buttons → a single-hue gradient, duotone, texture,
  or flat accent with a point of view (§2).
- **Every card `#f3f4f6`, uniform gray** → a **tinted neutral ramp** with real
  surface hierarchy (§2).
- **Pure `#000` on `#fff`** → near-black on off-white, temperature-tinted (§2).
- **One accent used for everything** → accent reserved for the **primary action**;
  functional colors for state (§2).
- **Everything 16px apart** → a spacing scale where between-group > within-group
  (§3).
- **Every heading the same weight/size** → contrast in weight + size + case (§1).
- **Uniform border-radius on literally everything** → deliberate radius language;
  vary or commit, don't sprinkle one value everywhere.
- **Emoji as icons** → a real icon set (consistent grid, stroke, corners).
- **Center-everything layout, no grid** → alignment discipline; a repeatable
  section skeleton (eyebrow → headline → sub → action).
- **Decorative motion everywhere** → purposeful transitions only, reduced-motion
  honored (§4).
- **No signature** → one ownable element (a shape, a type treatment, a motion) you
  could recognize with the logo removed. This is the whole point (see
  [`critique.md`](critique.md), dimension 8).

---

## 6. Accessibility within aesthetics

Accessibility is a design constraint that lives here, not a cleanup phase. Deep
per-component state work is in [`components.md`](components.md); these are the
aesthetic gates.

### Contrast targets (WCAG AA)

| Content | Minimum ratio |
|---|---|
| Body / small text (< 18px, or < 14px bold) | **4.5:1** |
| Large text (≥ 24px, or ≥ 18.66px bold) | **3:1** |
| UI components & graphical objects (borders, icons, focus rings) | **3:1** |

**Verify, don't eyeball** — check computed ratios against the actual background
(including tinted surfaces and over images). Muted/placeholder gray text is the
usual failure. A single failing body-text ratio caps a critique at 15/24
([`critique.md`](critique.md)).

### Never rely on color alone

State and meaning must survive grayscale. Pair color with a **second channel**:

- Error field → red border **+ icon + text**, not just a red glow.
- Success/danger badges → **icon or label**, not hue alone.
- Chart series / links → **shape, underline, or label**, not color only.

### Visible focus

Every interactive element needs a **visible, non-default** focus indicator meeting
3:1 against its background. Use `:focus-visible` so it shows for keyboard users
without cluttering mouse clicks.

```css
:where(a, button, input, select, textarea, [tabindex]):focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
  border-radius: 3px; /* soften the ring on rounded controls */
}
```

Never `outline: none` without an equally-visible replacement. On dark or accent
backgrounds, give the ring a contrasting color (or a white/dark halo) so it stays
≥ 3:1.

---

**Next:** wire these values into tokens ([`tokens-theming.md`](tokens-theming.md)),
apply them to component states ([`components.md`](components.md)), and assemble
sections with named layouts ([`patterns.md`](patterns.md)). To audit an existing
UI against everything above, use [`critique.md`](critique.md).
