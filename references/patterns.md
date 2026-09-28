# Design Patterns — 15 named, reusable moves

These patterns are distilled from teardowns of real high-converting sites (the
seed was a motocross-training site that turned cold traffic into paid coaching).
They are **compositions** — recurring arrangements of type, color, spacing, and
motion that solve a specific persuasion or legibility problem. They are not
mechanics: for the plumbing of a given component (every state, layout
primitives) see [`components.md`](components.md); for the raw type/color/spacing/
motion choices see [`aesthetics.md`](aesthetics.md); for the token layer that
makes any of this themeable see [`tokens-theming.md`](tokens-theming.md); and to
audit whether a page uses the right patterns well, see
[`critique.md`](critique.md). This file says *what to assemble and when* — it
does not re-teach those siblings.

## Index

| # | Pattern | Use it when… |
|---|---|---|
| 1 | [Gradient-scrim hero over photo](#1-gradient-scrim-hero-over-photo) | text must sit on a busy full-bleed image |
| 2 | [Eyebrow → headline → sub → single-CTA rhythm](#2-eyebrow--headline--sub--single-cta-rhythm) | every section needs a scannable skeleton |
| 3 | [Semantic accent colors](#3-semantic-accent-colors) | actions differ in meaning (go vs. now-or-never) |
| 4 | [Countdown + price-step urgency](#4-countdown--price-step-urgency) | a *real* deadline should drive action |
| 5 | [Big-number stat row](#5-big-number-stat-row) | proof is quantifiable and skimmable |
| 6 | [Good/better/best ladder + "best value" badge](#6-goodbetterbest-ladder--best-value-badge) | offers vary by commitment level |
| 7 | [Honest comparison table with highlighted column](#7-honest-comparison-table-with-highlighted-column) | buyers compare you against alternatives |
| 8 | [Risk-reversal chips under CTAs](#8-risk-reversal-chips-under-ctas) | objections stall the click |
| 9 | ["Not sure?" low-commitment on-ramp](#9-not-sure-low-commitment-on-ramp) | a chunk of traffic isn't ready to buy |
| 10 | [Segmented control for multi-audience sections](#10-segmented-control-for-multi-audience-sections) | one section must serve distinct personas |
| 11 | [Monogram avatar fallback](#11-monogram-avatar-fallback) | social proof lacks headshots |
| 12 | [Alternating dark-image / flat-color bands](#12-alternating-dark-image--flat-color-bands) | a long scroll needs pacing |
| 13 | [Scroll-reveal motion](#13-scroll-reveal-motion) | entrances should feel alive, not flashy |
| 14 | [Condensed-display vs humanist-body type pairing](#14-condensed-display-vs-humanist-body-type-pairing) | the brand needs athletic personality |
| 15 | [Working footer](#15-working-footer) | the footer should convert and reassure |

See these rendered in [`../examples/pattern-gallery.html`](../examples/pattern-gallery.html).

---

## 1. Gradient-scrim hero over photo

**What it is.** A full-bleed hero image with a *directional* dark (or brand-tinted)
gradient overlay — the scrim — so headline and CTA stay legible no matter what
the photo contains.

**When to use.** Any photographic hero where text overlaps the image. Use a
directional scrim (darker where the text sits, clearing toward the focal
subject) rather than a flat 50% black wash, which muddies the whole image. **Not
for** flat-color heroes (no scrim needed) or images where legibility can be
solved by simply moving text into a solid panel beside the photo — that's often
cleaner.

**How to build.** Layer the gradient *between* image and text, sized to the text
block, not the viewport corner.

```css
.hero {
  position: relative;
  min-height: 68svh;
  display: grid;
  align-items: end;
  background: center / cover no-repeat var(--hero-img);
}
.hero::before {                 /* the scrim */
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to top,
    rgb(0 0 0 / 0.78) 0%,
    rgb(0 0 0 / 0.45) 38%,
    rgb(0 0 0 / 0) 72%
  );
}
.hero > .hero__content { position: relative; }  /* above the scrim */
```

**Accessibility note.** Verify the *rendered* text-on-scrim contrast at the
lightest point the text touches (≥ 4.5:1 body, ≥ 3:1 large). A gradient that
passes at the bottom can fail near its fade — test the worst pixel, not the
average. Never rely on the image itself for contrast; the scrim must carry it
even if the image fails to load.

**Misuse / anti-pattern.** A flat 50% overlay on the entire image (kills the
photo and still under-contrasts pale text), text with no scrim "because the
image is dark enough" (breaks the moment the image changes), or a text-shadow
used as a substitute for a scrim (fragile, ugly at scale).

---

## 2. Eyebrow → headline → sub → single-CTA rhythm

**What it is.** The repeatable section skeleton: a small-caps accent **eyebrow**
(category/context), a large **headline** (the claim), one supporting **sub** line,
and exactly one dominant **CTA**. Every section is a variation on this beat.

**When to use.** As the default spine for nearly every marketing section — it
gives the page a consistent, scannable rhythm. **Not for** dense reference
content (docs, tables, dashboards) where a "one CTA per section" rule is
artificial, and never stack two co-equal CTAs here (see the single-CTA rule
below).

**How to build.**

```html
<section class="beat">
  <p class="eyebrow">Rider development</p>
  <h2 class="headline">Ride faster with less fear</h2>
  <p class="sub">A structured program that builds real technique, not luck.</p>
  <a class="btn btn--primary" href="#start">Start training</a>
</section>
```

```css
.eyebrow { font: 600 0.75rem/1 var(--font-sans); letter-spacing: .12em;
           text-transform: uppercase; color: var(--accent-primary); }
.headline { font: 700 clamp(1.8rem, 5vw, 3rem)/1.05 var(--font-display); }
.sub { max-width: 52ch; color: var(--text-muted); }
```

One primary action per beat. A secondary action, if truly needed, is a quiet
text link — never a second filled button competing for the eye.

**Accessibility note.** The eyebrow is styling, not structure — keep the real
heading in an `<h2>`/`<h3>` so the document outline is intact. Don't rely on
`text-transform` for meaning; the underlying text should read correctly to a
screen reader.

**Misuse / anti-pattern.** Two equally weighted CTAs (splits attention, drops
conversion), an eyebrow that just repeats the headline, or skipping heading
levels because the eyebrow "looks like" the label.

---

## 3. Semantic accent colors

**What it is.** Distinct, reserved accents mapped to *meaning*: one for the
affirmative/primary action ("go"), a separate, rarer accent for urgency
("now-or-never"). Color becomes a language, not decoration.

**When to use.** Whenever the page has actions of different weight — a primary
CTA plus time-sensitive elements (countdowns, "spots left", price-step
warnings). **Not** as a license to add more hues: two or three roles maximum.
See [`aesthetics.md`](aesthetics.md) for building the palette and
[`tokens-theming.md`](tokens-theming.md) for expressing roles as semantic tokens.

**How to build.** Name accents by role, never by hue.

```css
:root {
  --accent-primary: #1f6feb;   /* affirmative / go — the main CTA */
  --accent-urgent:  #d1451b;   /* scarcity / deadline — used sparingly */
  --accent-info:    #2f7d5b;   /* neutral confirmation / guarantees */
}
.btn--primary { background: var(--accent-primary); }
.countdown, .badge--urgent { color: var(--accent-urgent); }
```

The urgent accent should appear on maybe 1–2 elements per page; scarcity that's
everywhere reads as noise.

**Accessibility note.** Meaning must survive color-blindness: pair the urgent
accent with an icon or word ("Ends Fri"), never color alone. Check each accent
against both its light and dark background for AA.

**Misuse / anti-pattern.** One accent used for everything (it stops meaning
anything — the #1 slop tell), or urgency red on ordinary buttons (cries wolf;
users tune it out).

---

## 4. Countdown + price-step urgency

**What it is.** A live countdown tied to a *real* deadline, plus explicit
"price goes up on DATE" framing, to convert deliberation into action.

**When to use.** Only when the deadline and price step are **genuinely real** —
an enrollment window that truly closes, a price that truly increases. **Never**
for a fake or perpetually-resetting timer. A countdown that resets on refresh is
a dark pattern, erodes trust instantly when noticed, and in many jurisdictions
is unlawful. If there's no real deadline, don't fake one — use pattern
[9](#9-not-sure-low-commitment-on-ramp) instead.

**How to build.** Drive the countdown from a fixed target timestamp, not a
per-visitor offset.

```html
<div class="countdown" data-deadline="2026-10-15T23:59:00-05:00" role="timer" aria-live="off">
  <span data-unit="days">--</span>d
  <span data-unit="hours">--</span>h
  <span data-unit="mins">--</span>m
</div>
<p class="price-step">Price rises to <strong>$399</strong> on <time datetime="2026-10-16">Oct 16</time>.</p>
```

```js
const el = document.querySelector('.countdown');
const end = new Date(el.dataset.deadline).getTime();
setInterval(() => {
  const t = Math.max(0, end - Date.now());
  el.querySelector('[data-unit="days"]').textContent  = Math.floor(t / 864e5);
  el.querySelector('[data-unit="hours"]').textContent = Math.floor(t / 36e5) % 24;
  el.querySelector('[data-unit="mins"]').textContent  = Math.floor(t / 6e4) % 60;
}, 1000);
```

**Accessibility note.** Don't set `aria-live="polite"` on a per-second ticker —
it will spam screen readers every second. Announce the deadline once as static
text (`<time>`), and leave the visual ticker `aria-hidden` or `aria-live="off"`.

**Misuse / anti-pattern.** Resetting/fake timers, evergreen "offer ends today"
that never ends, or stacking multiple scarcity signals ("only 3 left!" + timer +
"127 people viewing") until the page feels like a scam.

---

## 5. Big-number stat row

**What it is.** A horizontal row of oversized numerals, each with a tiny
small-caps caption — instantly skimmable social proof or outcomes.

**When to use.** When proof is quantifiable (riders coached, years, podium
finishes, refund rate). Three or four stats is the sweet spot. **Not for** soft
claims that need a sentence, and never pad with vanity metrics ("14 coffees/day")
that dilute the real ones.

**How to build.**

```html
<dl class="stats">
  <div><dt>2,400+</dt><dd>Riders coached</dd></div>
  <div><dt>17</dt><dd>Pro podiums</dd></div>
  <div><dt>4.9/5</dt><dd>Avg rating</dd></div>
</dl>
```

```css
.stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: var(--space-6); }
.stats dt { font: 800 clamp(2rem, 6vw, 3.4rem)/1 var(--font-display);
            font-variant-numeric: tabular-nums; color: var(--accent-primary); }
.stats dd { font: 600 .7rem/1.2 var(--font-sans); letter-spacing: .1em;
            text-transform: uppercase; color: var(--text-muted); }
```

**Accessibility note.** Use `<dl>`/`<dt>`/`<dd>` so number and label are
programmatically paired. Use `tabular-nums` so figures align. The caption must be
a real label, not decorative — screen readers read "2,400 plus, riders coached".

**Misuse / anti-pattern.** Unlabeled numbers (a giant "98%" of *what?*), animated
count-ups that delay the meaning, or invented stats — fabricated proof is the
fastest way to lose the sale and the trust.

---

## 6. Good/better/best ladder + "best value" badge

**What it is.** Three (rarely four) offer tiers ordered by commitment, with the
intended anchor tier visually flagged ("Best value" / "Most popular").

**When to use.** When the offer scales by commitment (self-serve → coached →
1:1). The badge steers the undecided toward the tier you want to sell. **Not for**
a single product (no ladder to climb), and don't exceed four tiers — choice
overload kills conversion.

**How to build.** Elevate the recommended card (scale, border in the primary
accent, badge) so the eye lands there first.

```html
<div class="tiers">
  <article class="tier">…Good…</article>
  <article class="tier tier--featured">
    <span class="tier__badge">Best value</span> …Better…
  </article>
  <article class="tier">…Best…</article>
</div>
```

```css
.tier--featured { border: 2px solid var(--accent-primary); transform: translateY(-.5rem); }
.tier__badge { background: var(--accent-primary); color: #fff;
               font: 600 .7rem/1 var(--font-sans); letter-spacing: .08em;
               text-transform: uppercase; padding: .35rem .6rem; border-radius: 999px; }
```

**Accessibility note.** The badge must be real text inside the card, not a
background image, so it's announced. Don't convey "recommended" with color/scale
alone — the badge word carries it. Ensure the featured card's contrast holds in
dark mode (a light border on dark can drop below 3:1).

**Misuse / anti-pattern.** Flagging every tier (nothing stands out), a
decoy-only top tier priced absurdly to make the middle look cheap (manipulative
and transparent to buyers), or five-plus tiers.

---

## 7. Honest comparison table with highlighted column

**What it is.** A feature matrix (rows) × options (columns) with the recommended
option's column visually tinted/highlighted — using transparency about
trade-offs as the selling point.

**When to use.** When buyers actively compare you to alternatives (competitors,
DIY, "do nothing"). An *honest* table that admits where you're not the pick for
everyone builds more trust than a rigged all-checkmarks column. **Not for** a
single feature list (use a plain list), and never when you'd have to lie to win a
row.

**How to build.** Tint the recommended column via `col`/cell background and a
`scope`-correct header structure.

```html
<table class="compare">
  <thead>
    <tr>
      <th scope="col">Feature</th>
      <th scope="col">Free plan</th>
      <th scope="col" class="is-rec">Coached ★</th>
      <th scope="col">1:1</th>
    </tr>
  </thead>
  <tbody>
    <tr><th scope="row">Video library</th><td>✓</td><td class="is-rec">✓</td><td>✓</td></tr>
    <tr><th scope="row">Weekly feedback</th><td>—</td><td class="is-rec">✓</td><td>✓</td></tr>
  </tbody>
</table>
```

```css
.compare .is-rec { background: color-mix(in srgb, var(--accent-primary) 12%, transparent); }
.compare th[scope="col"].is-rec { border-top: 3px solid var(--accent-primary); }
```

**Accessibility note.** Use `<th scope="col">` and `<th scope="row">` so cells
are associated with both headers. Don't encode ✓/— with color only — the glyph
plus, ideally, an `aria-label` ("included" / "not included") carries the meaning.
Ensure the tint doesn't push cell text below AA.

**Misuse / anti-pattern.** A rigged table where your column is all ✓ and rivals
are all — (readers discount it instantly), tiny check icons with no text
alternative, or tinting so heavy the text underneath fails contrast.

---

## 8. Risk-reversal chips under CTAs

**What it is.** A small row of guarantee "chips" — financing available, money-back
refund, cancel anytime, secure checkout — placed *right at the decision point*,
directly beneath the CTA, to dissolve last-second objections.

**When to use.** Immediately under primary CTAs and in the checkout/footer area,
where hesitation peaks. **Not** scattered decoratively across the page (they lose
their job) and only for guarantees you actually honor.

**How to build.** Compact, icon-plus-label pills that read as reassurance, not
buttons.

```html
<a class="btn btn--primary" href="#buy">Enroll now</a>
<ul class="chips">
  <li class="chip">🔒 Secure checkout</li>
  <li class="chip">↩ 14-day refund</li>
  <li class="chip">◷ Cancel anytime</li>
</ul>
```

```css
.chips { display: flex; flex-wrap: wrap; gap: var(--space-2); list-style: none; padding: 0; }
.chip { display: inline-flex; align-items: center; gap: .4rem;
        font: 500 .8rem/1 var(--font-sans); color: var(--text-muted);
        border: 1px solid var(--border); border-radius: 999px; padding: .4rem .7rem; }
```

**Accessibility note.** Chips are informational, not interactive — don't give them
button/link semantics or focus styles that imply clickability. If an emoji/icon
carries meaning, follow it with real text (as above) so it isn't announced as
just "lock".

**Misuse / anti-pattern.** Making chips look like tappable buttons (frustrating
dead clicks), promising guarantees you don't offer, or so many chips the CTA
drowns — three to four, tightly scoped.

---

## 9. "Not sure?" low-commitment on-ramp

**What it is.** An explicit escape hatch for not-ready-to-buy visitors — a short
quiz, free tier, sample lesson, or "get the plan" — captured *before* they bounce.

**When to use.** When a meaningful share of traffic is interested but not ready,
and the primary offer is high-commitment/high-price. Place it *after* the main
CTA as the quieter alternative. **Not** as a co-equal competitor to the primary
CTA (it becomes an easy out that cannibalizes sales) — it's the softer second
option, styled quieter.

**How to build.** Visually subordinate: outline/ghost button or plain link,
never a second filled button.

```html
<div class="onramp">
  <a class="btn btn--primary" href="#enroll">Enroll now</a>
  <a class="btn btn--ghost" href="#quiz">Not sure? Take the 60-second fit quiz →</a>
</div>
```

```css
.btn--ghost { background: transparent; color: var(--accent-primary);
              border: 1px solid var(--border); }
```

**Accessibility note.** Make the on-ramp a real focusable link/button in logical
tab order after the primary CTA. If it opens a quiz, ensure the flow is keyboard-
completable and progress is announced.

**Misuse / anti-pattern.** Styling the on-ramp as loud as the primary CTA
(splits intent), a "free" on-ramp that's a bait-and-switch, or burying it so far
down that the not-ready crowd never sees it before leaving.

---

## 10. Segmented control for multi-audience sections

**What it is.** A single tab/toggle that swaps one section's content between
audiences or modes (Beginner / Advanced, Monthly / Annual) — serving multiple
personas without duplicating the page.

**When to use.** When one section genuinely differs by segment and the segments
are mutually exclusive (2–4 options). **Not for** navigation between pages (use
links/tabs at the page level), and not when both segments' content should be
visible at once (use columns instead).

**How to build.** A real tablist so keyboard and screen-reader users can operate
it.

```html
<div class="segmented" role="tablist" aria-label="Choose your level">
  <button role="tab" aria-selected="true"  aria-controls="p-beg" id="t-beg">Beginner</button>
  <button role="tab" aria-selected="false" aria-controls="p-adv" id="t-adv">Advanced</button>
</div>
<div role="tabpanel" id="p-beg" aria-labelledby="t-beg">…</div>
<div role="tabpanel" id="p-adv" aria-labelledby="t-adv" hidden>…</div>
```

```css
.segmented { display: inline-flex; background: var(--surface-2);
             border-radius: 999px; padding: .25rem; }
.segmented [aria-selected="true"] { background: var(--surface-1);
             box-shadow: 0 1px 2px rgb(0 0 0 / .15); }
```

**Accessibility note.** Use `role="tablist"`/`tab`/`tabpanel`, toggle
`aria-selected` and `hidden`, and support arrow-key movement between tabs. The
selected state must be visible without color alone (the raised pill does that
here).

**Misuse / anti-pattern.** `<div>`s with click handlers and no ARIA/keyboard
support, selected state shown by color only, or hiding critical content behind a
tab a user may never click (put must-see content outside the control).

---

## 11. Monogram avatar fallback

**What it is.** An initials-in-a-circle placeholder used when a real headshot
isn't available — keeping testimonial/social-proof rows graceful and consistent.

**When to use.** Any avatar context where a photo may be missing (testimonials,
comments, team). **Not** as a permanent substitute when real photos exist and
would land harder — a monogram is the *fallback*, not the goal.

**How to build.** Deterministic background from the name so the same person is
always the same color; a11y-labeled.

```html
<span class="avatar" role="img" aria-label="Dana Reyes" style="--h: 210">DR</span>
```

```css
.avatar { display: inline-grid; place-items: center; width: 2.5rem; height: 2.5rem;
          border-radius: 50%; color: #fff; font: 700 .9rem/1 var(--font-sans);
          background: hsl(var(--h) 45% 42%); }
```

**Accessibility note.** Give the element `role="img"` + `aria-label` with the
full name (the visible "DR" alone is meaningless to a screen reader). Ensure the
initials meet ≥ 4.5:1 against the generated background — cap lightness so no
random hue produces low contrast.

**Misuse / anti-pattern.** Random hue with no contrast floor (some names get
unreadable pastels), initials of the *company* not the person, or using
monograms to fake testimonials from people who don't exist.

---

## 12. Alternating dark-image / flat-color bands

**What it is.** Pacing a long scroll by alternating full-bleed **dark image**
sections with **flat light** (or flat-color) sections — a visual rhythm of
"cinematic → calm → cinematic".

**When to use.** On long marketing pages that would otherwise feel monotonous.
The dark image bands carry emotion/energy; the flat bands carry dense info
(pricing, FAQ, comparison) legibly. **Not for** short pages (one hero is enough)
or content-heavy apps where full-bleed imagery just adds load and distraction.

**How to build.** Alternate a `band--image` (scrim + photo, see pattern 1) with a
`band--flat` (solid surface token), keeping a shared content max-width inside.

```css
.band { padding-block: clamp(3rem, 8vw, 6rem); }
.band__inner { max-width: 68rem; margin-inline: auto; padding-inline: 1rem; }
.band--flat  { background: var(--surface-1); color: var(--text); }
.band--image { position: relative; color: #fff;
               background: center / cover var(--band-img); }
.band--image::before { content: ""; position: absolute; inset: 0;
               background: linear-gradient(rgb(0 0 0 /.6), rgb(0 0 0 /.75)); }
```

**Accessibility note.** Each band sets its *own* text color against its *own*
background — don't let a global text color ride from a dark band onto a light one
(a classic dark-mode bug). Re-verify contrast per band. Keep total image weight
down; lazy-load below-fold bands.

**Misuse / anti-pattern.** Every section full-bleed dark (exhausting, no rhythm,
heavy), or inconsistent content width jumping band to band (breaks the spine).

---

## 13. Scroll-reveal motion

**What it is.** A subtle fade-and-rise as sections enter the viewport, **once**,
to make the page feel alive — not a carnival of flying elements.

**When to use.** For a light layer of polish on marketing pages. Keep it small
(≤ ~16px rise, ~400–600ms) and one-shot. **Not for** app UIs, above-the-fold
content the user needs immediately, or anything that would delay interaction.

**How to build.** `IntersectionObserver` adds a class; CSS does the rest; reduced
motion is a first-class branch, not an afterthought.

```css
.reveal { opacity: 0; transform: translateY(16px);
          transition: opacity .5s ease, transform .5s ease; }
.reveal.is-in { opacity: 1; transform: none; }
@media (prefers-reduced-motion: reduce) {
  .reveal { opacity: 1; transform: none; transition: none; }
}
```

```js
if (!matchMedia('(prefers-reduced-motion: reduce)').matches) {
  const io = new IntersectionObserver((entries) => {
    for (const e of entries) if (e.isIntersecting) {
      e.target.classList.add('is-in'); io.unobserve(e.target);   // once
    }
  }, { threshold: 0.15 });
  document.querySelectorAll('.reveal').forEach((el) => io.observe(el));
} else {
  document.querySelectorAll('.reveal').forEach((el) => el.classList.add('is-in'));
}
```

**Accessibility note.** Content must be fully readable *without* JS and when
motion is reduced — the initial `opacity: 0` is only safe because the reduced-
motion branch and the no-JS fallback both reveal it. Never gate real content
behind an animation that might not fire.

**Misuse / anti-pattern.** Re-animating on every scroll pass (nauseating),
animating above-the-fold content (delays the first read), long/staggered
cascades that make the page feel slow, or forgetting the reduced-motion branch.

---

## 14. Condensed-display vs humanist-body type pairing

**What it is.** A high-contrast pairing: a tall, tight **condensed display**
typeface for headlines (athletic, urgent) against a warm **humanist sans** for
body (readable, approachable). The contrast *is* the personality.

**When to use.** When the brand wants energy and edge (sports, performance,
bold DTC). The width contrast (narrow display vs. normal body) reads as
distinctive without extra decoration. **Not for** dense editorial or data-heavy
UIs where a condensed display hurts scanning, and never condensed for body text.
See [`aesthetics.md`](aesthetics.md) for pairing theory and fallbacks.

**How to build.** Assign by role via tokens; give each a real fallback stack.

```css
:root {
  --font-display: "Anton", "Oswald", "Arial Narrow", system-ui, sans-serif;
  --font-sans: "Inter", ui-sans-serif, system-ui, -apple-system, sans-serif;
}
h1, h2, .headline { font-family: var(--font-display); letter-spacing: .01em;
                    line-height: 1.02; text-wrap: balance; }
body, p, li { font-family: var(--font-sans); line-height: 1.55; }
```

**Accessibility note.** Condensed faces at small sizes are hard to read — keep
condensed for large headings only, and ensure the body face has generous
line-height (~1.5) and 45–75ch line length. Don't set condensed all-caps for long
strings (legibility drops sharply).

**Misuse / anti-pattern.** Two similar sans fonts (no real contrast, wasted
loads), condensed used for body/labels, or a display face with no fallback (FOUT
or a broken layout when it fails to load).

---

## 15. Working footer

**What it is.** A footer that *does work* — email capture, social row, full
sitemap, trust/payment badges, and a locale switcher — instead of a dead
copyright line. It converts stragglers and reassures.

**When to use.** On essentially every marketing/commerce site — the footer is the
second-most-visited region after the hero. **Not** an excuse to dump every link:
group into a real sitemap. Skip locale switching if you're single-locale.

**How to build.** A responsive multi-column grid that stacks on mobile, with the
capture form and reassurance elements grouped.

```html
<footer class="site-footer">
  <div class="foot-grid">
    <form class="foot-capture">
      <label for="foot-mail">Get the free training plan</label>
      <input id="foot-mail" type="email" autocomplete="email" placeholder="you@email.com">
      <button type="submit" class="btn btn--primary">Send it</button>
    </form>
    <nav class="foot-nav" aria-label="Footer"><!-- grouped sitemap columns --></nav>
  </div>
  <div class="foot-meta">
    <ul class="social" aria-label="Social media"><!-- icon links --></ul>
    <ul class="pay-badges" aria-label="Accepted payments"><!-- Visa, MC, … --></ul>
    <label class="locale"><span class="sr-only">Language</span>
      <select><option>English</option><option>Español</option></select>
    </label>
  </div>
</footer>
```

**Accessibility note.** The email `<input>` needs a real associated `<label>`
(visible or `sr-only`) and `type="email"` + `autocomplete`. Wrap link groups in
`<nav aria-label>`. Icon-only social links need `aria-label`. The `<select>`
needs a label too. Footer contrast fails often because it's usually the darkest
band — verify it.

**Misuse / anti-pattern.** A one-line "© 2026" with nothing actionable
(wasted real estate), an unlabeled email field, a wall of ungrouped links, or
fake trust badges (a security seal you don't actually hold).
