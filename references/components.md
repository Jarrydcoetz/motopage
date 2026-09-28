# Components — anatomy, states, primitives

How to design a component so it holds up in the real world, not just the
screenshot. Tokens (the `var(--…)` values used throughout) are defined in
[`tokens-theming.md`](tokens-theming.md); visual choices (type, color roles,
motion) live in [`aesthetics.md`](aesthetics.md); *when/why* to reach for a given
composite UI lives in [`patterns.md`](patterns.md). This file covers the
**mechanics, states, and accessibility** of the components themselves.

## 1. Component anatomy

Design every component along four axes before writing markup:

| Axis | Question | Example |
|---|---|---|
| **Structure** | What are the semantic parts? | button = label (+ optional icon) |
| **Variants** | What *roles* does it play? | primary / secondary / ghost / danger |
| **Sizes** | What scale steps exist? | sm / md / lg — never arbitrary |
| **States** | How does it respond over time? | the checklist below |

The happy-path default is the *easy 20%*. The other 80% of quality is states.
A component that only has a `:default` look is unfinished.

### States checklist — design every one that applies

- [ ] **default** — resting appearance
- [ ] **hover** — pointer over it (pointer devices only; never the sole signal)
- [ ] **focus-visible** — reached by keyboard; must be clearly visible
- [ ] **active / pressed** — during the press
- [ ] **disabled** — non-interactive; reduced contrast + `cursor: not-allowed`, and it must still be perceivable
- [ ] **loading** — async in flight; show progress, keep width stable, block re-entry
- [ ] **selected / checked / current** — for toggles, tabs, nav
- [ ] **error / invalid** — for inputs and forms
- [ ] **empty** — for any collection (list, table, results, cart)
- [ ] **read-only** vs disabled — visually distinct where both exist

If a state can't happen for a given component, say so deliberately — don't just
forget it.

## 2. Interaction & accessibility baseline

Non-negotiable for every interactive component:

**Semantic HTML first.** A `<button>` is a button; a link that navigates is an
`<a href>`. Reach for ARIA only to fill a genuine gap — a real element beats
`role="button"` + a keydown handler every time.

**Visible focus.** Never `outline: none` without a replacement. Use
`:focus-visible` so keyboard users get a ring while mouse users don't see one on
click:

```css
:where(a, button, input, select, textarea, [tabindex]):focus-visible {
  outline: 2px solid var(--color-focus-ring, var(--color-primary));
  outline-offset: 2px;
  border-radius: var(--radius-sm); /* ring follows the shape */
}
```

**Touch targets ≥ 44×44px.** Small controls get padding or an invisible hit
area — visual size can be smaller than the target:

```css
.icon-btn { position: relative; }
.icon-btn::after { /* expands the tap target without changing looks */
  content: ""; position: absolute; inset: -8px;
}
```

**Keyboard operable.** Everything reachable and actionable by keyboard alone
(Tab to reach, Enter/Space to activate, Esc to dismiss, arrows within composites
like tabs/menus). Focus order follows visual order.

**Don't rely on color alone.** Pair color with an icon, text, weight, or shape —
error fields get an icon + message, not just a red border; selected tabs get an
underline, not just a hue.

**Reduced motion.** Honor `prefers-reduced-motion` on every transition — see
[`aesthetics.md`](aesthetics.md) for the motion policy.

## 3. Layout primitives

Compose layouts from a handful of reusable primitives instead of writing bespoke
flex/grid on every screen. Each is one job. (Names follow the Every Layout
tradition.)

**Stack** — vertical rhythm between siblings:

```css
.stack { display: flex; flex-direction: column; }
.stack > * + * { margin-block-start: var(--space-md); } /* only between items */
```

**Cluster / Row** — horizontal group that wraps (toolbars, tag lists, button rows):

```css
.cluster {
  display: flex; flex-wrap: wrap;
  gap: var(--space-sm); align-items: center;
}
```

**Grid** — responsive cards without media-query soup:

```css
.grid {
  display: grid; gap: var(--space-lg);
  grid-template-columns: repeat(auto-fit, minmax(min(16rem, 100%), 1fr));
}
```

**Center / Container** — a max-width reading column with side gutters:

```css
.container {
  width: 100%; max-width: var(--measure, 72rem);
  margin-inline: auto;
  padding-inline: var(--space-gutter, 1rem);
}
```

