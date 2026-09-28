# Design Tokens & Theming — the foundation

Establish tokens **before** aesthetics and components. Tokens are the wiring;
[`aesthetics.md`](aesthetics.md) decides what values flow through them and
[`components.md`](components.md) consumes them. Get this layer right and every
later decision cascades instead of being retyped and drifting.

## Why tokens

- **Single source of truth.** A color or spacing value is defined once; every
  surface references the name, not the hex. Change the name, change everywhere.
- **Theming for free.** Light/dark (and brand variants) become a remap of a
  small semantic layer — not a rewrite of component CSS.
- **Consistency by construction.** If the only radius available is
  `--radius-md`, components can't invent a seventh corner rounding.
- **Brand adaptation.** Reskinning is swapping primitive values, not hunting
  literals across a codebase.

## Token tiers

Three tiers, each referencing the one above. This indirection is the whole
point — don't collapse it.

| Tier | What it is | Examples | Rule |
|---|---|---|---|
| **1. Primitive** (global) | Raw, context-free scales. Named by *what they are*. | `--blue-500`, `--gray-950`, `--space-4`, `--radius-md`, `--text-lg` | Never referenced by components directly. |
| **2. Semantic** (alias) | Role-based names that point at primitives. Named by *what they're for*. | `--color-bg`, `--color-surface`, `--color-text`, `--color-primary`, `--color-danger` | The **only** layer themes remap. |
| **3. Component** | Local names scoped to one component, pointing at semantic tokens. | `--button-bg`, `--card-radius`, `--input-border` | Optional; use when a component needs its own knob. See [`components.md`](components.md). |

**The cascade rule:** components consume **semantic** tokens (or their own
component tokens that resolve to semantic ones), **never primitives directly**.
`background: var(--blue-500)` in a button is a bug — it can't theme and it
leaks a raw value into a role. Use `var(--color-primary)`.

Why the middle tier matters: primitives answer "what colors exist," semantics
answer "what colors *mean here*." Dark mode changes the meaning
(`--color-surface` becomes a dark gray) without touching the primitive ramp or
any component. No semantic layer → every component hard-codes and dark mode is a
find-and-replace.

## Concrete implementation

A coherent, copy-pasteable starting `:root`. Replace the *values* per brand (see
[`aesthetics.md`](aesthetics.md) for choosing them); keep the *structure*.

```css
:root {
  /* ---------- TIER 1: PRIMITIVES ---------- */

  /* Neutral ramp (50 lightest → 950 darkest). Slightly cool-tinted, not pure gray. */
  --gray-50:  #f7f8fa;
  --gray-100: #eceef2;
  --gray-200: #dce0e7;
  --gray-300: #c2c9d4;
  --gray-400: #97a1b2;
  --gray-500: #6b7789;
  --gray-600: #4d5766;
  --gray-700: #3a424e;
  --gray-800: #262c35;
  --gray-900: #171b21;
  --gray-950: #0d0f13;

  /* Primary accent ramp */
  --brand-50:  #eef4ff;
  --brand-100: #d9e6ff;
  --brand-200: #b6ccff;
  --brand-300: #85a9ff;
  --brand-400: #5a83f5;
  --brand-500: #3b5ee0;   /* base */
  --brand-600: #2f49b8;
  --brand-700: #283c93;
  --brand-800: #243373;
  --brand-900: #1f2b5c;

  /* Secondary accent (reserve for a distinct role — highlight, not a second CTA) */
  --amber-400: #f4b740;
  --amber-500: #e09b1f;
  --amber-600: #b87b12;

  /* Feedback primitives */
  --red-500:   #d9414c;   --red-600:   #b32d38;
  --green-500: #1f9d63;   --green-600: #157a4b;

  /* Spacing scale (rem, 4px base) */
  --space-1:  0.25rem;  --space-2: 0.5rem;   --space-3: 0.75rem;
  --space-4:  1rem;     --space-5: 1.5rem;   --space-6: 2rem;
  --space-8:  3rem;     --space-12: 4.5rem;  --space-16: 6rem;

  /* Radii */
  --radius-sm: 0.25rem; --radius-md: 0.5rem; --radius-lg: 0.875rem;
  --radius-xl: 1.25rem; --radius-full: 999px;

  /* Type scale (fluid-friendly; pair via aesthetics.md) */
  --font-sans: "Inter", system-ui, sans-serif;
  --font-display: "Fraunces", Georgia, serif;
  --font-mono: "JetBrains Mono", ui-monospace, monospace;
  --text-xs: 0.75rem;  --text-sm: 0.875rem; --text-base: 1rem;
  --text-lg: 1.125rem; --text-xl: 1.375rem; --text-2xl: 1.75rem;
  --text-3xl: 2.25rem; --text-4xl: 3rem;
  --leading-tight: 1.15; --leading-normal: 1.5; --leading-loose: 1.7;
  --weight-normal: 400; --weight-medium: 500; --weight-bold: 700;

  /* Shadows (tuned for light bg; dark redefines below) */
  --shadow-sm: 0 1px 2px rgb(13 15 19 / 0.06);
  --shadow-md: 0 4px 12px rgb(13 15 19 / 0.08);
  --shadow-lg: 0 12px 32px rgb(13 15 19 / 0.12);

  /* Z-index scale — named, never magic numbers */
  --z-base: 0; --z-dropdown: 100; --z-sticky: 200;
  --z-overlay: 300; --z-modal: 400; --z-toast: 500;

  /* ---------- TIER 2: SEMANTICS (light defaults) ---------- */
  --color-bg:            var(--gray-50);
  --color-surface:       #ffffff;
  --color-surface-raised: #ffffff;
  --color-text:          var(--gray-900);
  --color-text-muted:    var(--gray-600);
  --color-text-subtle:   var(--gray-500);
  --color-border:        var(--gray-200);
  --color-border-strong: var(--gray-300);

  --color-primary:       var(--brand-500);
  --color-primary-hover: var(--brand-600);
  --color-primary-fg:    #ffffff;          /* text/icon ON primary */

  --color-accent:        var(--amber-500);
  --color-accent-fg:     var(--gray-950);

  --color-danger:        var(--red-500);
  --color-danger-fg:     #ffffff;
  --color-success:       var(--green-500);
  --color-success-fg:    #ffffff;

  --color-focus-ring:    var(--brand-400);
  --shadow-card:         var(--shadow-md);
}
```

