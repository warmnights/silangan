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

- `style.css` holds everything shared: fonts, variables, global rules, utilities, the skip link, header and navigation, buttons, labels and links, the hero, the plates, the home page sections and the footer.
- Each page has one stylesheet named after it, loaded **after** `style.css`. A stylesheet may serve several related pages (`auth.css`, `legal.css`).
- A page stylesheet adds only what is different on that page. Anything `style.css` already sets is not repeated. [global.md](global.md) lists what is already set.

### Order inside `style.css`

1. `@font-face` blocks (Cardo, then Poppins)
2. Breakpoints note
3. `:root` variables
4. Global rules and utilities, under one `GLOBAL` banner: `*`, `html`, `body`, `img`, `a`, form elements, headings, `p`, `:focus-visible`, then `.container`, `.full-bleed`, `.list-reset` and `.visually-hidden`
5. Components in page order: skip link, header and navigation, buttons, labels and links
6. The hero, then the plates
7. The home page sections: one shared spacing rule, then one banner per section in page order
8. The footer
9. Media queries

### Order inside a page stylesheet

Sections follow the order they appear on the page. Media queries come last.

---

## 2. Formatting

- 4 spaces per level, no tabs, no trailing spaces.
- Selector and `{` on one line, one declaration per line, a space after the colon, a semicolon on every declaration.
- One blank line between rules. Two blank lines before and after a section banner. Never three in a row.
- When a rule has several selectors, each is on its own line:

```css
.footer-bottom p,
.footer-bottom span {
    margin: 0;
}
```

- Numbers carry a leading zero (`0.2s`, `0.5rem`). Zero has no unit (`margin: 0`).
- A space follows every comma inside a value: `clamp(2.5rem, 6vw, 4.5rem)`, `color-mix(in srgb, var(--page-bg) 40%, transparent)`.
- Hex colors are uppercase (`#06170F`) and written only inside `:root`.
- Strings in selectors and `content` use double quotes (`[aria-current="page"]`, `content: ""`). Font names and `url()` paths use single quotes (`'Cardo'`, `url('../assets/fonts/Cardo-Regular.woff2')`). The one exception is `"Segoe UI"` in the system font fallback list.
- Property order and blank lines inside a rule follow [css-property-guide.md](css-property-guide.md).

---

## 3. Comments

Comments explain **why**: a reason, a pitfall, or how a class is meant to be used. They do not repeat what the code says. They are plain sentences, and they say what to change and where when that helps (`Change --section-space in :root to move all of them at once.`).

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

- A banner is named for what the section shows. A short working name goes in brackets (`FILIPINO DISHES TO EXPLORE (POPULAR FOODS)`). A component banner may add a colon and a short description (`PLATES: TOP-DOWN DISHES FLOATING ON THE GREEN`).
- The `BREAKPOINTS` banner holds its own list. A blank line follows the name, then the text.

```css


/*


    BREAKPOINTS

    This file uses three max-width breakpoints:

    1080px  the nav becomes a hamburger menu (nav.js checks 1081px)
    900px   the hero, the plates, the regions and the footer stack
    640px   small phones

    It also has two queries on pointer type, for comfortable touch targets.


*/
```

### Note

A note sits above the rule it explains, with a blank line before and after. It is wrapped by hand at about 75 characters, and the text is indented 4 spaces.

```css
/*

    The skip link moves focus to <main> (tabindex="-1"). It isn't
    interactive, so it doesn't need the focus ring the links and
    buttons get.

*/

main:focus {
    outline: none;
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

### Variable group note

Inside `:root`, each group of quick controls starts with a block comment. It is one line, named in capitals, followed by a colon. Lines that explain single variables sit under it, indented 4 more spaces. These lines are not wrapped.

```css
    /*

        HEADER: The navigation bar is dark glass.
            --header-bg is the see-through tint
            --header-blur is how much the page behind it is blurred
            --header-menu-bg is the almost solid tint behind the dropdown and the open phone menu
            Keep --header-bg at 70% or more so the links stay readable.

    */
```

`QUICK CONTROLS` is a comment of its own above the first group.

### Compact note

A one- or two-sentence note may be written compactly. It stays on one line, or it wraps with the continuation lines indented 3 spaces so they line up under the text. A blank line follows it. It sits under a media query label, or inside a query above the rules it explains.

```css
/* The site nav switches to a hamburger menu. The links and buttons are hidden
   until the menu button is pressed (nav.js adds/removes .is-open). */