**Sidebar** — content + aside that collapses to stacked when space runs out:

```css
.with-sidebar { display: flex; flex-wrap: wrap; gap: var(--space-lg); }
.with-sidebar > .sidebar { flex: 1 1 16rem; }
.with-sidebar > .main    { flex: 999 1 60%; min-width: 50%; }
```

**Aspect-ratio media box** — reserves space so layout doesn't jump on load:

```css
.media { aspect-ratio: 16 / 9; overflow: hidden; border-radius: var(--radius-md); }
.media > img, .media > video { width: 100%; height: 100%; object-fit: cover; }
```

Rule: if you're about to write a one-off `display: flex` with tuned margins, ask
whether Stack/Cluster/Grid already does it. Bespoke layout is where consistency
quietly dies.

## 4. Buttons & links

**Button vs. link — decide by outcome, not by looks.** A `<button>` *does*
something on this page (submit, toggle, open). An `<a href>` *navigates*
somewhere. A "button" that changes the URL is a link styled as a button; a "link"
that mutates state is a button. Get this right for keyboard, screen readers, and
open-in-new-tab.

**Variant system** — roles, not decoration:

| Variant | Role | Emphasis |
|---|---|---|
| `primary` | the one main action | filled, accent |
| `secondary` | alternative action | outlined / tonal |
| `ghost` | low-stakes / tertiary | text only until hover |
| `danger` | destructive | filled with `--color-danger` |

At most **one primary per view/section**. Sizes come from the space scale
(`sm` / `md` / `lg`), never arbitrary padding.

```css
.btn {
  --_bg: var(--color-primary);
  --_fg: var(--color-on-primary);
  display: inline-flex; align-items: center; gap: var(--space-2xs);
  min-height: 44px; padding: var(--space-xs) var(--space-md);
  border: 1px solid transparent; border-radius: var(--radius-md);
  background: var(--_bg); color: var(--_fg);
  font: inherit; font-weight: 600; line-height: 1; cursor: pointer;
  transition: background-color .15s ease, transform .05s ease;
}
.btn:hover        { background: var(--color-primary-hover); }
.btn:active       { transform: translateY(1px); }
.btn:focus-visible{ outline: 2px solid var(--color-focus-ring); outline-offset: 2px; }
.btn:disabled     { opacity: .5; cursor: not-allowed; background: var(--_bg); }

.btn--secondary {
  --_bg: transparent; --_fg: var(--color-primary);
  border-color: var(--color-border-strong);
}
.btn--ghost  { --_bg: transparent; --_fg: var(--color-text); }
.btn--danger { --_bg: var(--color-danger); --_fg: var(--color-on-danger); }
.btn--sm { min-height: 36px; padding: var(--space-2xs) var(--space-sm); }
.btn--lg { min-height: 52px; padding: var(--space-sm) var(--space-lg); }
```

**Loading state** keeps width stable and blocks double-submit:

```css
.btn[aria-busy="true"] { pointer-events: none; color: transparent; position: relative; }
.btn[aria-busy="true"]::after {
  content: ""; position: absolute; inset: 0; margin: auto;
  width: 1em; height: 1em; border: 2px solid currentColor; border-top-color: transparent;
  border-radius: 50%; color: var(--_fg); animation: btn-spin .6s linear infinite;
}
@keyframes btn-spin { to { transform: rotate(360deg); } }
@media (prefers-reduced-motion: reduce) { .btn[aria-busy="true"]::after { animation-duration: 1.5s; } }
```

Set `disabled` (or `aria-disabled`) *and* `aria-busy="true"` while loading, and
restore the label text for screen readers.

## 5. Forms & inputs

**Structure** — every field is a small, predictable unit:

```html
<div class="field">
  <label for="email">Email</label>
  <input id="email" name="email" type="email"
         aria-describedby="email-help email-error" aria-invalid="false" required>
  <p id="email-help" class="field__help">We'll only use this for your receipt.</p>
  <p id="email-error" class="field__error" hidden>Enter a valid email address.</p>
</div>
```

Rules that matter:

- **Always a visible `<label>`** tied by `for`/`id`. Placeholder text is not a
  label — it vanishes on input and often fails contrast.
- **Associate help and error** via `aria-describedby` so they're announced.
- **Signal invalidity** with `aria-invalid="true"` *and* a visible message +
  icon — never a red border alone.