Note every background role ships with a paired `*-fg` (foreground) token. That
pairing is what makes AA contrast checkable — see [Accessibility](#accessibility).

## Light/dark theming contract

**The key pattern.** Theme by **remapping semantic tokens only** — never by
overriding component styles per theme. A component written against
`var(--color-surface)` themes itself the moment the token's value changes.

Support three intents, in this precedence: an explicit **manual** choice wins;
otherwise follow the **OS** preference; light is the baseline default.

```css
/* Dark values, defined once as a reusable block. */
@media (prefers-color-scheme: dark) {
  /* Guard so a manual [data-theme="light"] override still wins over the OS. */
  :root:not([data-theme="light"]) {
    --color-bg:            var(--gray-950);
    --color-surface:       var(--gray-900);
    --color-surface-raised: var(--gray-800);
    --color-text:          var(--gray-50);
    --color-text-muted:    var(--gray-400);
    --color-text-subtle:   var(--gray-500);
    --color-border:        var(--gray-800);
    --color-border-strong: var(--gray-700);

    --color-primary:       var(--brand-400);   /* lift for contrast on dark */
    --color-primary-hover: var(--brand-300);
    --color-primary-fg:    var(--gray-950);

    --color-accent:        var(--amber-400);
    --color-accent-fg:     var(--gray-950);

    --color-danger:        var(--red-500);
    --color-success:       var(--green-500);

    --color-focus-ring:    var(--brand-300);

    --shadow-sm: 0 1px 2px rgb(0 0 0 / 0.4);
    --shadow-md: 0 4px 12px rgb(0 0 0 / 0.5);
    --shadow-lg: 0 12px 32px rgb(0 0 0 / 0.6);
  }
}

/* Explicit manual overrides — highest specificity, work regardless of OS. */
:root[data-theme="dark"] {
  --color-bg:            var(--gray-950);
  --color-surface:       var(--gray-900);
  --color-surface-raised: var(--gray-800);
  --color-text:          var(--gray-50);
  --color-text-muted:    var(--gray-400);
  --color-border:        var(--gray-800);
  --color-primary:       var(--brand-400);
  --color-primary-hover: var(--brand-300);
  --color-primary-fg:    var(--gray-950);
  --color-focus-ring:    var(--brand-300);
  --shadow-sm: 0 1px 2px rgb(0 0 0 / 0.4);
  --shadow-md: 0 4px 12px rgb(0 0 0 / 0.5);
  --shadow-lg: 0 12px 32px rgb(0 0 0 / 0.6);
}

:root[data-theme="light"] {
  /* Re-assert light so a manual "light" choice overrides the OS media query.
     (Same values as :root defaults; list the ones the dark block changed.) */
  --color-bg:            var(--gray-50);
  --color-surface:       #ffffff;
  --color-text:          var(--gray-900);
  --color-primary:       var(--brand-500);
  --color-primary-fg:    #ffffff;
}

/* body ALWAYS gets an explicit background + text color. */
body {
  background: var(--color-bg);
  color: var(--color-text);
  font-family: var(--font-sans);
}
```

**Why the dark block is duplicated (media query + `[data-theme="dark"]`):** the
media query handles "user hasn't chosen, follow the OS"; the attribute selector
handles "user clicked the toggle." Extract shared declarations into a CSS custom
property set or `@layer` if the duplication bothers you, but keep both entry
points — they answer different questions.

**Toggle wiring (minimal):** set `document.documentElement.dataset.theme` to
`"light"` / `"dark"`, persist to `localStorage`, and to avoid a flash apply the
saved value in a tiny inline `<head>` script before first paint. Removing the
attribute (`delete dataset.theme`) falls back to "follow OS."

**Contract rules:**
- Theme = remap semantics. Never write `[data-theme="dark"] .button { … }`.
- Every semantic token that changes in light must have a dark counterpart.
- Test both themes for every component (see [`critique.md`](critique.md)).
- Shadows are theme tokens too — dark surfaces need darker, softer shadows or
  they vanish.

## Naming conventions

Predictable names are what let you guess a token without grepping.

- **Primitives:** `--<hue|scale>-<step>` — `--gray-700`, `--space-4`,
  `--text-lg`. Numeric scales ascend consistently (lightness for color, size for
  space/type).
- **Semantics:** `--color-<role>[-<variant>]` — `--color-text-muted`,
  `--color-surface-raised`. Role first, modifier last.
- **Pairing:** a `*-fg` foreground for every background role
  (`--color-primary` / `--color-primary-fg`).
- **Components:** `--<component>-<property>` — `--button-bg`, `--card-radius`.

| Do | Don't |
|---|---|
| `--color-text-muted` (role) | `--gray-secondary-text` (mixes tiers) |
| `--space-4` (scale step) | `--gap-16px` (encodes the value in the name) |
| `--color-primary` / `--color-primary-fg` | `--color-primary` alone (no paired fg) |
| `--color-danger` (semantic role) | `--color-red` (primitive posing as a role) |
| `--brand-500` (primitive) referenced only in semantics | `var(--brand-500)` inside a component |
| Consistent steps: `50…950` | `--gray-lighter`, `--gray-lightish` (unorderable) |

## Brand adaptation workflow

To reskin without touching components or the semantic layer:

1. **Swap the primitive ramps.** Regenerate `--brand-*` (and `--gray-*` if the
   neutral tint changes) for the new brand. Keep step count and naming.
2. **Repoint accents if roles shift.** If the brand's accent is warm, update the
   `--amber-*` primitives (or add a new ramp) and point `--color-accent` at it.
3. **Leave the semantic layer alone.** `--color-primary: var(--brand-500)` still
   holds; only what `--brand-500` *is* changed.
4. **Re-verify contrast** for both themes ([Accessibility](#accessibility)) —
   new hues can break AA pairings even when names are unchanged.
5. **Adjust type/radius primitives** if the brand's voice differs (a serif
   display, tighter radii). Components inherit automatically.
6. **Don't touch component CSS.** If you have to, a token is missing — add it to
   tier 2/3 rather than hard-coding.

The test of a good token setup: a full rebrand is a diff confined to the
primitive block and a handful of semantic repoints.

## Framework notes

- **Tailwind.** Map the *primitive* ramps into `theme.colors` /
  `theme.spacing` / `theme.borderRadius`, then define semantic aliases that
  reference CSS variables (`primary: 'var(--color-primary)'`). Drive dark mode
  with `darkMode: ['class', '[data-theme="dark"]']` so the class strategy matches
  the attribute contract above. Utilities then consume semantics
  (`bg-primary text-primary-fg`), keeping the "no primitives in components" rule.
- **shadcn/ui.** Already ships this model: `globals.css` defines semantic CSS
  variables (`--background`, `--foreground`, `--primary`, `--border`, …) on
  `:root` and `.dark`. Treat those as your tier-2 layer, back them with your own
  primitive ramp, and let components read them via Tailwind. Align its
  `.dark` class with your `[data-theme]` strategy. See [`vercel:shadcn`] for
  component specifics — theme by editing the variables, not the components.

## Accessibility

Contrast is a **token-pair** property, so bake it into the token design:

- **Every background role has a paired foreground token**, and the pair is
  verified: body text ≥ **4.5:1**, large text (≥ 24px / 19px bold) and UI/icons
  ≥ **3:1**. Verify with a contrast checker — never eyeball.
- **Check both themes.** A pair that passes on light can fail on dark. This is
  why `--color-primary` lifts to `--brand-400` in dark mode: `--brand-500` on a
  dark surface often misses AA. Re-run every pair per theme.
- **Muted text still passes.** `--color-text-muted` must clear 4.5:1 against
  `--color-bg` *and* `--color-surface`; "muted" is not a license to fail.
- **Focus ring is a token with its own contrast.** `--color-focus-ring` needs
  ≥ 3:1 against adjacent backgrounds in both themes.
- **Never signal state by color alone** (icon/text/shape too) — a token
  supplies the color, not the meaning; see [`patterns.md`](patterns.md) and
  [`components.md`](components.md) for state treatment.
- Contrast is a **gate**, not a nice-to-have — a failing pair caps a design
  critique regardless of everything else ([`critique.md`](critique.md)).

**Definition of done for this layer:** three tiers exist and are respected; the
semantic layer is the only thing themes remap; both themes remap every changed
token; `body` sets an explicit background; every background/foreground pair
passes WCAG AA in light *and* dark.