```

### Media query label

```css
/* ---------- Tablets landscape and smaller (up to 1080px) ---------- */
```

Inside a query, a short label groups related rules: `/* Menu button */`, `/* Closed and open menu */`.

---

## 4. Variables

Reusable values live in `:root`, grouped with a blank line between groups.

| Kind | Naming | Examples |
| --- | --- | --- |
| Fonts | `--font-*` | `--font-heading`, `--font-body` |
| Page background | `--page-*` | `--page-bg`, `--page-bg-top`, `--page-glow`, `--page-gradient` |
| Colors | `--color-*` | `--color-text`, `--color-primary`, `--color-ink`, `--color-border` |
| Quick controls | `--button-*`, `--header-*`, `--link-*`, `--label-*`, `--field-*`, `--plate-*`, `--footer-bg` | `--button-bg`, `--header-bg`, `--label-size`, `--plate-size` |
| Type scale | `--text-*` | `--text-base`, `--text-small`, `--text-h1`, `--text-h2`, `--text-h3` |
| Spacing | named for the thing | `--page-gutter`, `--section-space`, `--section-gap` |
| Repeated values | named for the thing | `--border-line`, `--transition`, `--logo-spacing` |
| Radius | `--radius-*` | `--radius-md`, `--radius-pill` |
| Shadows | `--shadow-*` | `--shadow-menu` |
| Focus ring | `--focus-ring` | `--focus-ring` |
| Sizes | named for the thing | `--container-width`, `--site-nav-height`, `--hero-max-height` |

- The base palette is small and is written in hex only inside `:root`: three page greens, the cream text color, the gold primary and black for shade. Every other color is built from those.
- Colors in rules always come from a variable. Buttons, the header, links, labels, form fields, plates and the footer use their quick controls (`--button-*`, `--header-*`, `--link-*`, `--label-*`, `--field-*`, `--plate-*`, `--footer-bg`). Everything else uses `var(--color-...)`. There are no hex or rgb values outside `:root`.
- A see-through or softened color is made with `color-mix(in srgb, var(--color-primary) 35%, transparent)`, not a new hex code. This is how `--color-text-muted`, `--color-border`, `--header-bg`, `--field-bg`, `--footer-bg` and `--shadow-menu` are written.
- A color that is only another name for one already there is an alias (`--color-heading: var(--color-text);`, `--color-ink: var(--page-bg);`).
- The quick controls start as base colors (`--button-bg: var(--color-primary);`). Each group sits under a block comment that says what it changes. What each one changes is in [colors.md](colors.md).
- A value that repeats becomes a variable: a border (`--border-line`), the transition (`--transition`), a font size used for a role (`--text-small`). A size used once is written in `rem` in its rule.
- Border radius uses the `--radius-*` variables. `50%` is allowed for circles.
- A variable set inside a rule (`--gutter-x` on `.full-bleed`) is for a value that changes per element or per breakpoint. It is the first line of the rule. A media query may set it again (`--plate-size-large` on `.hero-plate` at 900px).
- Names describe purpose, not appearance.
- Every variable in `:root` is used. A variable that nothing reads is deleted.

The full list is in [global.md](global.md).

---

## 5. Selectors

- **Classes only** for styling. No id selectors and no `!important`.
- Class naming follows the [HTML guide](html-writing-guide.md#4-class-names).
- Global rules use the tag (`h1`, `a`, `img`). Tags inside a component are styled through the component class instead of giving each tag its own class:

```css
.region-name h3 { ... }
.footer-links h3 { ... }
.hero-title em { ... }
```

- `>` matches direct children only (`.footer-brand > p`, `.site-nav-dropdown > summary`).
- A second class raises the specificity only where one component sits inside another: `.hero-plate .plate-image`, `.trivia-content .plate-image`.
- States sit directly under the rule they belong to, in this order: base, `:hover`, `:active`, `:focus` or `:focus-visible`, then pseudo-elements. A modifier comes after its base rule and its states, with its own states under it (`.button`, `.button:hover`, `.button:active`, `.button-outline`, `.button-outline:hover`).
- Attribute selectors match state the HTML already carries (`[aria-current="page"]`, `[aria-expanded="true"]`, `[open]`). Form states use `:disabled` and `::placeholder`.
- Several selectors that share the same declarations share one rule, each on its own line (`.fresh-finds-section, .popular-foods-section, ...`).
- `:nth-child` is used rarely, and only with a note that says why (the middle plate sits lower).
- Page stylesheets start their class names with the page or feature name so they cannot clash.

---

## 6. Units and values

| Unit | Used for |
| --- | --- |
| `rem` | Font sizes, padding, margin, gap, most max-widths |
| `px` | Borders, icon sizes, the 44px touch size, fixed bar sizes (`--site-nav-height`), blur, breakpoints |
| `em` | Letter spacing and underline offset, so they follow the font size |
| `fr` | Grid columns (`1.1fr 0.9fr`, `1fr 2fr auto`) |
| `clamp()` | Fluid headings, gutters and section space: `clamp(2.5rem, 6vw, 4.5rem)` |
| `min()` and `calc()` | Mixing units and capping a size: `min(calc(100svh - var(--site-nav-height)), var(--hero-max-height))` |
| `vw` | Fluid sizes inside `clamp()`, and the full-bleed width |

- Sides and axes use logical properties: `margin-block-start`, `margin-inline-start`, `padding-block`, `padding-inline`, `border-block-start`, `border-block-end`. There is no `margin-top`, `padding-left` or `border-bottom`. Shorthand is used when all sides are set (`margin: 0 0 0.75rem`, `margin: 0 auto`). `top`, `right`, `bottom`, `left` stay for positioned elements.
- A newer unit gets an older line before it, so older browsers still work. The older value comes first (`min-height` with `100vh`, then again with `100svh`; `max-height: 100vh` then `100dvh`). This is the one case where a property appears twice in a rule.
- A vendor-prefixed property sits right after its standard version (`backdrop-filter`, then `-webkit-backdrop-filter`).
- Font weights are only the ones that load. Cardo loads 400 and 700, Poppins loads 400, 600 and 700, and both load an italic at 400. Headings use 400, so the browser never fakes a bold in Cardo, and buttons use 600.
- Touch targets are at least 44px tall: `min-height: 44px` on buttons, and on links in the hit areas query.
- Images keep their shape. `img` and `svg` are `display: block; max-width: 100%; height: auto` everywhere. A round or fixed frame uses `aspect-ratio` (`.plate-image`), and an image inside it is sized with `width`.

---

## 7. Transitions and motion

- Transitions use the `--transition` variable (`0.2s ease`) and list only the properties that change:

```css
transition: background-color var(--transition), border-color var(--transition), color var(--transition);
```

- `transition: all` is avoided.
- Hover changes colors only. Nothing lifts or moves on hover, and `.button:active` moves down `1px`.
- Anything that **moves** (`transform`, `scroll-behavior`) is switched off under `prefers-reduced-motion: reduce`. `style.css` has one block for the shared pieces: smooth scrolling, the button press and the flipping menu arrow. A page stylesheet whose own rules move something carries its own block. Color-only fades do not need one.
- A `transform` that only places something and does not animate (the skip link) is not switched off.
- The reduced motion block has a note above it that names what it switches off:

```css
/*

    Smooth scrolling (html), the button press (.button:active) and the
    flipping menu arrow all move, so they are switched off for visitors
    who ask for less motion.

*/