- **Validate inline, forgivingly.** Validate on blur / submit, not on every
  keystroke; clear the error as soon as it's fixed.
- On submit failure, **move focus to the first invalid field** (or a summary).

```css
.field { display: flex; flex-direction: column; gap: var(--space-2xs); }
.field label { font-weight: 600; color: var(--color-text); }
.field input, .field select, .field textarea {
  min-height: 44px; padding: var(--space-xs) var(--space-sm);
  background: var(--color-surface); color: var(--color-text);
  border: 1px solid var(--color-border); border-radius: var(--radius-md);
}
.field input:focus-visible { outline: 2px solid var(--color-focus-ring); outline-offset: 1px; border-color: var(--color-primary); }
.field__help  { color: var(--color-text-muted); font-size: var(--font-sm); }
.field__error { color: var(--color-danger); font-size: var(--font-sm); }
.field input[aria-invalid="true"] { border-color: var(--color-danger); }
```

## 6. Component catalog

For each: purpose, structure, key states, accessibility notes. Strategy
(when/why) lives in [`patterns.md`](patterns.md).

### Card
- **Purpose:** group related content into a scannable unit.
- **Structure:** optional media (aspect-ratio box) → body (heading, text) →
  footer (actions). Use tokens for `--radius`, `--shadow`, `--color-surface`.
- **States:** default; hover/focus only if the *whole card* is a link/button
  (then wrap the primary link and make the card a clickable region — one focus
  stop, not many).
- **A11y:** don't nest interactive elements inside a linked card ("nested
  interactive"). Heading first for screen-reader scanning.

### Segmented control / tab toggle
- **Purpose:** switch between 2–5 mutually exclusive views/options.
- **Structure:** `role="tablist"` + `role="tab"` buttons controlling
  `role="tabpanel"`; or a `radiogroup` for form-like choices.
- **States:** selected (`aria-selected="true"`), hover, focus-visible, disabled.
- **A11y:** arrow keys move between tabs; only the active tab is in tab order
  (`tabindex="-1"` on the rest). Selection shown by underline/fill *and* aria,
  not color alone.

### Comparison table (highlighted column)
- **Purpose:** compare plans/features side by side.
- **Structure:** real `<table>` with `<th scope="col">` per plan and
  `<th scope="row">` per feature; one column visually promoted.
- **States:** highlighted column (border + tint via `--color-surface-raised`);
  row hover for tracking; sticky header on long tables.
- **A11y:** use a semantic checkmark with a text label (`<span class="sr-only">
  Included</span>`), not a bare ✓ glyph. Let it scroll horizontally on mobile
  inside a `role="region"` with `aria-label` and `tabindex="0"`.

### Pricing tier cards (good / better / best)
- **Purpose:** present 2–4 plans as a ladder with a recommended pick.
- **Structure:** Grid of cards; each = tier name, price, feature list, CTA. One
  card carries a "Most popular" badge and elevation.
- **States:** default; recommended (raised, accent border); CTA gets full button
  states from §4.
- **A11y:** the badge is real text, not just a colored ribbon. Keep every CTA
  the same size so no plan is keyboard-favored by accident.

### Stat / metric block
- **Purpose:** headline a single number (users, uptime, savings).
- **Structure:** big value + label + optional delta/trend indicator.
- **States:** loading (skeleton at final width); counts-up animation is optional
  and must respect reduced-motion and end at the true value.
- **A11y:** the accessible name is the full number + unit + label, not "↑ 12%"
  alone. Trend arrows pair with a sign/word, not color only.

### Testimonial card (avatar + rating)
- **Purpose:** social proof — a quote attributed to a real person.
- **Structure:** quote → rating → attribution (name, role) → avatar.
- **Avatar:** prefer initials monogram over a sourced image (project rule: no
  self-sourced images). Deterministic, always renders:

```html
<span class="avatar" aria-hidden="true">JC</span>
```
```css
.avatar {
  display: inline-grid; place-items: center;
  width: 2.5rem; height: 2.5rem; border-radius: 50%;
  background: var(--color-surface-raised); color: var(--color-text);
  font-weight: 700; font-size: var(--font-sm);
}
```

- **Rating:** render N filled + (5−N) empty stars, but expose the value to
  assistive tech as text so it isn't read as a pile of glyphs:

