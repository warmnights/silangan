# CSS Writing Guide

> **Purpose:** Describes how stylesheets are organized and written on Silangan: file structure, formatting, comments, variables, selectors, units, motion and breakpoints. The order of properties inside a rule is covered separately.
> **Read when:** Creating a stylesheet, adding a section, or editing CSS.
> **Related:** [overview.md](overview.md) · [colors.md](colors.md) · [css-property-guide.md](css-property-guide.md) · [html-writing-guide.md](html-writing-guide.md) · [global.md](global.md)

---

## 1. File structure

```
css/
├── style.css          shared styles, loaded on every page
├── near-me.css        styles for one page
├── auth.css           styles shared by login, sign up and forgot password
└── ...
```

- `style.css` holds everything shared: fonts, variables, global rules, utilities, buttons, cards, navigation, footer, and the home page sections.
- Each page has one stylesheet named after it, loaded **after** `style.css`. A stylesheet may serve several related pages (`auth.css`, `legal.css`).
- A page stylesheet adds only what is different on that page. Anything `style.css` already sets is not repeated. [global.md](global.md) lists what is already set.

### Order inside `style.css`

1. `@font-face` blocks
2. Breakpoints note
3. `:root` variables
4. Global rules (`*`, `html`, `body`, `img`, `a`, headings, `:focus-visible`)
5. Utilities (`.container`, `.full-bleed`, `.list-reset`, `.visually-hidden`)
6. Components in page order: skip link, header and navigation, breadcrumb and page heroes, buttons and the home hero, then the home page sections
7. Form fields and the footer
8. Media queries

### Order inside a page stylesheet

Sections follow the order they appear on the page. Media queries come last.

---

## 2. Formatting

- 4 spaces per level, no tabs, no trailing spaces.
- Selector and `{` on one line, one declaration per line, a space after the colon, a semicolon on every declaration.
- One blank line between rules.
- When a rule has several selectors, each is on its own line:

```css
.footer-bottom p,
.footer-bottom span {
    margin: 0;
}
```

- Numbers carry a leading zero (`0.2s`, `0.5rem`). Zero has no unit (`margin: 0`).
- Hex colors are uppercase (`#2C0E11`) and written only inside `:root`.
- Strings in selectors and `content` use double quotes (`[aria-current="page"]`, `content: ""`).
- Property order and blank lines inside a rule follow [css-property-guide.md](css-property-guide.md).

---

## 3. Comments

Comments explain **why**: a reason, a pitfall, or how a class is meant to be used. They do not repeat what the code says.

### Section banner

Each section starts with a banner. The name is in capitals, with a blank line above and below it inside the comment. Two blank lines come before the banner and two after it.

```css


/*


    SITE HEADER AND NAVIGATION BAR


*/


.site-header {
    ...
}
```

### Note

A note sits above the rule it explains, with a blank line before and after.

```css
/*

    1rem stops iPhones from zooming the page when a field is tapped

*/

.form-group input {
    ...
}
```

### Usage note

When a class has a particular way of being used, the comment shows it.

```css
/*

    .button is the filled button. Add .button-outline next to it
    for the outlined version: class="button button-outline"

*/
```

### Compact note

A one- or two-sentence note may be written compactly. It stays on one line, or it wraps with the continuation lines indented 3 spaces so they line up under the text. A blank line follows it.

```css
/* Use on cards that sit inside a section with its own background,
   so the card stays darker than the section behind it. */
```

### Media query label

```css
/* ---------- Tablets (up to 1024px) ---------- */
```

---

## 4. Variables

Reusable values live in `:root`, grouped with a blank line between groups.

| Kind | Naming | Examples |
| --- | --- | --- |
| Fonts | `--font-*` | `--font-heading`, `--font-body` |
| Colors | `--color-*` | `--color-primary`, `--color-text-muted` |
| Quick color controls | `--button-*`, `--card-*`, `--header-*`, `--band-*` | `--button-bg`, `--card-title`, `--header-bg`, `--band-bg-alt` |
| Shadows | `--shadow-*` | `--shadow-card` |
| Focus ring | `--focus-ring` | `--focus-ring` |
| Radius | `--radius-*` | `--radius-sm`, `--radius-md`, `--radius-lg`, `--radius-pill` |
| Sizes | named for the thing | `--container-width`, `--site-nav-height` |

- Colors in rules always come from a variable. Buttons, cards, the header and full-width sections use the quick controls (`--button-*`, `--card-*`, `--header-*`, `--band-*`). Everything else uses `var(--color-...)`. There are no hex or rgb values outside `:root`.
- The quick controls start as base colors (`--button-bg: var(--color-card);`). Each group (buttons, cards, header, bands) sits under a block comment that says what it changes. What each one changes is in [colors.md](colors.md).
- A see-through color is made with `color-mix(in srgb, var(--color-primary) 20%, transparent)`, not a new hex code.
- Border radius uses the `--radius-*` variables. `50%` is allowed for circles.
- A variable set inside a rule (`--gutter-x`, or `--color-heading` inside a card) is for a value that changes per element or per breakpoint. It is the first line of the rule.
- Names describe purpose, not appearance.

The full list is in [global.md](global.md).

---

## 5. Selectors