@media (prefers-reduced-motion: reduce) {

    html {
        scroll-behavior: auto;
    }

    .button:active {
        transform: none;
    }

    .site-nav-dropdown[open] .site-nav-chevron {
        transform: none;
    }
}
```

---

## 8. Breakpoints and media queries

The site is written **desktop first**, with `max-width` queries. There are three breakpoints:

| Breakpoint | Used for |
| --- | --- |
| 1080px | The nav becomes a hamburger menu. `nav.js` and the CSS use 1081px as the desktop check. |
| 900px | Two-column blocks stack into one column (the hero, the halo-halo fact, the plates, the regions and the near me notice), and the footer goes to two columns |
| 640px | Small phones |

Three other queries are used on purpose:

| Query | Used for |
| --- | --- |
| `(max-width: 900px), (any-pointer: coarse)` | Comfortable hit areas: links and buttons get `min-height: 44px` on small screens and on any touch screen |
| `(min-width: 1081px) and (any-pointer: coarse)` | Touch screens on wide layouts: the nav links are centered inside their taller hit area |
| `@supports not (backdrop-filter ...)` and `prefers-reduced-transparency` | The glass header becomes a solid bar |

Other breakpoints are not added without a reason.

Rules:

- Media queries go at the end of the file under a `MEDIA QUERIES` banner. In `style.css`, two groups sit beside the rules they change instead: the `@supports` fallback and the `prefers-reduced-transparency` query for the glass header (after the header rules, under one note), and the `prefers-reduced-motion` block (after the button rules, under its own note).
- Width queries run from the largest breakpoint to the smallest. A `min-width` query that adds something back for touch screens comes last.
- Each has a label comment, and a blank line follows the opening brace.
- A compact note may sit under the label to say what the query does, and inside a query above the rules it explains.
- Inside a query, related rules are grouped under short comments (`/* Menu button */`).
- Only the properties that change are repeated, never the whole rule. A variable that changes at a breakpoint is set on the first line of its rule (`--plate-size-large: 20rem;`).
- Stacked grids use `grid-template-columns: minmax(0, 1fr)`, so the content can shrink to the screen instead of keeping its default width.

```css
/* ---------- Small tablets (up to 900px) ---------- */