```html
<span class="rating" role="img" aria-label="Rated 5 out of 5">
  <span aria-hidden="true">★★★★★</span>
</span>
```

- **A11y:** mark up the quote as `<blockquote>` with a `<cite>` for attribution.

### Countdown timer
- **Purpose:** convey urgency toward a real deadline.
- **Structure:** days/hours/minutes/seconds segments, each value + unit label.
- **States:** running; **expired** (design it — swap to a clear "ended" message,
  never negative numbers); paused if reduced-motion suppresses per-second ticks.
- **A11y:** wrap in a live region that announces sparingly —
  `aria-live="off"` for the seconds, updating an `aria-live="polite"` summary on
  coarse intervals so screen readers aren't spammed every second. Only ever
  count toward a genuine deadline (see `patterns.md` on fake urgency).

### Accordion / FAQ
- **Purpose:** collapse long or optional content to reduce scanning cost.
- **Structure:** `<button aria-expanded>` as the header controlling a region via
  `aria-controls`; native `<details>/<summary>` is the zero-JS option.
- **States:** collapsed/expanded, hover, focus-visible, disabled.
- **A11y:** the toggle is a real `<button>`; expanded state is on
  `aria-expanded`, not just a rotated chevron. Animate `grid-template-rows`
  0fr→1fr (or height) and gate it on reduced-motion.

### Sticky navigation bar
- **Purpose:** keep primary nav + CTA reachable while scrolling.
- **Structure:** `<header>` with `position: sticky; top: 0`, a landmark `<nav>`,
  and a mobile disclosure (`aria-expanded` menu button).
- **States:** at-top vs. scrolled (add shadow/background on scroll for separation);
  current page marked `aria-current="page"`; menu open/closed on mobile.
- **A11y:** keep a `z-index` above content but below modals; ensure focus isn't
  trapped; account for sticky height with `scroll-margin-top` on anchor targets
  so headings aren't hidden under the bar.

### Newsletter / email-capture field
- **Purpose:** capture an email inline with minimal friction.
- **Structure:** a `<form>` with one labeled `<input type="email">` + submit; a
  status line for result.
- **States:** default, focus, invalid, submitting (`aria-busy`), success, error.
- **A11y:** visible label (visually hidden if the design demands, but present);
  announce success/error through `aria-live="polite"`; never disable the button
  in a way that hides why submission failed.

### Footer
- **Purpose:** wayfinding + trust (nav groups, legal, contact, social).
- **Structure:** `<footer>` landmark → column groups each with a heading → link
  lists → a baseline row (copyright, legal).
- **States:** link hover/focus; responsive collapse of columns to a stack.
- **A11y:** real heading elements per group; social icons need
  `aria-label`s (icon-only links are invisible to screen readers otherwise).

## 7. Consistency rules

Components feel like one system only when they **share a language** through
tokens — never per-component magic numbers.

- **Radius:** one scale (`--radius-sm/md/lg/full`). A control's radius is
  consistent everywhere it appears.
- **Shadow:** an elevation scale (`--shadow-sm/md/lg`); elevation maps to
  meaning (raised = interactive/important), not decoration.
- **Border:** `--color-border` (subtle) vs. `--color-border-strong`
  (interactive) — used consistently for the same roles.
- **Spacing:** internal padding and inter-element gaps come from the space
  scale; a `md` button and an `md` input share vertical rhythm.
- **Focus ring:** identical treatment across all interactive components.

See [`tokens-theming.md`](tokens-theming.md) for the token definitions and the
light/dark contract these all resolve through.

### Definition of done (per component)

- [ ] All applicable **states** from §1 are designed and reachable.
- [ ] **Semantic element** used; ARIA only where it fills a real gap.
- [ ] **Visible focus-visible** ring; full keyboard operation.
- [ ] Touch targets **≥ 44px**.
- [ ] Meaning never conveyed by **color alone**.
- [ ] Radius / shadow / border / spacing come from **tokens**, no magic numbers.
- [ ] Renders correctly in **light and dark**.
- [ ] **Loading** and **empty/error** paths handled, not just the happy path.
- [ ] Motion respects **`prefers-reduced-motion`**.

Reviewing rather than building? Take these to [`critique.md`](critique.md), whose
"Components & states" dimension scores exactly this.