- **Classes only.** No id selectors and no `!important`.
- Class naming follows the [HTML guide](html-writing-guide.md#4-class-names).
- Tags inside a component are styled through the component class instead of giving each tag its own class:

```css
.fresh-find-info h3 { ... }
.fresh-find-info a { ... }
```

- `>` matches direct children only (`.fresh-find-card > img`, `.site-nav-dropdown > summary`).
- States sit directly under the rule they belong to, in this order: base, `:hover`, `:active`, `:focus` or `:focus-visible`, then pseudo-elements.
- Attribute selectors match state the HTML already carries (`[aria-current="page"]`, `[open]`).
- A modifier adds to its base class and does not redefine it (`.button`, then `.button-outline`).
- Page stylesheets start their class names with the page or feature name so they cannot clash.

---

## 6. Units and values

| Unit | Used for |
| --- | --- |
| `rem` | Font sizes, padding, margin, gap, most max-widths |
| `px` | Borders, fixed image heights, the 44px touch size, breakpoints |
| `%` | Side gutters (`4%`), column widths |
| `clamp()` | Fluid headings and gaps: `clamp(1.75rem, 4vw, 2.5rem)` |
| `calc()` | Mixing units: `calc(100svh - var(--site-nav-height))` |

- Sides and axes use logical properties: `padding-block`, `padding-inline`, `margin-inline`.
- Shorthand is the default (`margin: 0 0 0.75rem`).
- Font weights are `400`, `600` and `700`, because those are the only weights that load.
- Touch targets are at least 44px tall.
- A fixed `height` on an image is paired with `width: 100%` or `width: auto`.

---

## 7. Transitions and motion

- Transitions default to `0.2s ease` and list only the properties that change:

```css
transition: background-color 0.2s ease, border-color 0.2s ease, color 0.2s ease;
```

- `transition: all` is avoided.
- Card hover lifts by `translateY(-5px)`. Buttons do not lift on hover, and `.button:active` moves down `1px`.
- Anything that **moves** (`transform`, `scroll-behavior`) is switched off under `prefers-reduced-motion: reduce`. `style.css` has one block for the shared pieces (smooth scrolling and the `.card` lift). A page stylesheet whose own rules move something carries its own block. Color-only fades do not need one.

```css
@media (prefers-reduced-motion: reduce) {

    .card {
        transition: none;
    }

    .card:hover {
        transform: none;
    }
}
```

---

## 8. Breakpoints and media queries

The site is written **desktop first**, with `max-width` queries.

| Breakpoint | Used for |
| --- | --- |
| 1024px | Tablets |
| 900px | Small tablets: card rows become swipeable |
| 768px | Large phones, landscape |
| 640px | Phones |
| 480px | Small phones |
| 360px | Very small phones |

`style.css` adds three on purpose:

| Breakpoint | Used for |
| --- | --- |
| 1180px | Tightens nav spacing just before it collapses |
| 1080px | The nav becomes a hamburger menu. `nav.js` and the CSS use 1081px as the desktop check. |
| 800px | The home hero switches to its mobile layout |

It also has a few queries on orientation, height and pointer type for the nav and touch devices, and two for the glass header: one for visitors who ask for reduced transparency and one for browsers without backdrop blur. Other breakpoints are not added without a reason.

Rules:

- Media queries go at the end of the file under a `MEDIA QUERIES` banner. In `style.css`, three blocks sit beside the rules they change instead: the `@supports` fallback and the `prefers-reduced-transparency` query for the glass header (under the header rules), and the `prefers-reduced-motion` block (under `.card`). They have no label comment.
- They run from the largest breakpoint to the smallest.
- Each has a label comment, and a blank line follows the opening brace.
- Inside a query, related rules are grouped under short comments (`/* Section spacing */`).
- Only the properties that change are repeated, never the whole rule.

```css
/* ---------- Mobile landscape / large phones (up to 768px) ---------- */

@media (max-width: 768px) {

    /* Section spacing */

    .near-spots-section {
        padding-block: 3.5rem;
    }
}
```

---

## 9. Reusable layout patterns

| Pattern | How it works |
| --- | --- |
| Full-width section | Add `full-bleed` in the HTML. In CSS set only `--gutter-x` and `padding-block`. The gutter defaults to `4%`. |
| Card grid | `display: grid; gap: 1.5rem; grid-template-columns: repeat(3, 1fr);`. At 900px it becomes a sideways scroll row using `scroll-snap-type`. |
| Card | `.card` holds the border, radius, background and hover. The component class adds the image height and text. |
| Image in a fixed box | `width: 100%; height: ...; object-fit: cover;` |
| Text for screen readers only | `.visually-hidden` |
| List without bullets | `.list-reset`. It does not reset margin, so add `margin: 0` where needed. |
| Buttons | `.button`, with `.button-outline` for the outlined version. Colors and hover come from the `--button-*` variables. |

---

## 10. Not used

- `!important`
- Id selectors
- Hex or rgb colors outside `:root`
- JavaScript for anything CSS can do (the More dropdown is `<details>`, and hover and focus are CSS)
- A removed focus outline without a replacement (`:focus-visible` always shows a ring)
- Repeated shared styles in a page stylesheet

---

## 11. Stylesheet checklist

- [ ] Sections follow the page order, each with a banner
- [ ] Colors come from variables only: `--button-*`, `--card-*`, `--header-*` and `--band-*` for buttons, cards, the header and bands, otherwise `--color-*`
- [ ] Radius uses `var(--radius-*)`
- [ ] Property order follows the property guide
- [ ] States sit directly under their base rule
- [ ] Media queries are at the bottom, largest first, with labels
- [ ] Anything that moves has a `prefers-reduced-motion` rule
- [ ] No `!important`, no id selectors, nothing already set in `style.css`