@media (max-width: 900px) {

    /* Hero, plates and the halo-halo fact stack into one column.
       minmax(0, 1fr) lets the search box shrink to the screen instead
       of keeping its default input width. */

    .hero-content,
    .trivia-content {
        gap: 2.5rem;
        grid-template-columns: minmax(0, 1fr);
    }

    .hero-plate {
        --plate-size-large: 20rem;

        order: -1;
    }
}
```

---

## 9. Reusable layout patterns

| Pattern | How it works |
| --- | --- |
| Full-width section | Add `full-bleed` in the HTML. In CSS set only `--gutter-x` and `padding-block`. The gutter defaults to `--page-gutter`, so changing that variable moves every section's side space at once. |
| Home section spacing | The home sections share one rule with `padding-block: var(--section-space)`. A new home section is added to that list. |
| Section heading | `.section-header` is centered, `40rem` wide, and ends with `--section-gap` of space. |
| Two columns | `display: grid; align-items: center; gap: 3rem; grid-template-columns: 1.1fr 0.9fr;`. At 900px it becomes `minmax(0, 1fr)`. |
| Plate (round dish image) | `.plate-image` is a thin ring with `aspect-ratio: 1; border-radius: 50%`. Its size is `--plate-size`, and `--plate-fill` is how much of the ring the dish fills. The ring keeps the layout if an image is missing. |
| Row of plates | `.plates-grid` has three columns and becomes one at 900px. The middle plate sits lower, and the offset is reset at 900px. |
| Ruled list | `.regions-list` has a top border and each `.region` a bottom border, so the lines run between rows. Columns are `1fr 2fr auto` and become one at 900px. |
| Buttons | `.button` is the filled pill. `.button-outline` is added next to it for the outlined version. Colors and hover come from the `--button-*` variables. |
| Label | `.label` is the small gold line above a name or heading. It is typed in capitals in the HTML, so there is no `text-transform`. |
| Text link | `.text-link` is gold with a thin underline, and turns cream on hover. |
| Text for screen readers only | `.visually-hidden` |
| List without bullets | `.list-reset`. It does not reset margin, so add `margin: 0` where needed. |

---

## 10. Not used

- `!important`
- Id selectors
- Hex or rgb colors outside `:root`
- `margin-top`, `margin-bottom`, `margin-left`, `margin-right`, the same for `padding`, and `border-top`, `border-bottom`, `border-left`, `border-right` (use the logical forms)
- `transition: all`
- `text-transform` (labels are typed in capitals in the HTML)
- JavaScript for anything CSS can do (the More dropdown is `<details>`, and hover and focus are CSS)
- A removed focus outline without a replacement. `main:focus { outline: none; }` is the one exception, because `<main>` is not interactive, and it carries a note.
- A variable that nothing uses
- Repeated shared styles in a page stylesheet

---

## 11. Stylesheet checklist

- [ ] Sections follow the page order, each with a banner
- [ ] Colors come from variables only: the quick controls for buttons, the header, links, labels, form fields, plates and the footer, otherwise `--color-*`
- [ ] A color that is a softer version of another uses `color-mix()`, not a new hex
- [ ] Radius uses `var(--radius-*)`
- [ ] Property order follows the property guide
- [ ] Sides and axes use logical properties
- [ ] Transitions use `var(--transition)` and name their properties
- [ ] States sit directly under their base rule
- [ ] Media queries are at the bottom, largest first, with labels
- [ ] Anything that moves has a `prefers-reduced-motion` rule
- [ ] Every variable in `:root` is used
- [ ] No `!important`, no id selectors, nothing already set in `style.css`