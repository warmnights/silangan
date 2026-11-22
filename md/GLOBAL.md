# Global CSS Reference

> **Purpose:** Lists what `css/style.css` already provides on every page: variables, global rules, utilities and shared components. It is the source of truth for what page stylesheets do **not** need to repeat.
> **Read when:** Before writing CSS in a page stylesheet, or when cleaning one up.
> **Related:** [overview.md](overview.md) · [css-writing-guide.md](css-writing-guide.md) · [css-property-guide.md](css-property-guide.md) · [html-writing-guide.md](html-writing-guide.md)

`style.css` is the final authority. This document summarizes it.

---

## 1. How to use this reference

- A declaration in a page stylesheet is a **duplicate** when it sets the same property to the same value that is already set here, directly or by inheritance. Duplicates are removed.
- A page stylesheet may **override** a global value with a different one. That is not a duplicate.
- A hard-coded value that matches a variable in `:root` (a hex or rgb color, a repeated shadow, a border radius equal to a `--radius-*` value) should use the variable.
- There is no `!important` anywhere in the project, and no hex or rgb colors outside `:root`.

---

## 2. Page structure

The full markup rules are in [html-writing-guide.md](html-writing-guide.md). The parts that affect CSS:

- Every page wraps all its content in one `<div class="container">`. The container sets the global max width. Sections do not set their own `max-width` or `margin: 0 auto` for the page width. A narrower reading width inside a section (`max-width: 700px; margin: 0 auto` on centered text) is fine.
- The header is `<header class="site-header">` holding `<nav class="site-nav full-bleed">`. It is the same on every page. When the current page is inside the More menu, the More button highlights itself through CSS `:has()`, so no extra class is needed. The footer is `<footer class="footer full-bleed">`.
- `<main id="main" tabindex="-1">` is the target of the skip link. `main:focus` has no outline on purpose.
- A section whose background spans the full screen gets `full-bleed` in the HTML. Its page CSS does not repeat `width: 100vw`, `margin-left: calc(50% - 50vw)` or the side padding. It sets only `--gutter-x` and `padding-block`.
- `<ul class="list-reset">` removes bullets and left padding. It does not reset margin, so lists that need it set `margin: 0`.
- `class="visually-hidden"` hides text from the screen but keeps it for screen readers.

### Paths and the 404 page

Pages in `pages/` use `../` to reach the root. `index.html` uses no prefix. `404.html` uses root paths (`/css/style.css`) because it can be served from any URL, which works only when the site is served from a domain root, as on Vercel. Opened straight from a folder, `404.html` shows unstyled.

---

## 3. Images

- Every `<img>` has `width` and `height` attributes with the file's real pixel size. They reserve space so the page does not jump. CSS still controls the displayed size, and the global `height: auto` keeps the shape.
- A rule that sets a fixed `height` on an image also sets `width: 100%` or `width: auto`, or the image distorts.
- Attribute layout and lazy loading are in [html-writing-guide.md](html-writing-guide.md#6-images).

---

## 4. What is already set globally

| Selector | Set by `style.css` |
| --- | --- |
| `*`, `*::before`, `*::after` | `box-sizing: border-box` |
| `html` | smooth scrolling (off under `prefers-reduced-motion`), `scroll-padding-top: var(--site-nav-height)` |
| `body` | text color, background, `font-family: var(--font-body)`, `font-size: 1rem`, `line-height: 1.7`, `margin: 0`, `overflow-x: clip`, `min-height: 100vh` |
| `h1`, `h2`, `h3`, `h4` | `margin: 0 0 0.5rem`, heading color, heading font, weight 700, `line-height: 1.1` |
| `h2` | `font-size: clamp(1.75rem, 4vw, 2.5rem)` |
| `h3` | `font-size: 1.25rem` |
| `a` | `color: inherit`, no underline |
| `img`, `svg` | `display: block`, `max-width: 100%`, `height: auto` |
| `input`, `button` | `font-family: inherit` |
| `:focus-visible` | 3px solid `var(--color-primary)` outline with a 3px offset |
| `main:focus` | no outline |

Utility and shared classes: `.container`, `.full-bleed`, `.list-reset`, `.visually-hidden`, `.section-eyebrow`, `.skip-link`.

### Components styled in `style.css`

Page stylesheets do not restyle these.

- **Site header and navigation:** `.site-header`, `.site-nav`, `.site-nav-logo`, `.site-nav-toggle`, `.site-nav-links`, `.site-nav-dropdown`, `.site-nav-dropdown-menu`, `.site-nav-actions`, `.site-nav-cta`. This includes the hamburger menu below 1080px, where `nav.js` toggles `.is-open`. The header is sticky at `z-index: 10`, and the More menu is `z-index: 11`. The bar is 88px tall, or 69px at 1080px and below.
- **Footer:** `.footer`, `.footer-main`, `.footer-logo`, `.footer-brand`, `.footer-brand-tagline`, `.footer-links`, `.footer-about`, `.footer-explore-link`, `.footer-bottom`, `.footer-legal`. Footer text is `var(--color-background)` on a `var(--color-primary)` background.
- **Restaurant notice:** `.restaurant-preview-notice` and its `p` (used on the home page and Near Me).
- **Home page sections:** `.hero`, `.near-me-section`, `.fresh-finds-section`, `.trivia-section`, `.regional-discoveries-section`, `.popular-foods-section`, `.random-food-section` and their parts. These live in `style.css`, not in a page file.

---

## 5. Shared components

### Buttons

`.button` is the filled button. `.button-outline` is added next to it for the outlined version: `class="button button-outline"`.

```css
.button {
    display: inline-flex;
    justify-content: center;
    align-items: center;

    min-height: 44px;
    padding: 0.9rem 1.5rem;
    border: 1px solid var(--color-primary);
    border-radius: var(--radius-sm);

    color: var(--color-background);
    background-color: var(--color-primary);

    font-size: 0.9rem;
    font-weight: 600;
    line-height: 1.5;

    transition: background-color 0.2s ease, border-color 0.2s ease, color 0.2s ease;
}

.button:hover {
    border-color: var(--color-accent);

    background-color: var(--color-accent);
}

.button-outline {
    color: var(--color-primary);
    background-color: transparent;
}
```

- Hover is the accent color on every button. There is no per-page button class or per-page hover color.
- Text on the accent color uses `var(--color-background)`, not white. White on the accent reaches only 3.35:1 contrast, and dark text reaches 5.54:1.

### Cards

`.card` is the shared base for home page cards: border, radius, background and hover lift. Each card keeps its own class for its image height and text. `.card-on-secondary` is used when the card sits on a section that already uses `--color-background-secondary`, so the card stays darker than the section. `.trivia-card` stays separate from `.card` on purpose.

```css
.card {
    overflow: hidden;
    border: 1px solid var(--color-border);
    border-radius: var(--radius-md);

    background-color: var(--color-background-secondary);

    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.card:hover {
    box-shadow: var(--shadow-card);

    transform: translateY(-5px);
}
```

### Page heroes

| Class | Purpose |
| --- | --- |
| `.page-hero` | Background `var(--color-background-secondary)`. Always used with `full-bleed`. |
| `.page-hero-centered` | `padding-block: 3.5rem`, centered text |
| `.page-hero-content` | `max-width: 850px`, centered. Holds the breadcrumb, eyebrow, `<h1>` and paragraph. |
| `.page-hero-split` | Two columns (`1fr 1fr`), `gap: 4rem`, `min-height: 580px`, `padding-block: 5rem`, `--gutter-x: 8%`. Used by Regions and Top Foods. |
| `.page-hero-split-content`, `.page-hero-split-image` | The text side, and the image side (430px tall, `--radius-lg`, `object-fit: cover`) |

Pages add only what differs, such as a shorter image on small screens. The image caption `.page-hero-split-image-caption` is styled in `regions.css`, because only the Regions page uses it.

### Breadcrumb

`.breadcrumb` (`margin-bottom: 2rem`, `font-size: 0.875rem`), `.breadcrumb-list` (used with `list-reset`) and `.breadcrumb-centered`. The current page is the last `<li aria-current="page">`.

### Form fields

`.form-group` is a vertical stack (`flex-direction: column`, `gap: 0.6rem`, `margin-bottom: 1.5rem`) for a label and its input. It is used on Log In, Sign Up and Forgot Password.

---

## 6. Sections of `style.css`

In order: Breakpoints, Variables, Global, Skip link, Site header and navigation bar, Breadcrumb (also holds the page heroes), Buttons (also holds the home hero), Near me preview, Bicol specialties (fresh finds), Did you know, From around the Philippines, Filipino dishes to explore (popular foods), Random food, Form fields, Footer, Media queries.

All media queries for `style.css` are collected at the bottom. Breakpoints are listed in [css-writing-guide.md](css-writing-guide.md#8-breakpoints-and-media-queries).

---

## 7. Variables

```css
:root {
    --font-heading: 'Fraunces', Georgia, serif;
    --font-body: 'Poppins', system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;

    --color-background: #2B0609;
    --color-background-secondary: #4A0A14;
    --color-surface: #3A0810;

    --color-text: #FBF1DC;
    --color-text-muted: #FBF1DCB3;
    --color-heading: #F0C766;

    --color-primary: #E8B84A;
    --color-secondary: #C98F47;
    --color-accent: #E8633A;

    --color-white: #FFFFFF;
    --color-black: #000000;

    --color-border: rgba(232, 184, 74, 0.28);

    --shadow-card: 0 8px 24px rgba(10, 0, 2, 0.5);

    --radius-sm: 8px;
    --radius-md: 12px;
    --radius-lg: 16px;
    --radius-pill: 999px;

    --container-width: 1400px;
    --site-nav-height: 88px;
}
```

| Radius | Used for |
| --- | --- |
| `--radius-sm` (8px) | Buttons, menus |
| `--radius-md` (12px) | Cards |
| `--radius-lg` (16px) | Large cards and images |
| `--radius-pill` (999px) | Tags and pills |

Notes:

- `--gutter-x` is not in `:root`. It is set per element (`.full-bleed` defaults it to `4%`). `--mountain-height` and `--sun-size` belong only to `.hero`.
- `--color-white` and `--color-black` are only for text over photos and for shadows. White is never used as text on `--color-accent`.
- Contrast on `--color-background` (WCAG AA needs 4.5:1 for small text): text 16.5:1, muted text 8.4:1, heading gold 11.5:1, primary gold 10:1, orange accent 5.5:1.
- Contrast on `--color-background-secondary` (hero bands and cards): text 13.9:1, muted text 7.3:1, heading gold 9.7:1, orange accent 4.7:1. The orange `.section-eyebrow` on a secondary background is the closest pair to the limit, so it is not made darker or smaller.

---

## 8. Fonts

The fonts are self-hosted `.woff2` files in `assets/fonts/`, declared with `font-display: swap`. Only weights 400, 600 and 700 are loaded, so only those are used.

| Family | Files | Used for |
| --- | --- | --- |
| Fraunces | Regular (400), SemiBold (600), Bold (700) | Headings (`--font-heading`) |
| Poppins | Regular (400), Italic (400), SemiBold (600), Bold (700) | Body text (`--font-body`) |

Each page preloads Poppins Regular and Fraunces Bold (see [html-writing-guide.md](html-writing-guide.md#1-page-skeleton)).

---

## 9. Using `.full-bleed`

Add the class in the HTML, then set only the gutter and the vertical spacing in CSS. Leave `--gutter-x` out when the default `4%` is wanted.

```css
.hero {
    --gutter-x: 1.5rem;

    padding-block: 3rem 5rem;
}

@media (max-width: 480px) {
    .hero {
        --gutter-x: 1.25rem;
    }
}
```

---

## 10. Reduced motion

`style.css` has one `@media (prefers-reduced-motion: reduce)` block that turns off smooth scrolling and the `.card` transition and lift. A page stylesheet whose own rules move something (hover lifts, transforms) carries its own block. See [css-writing-guide.md](css-writing-guide.md#7-transitions-and-motion).

---

## 11. JavaScript

`js/nav.js` is the only script. It:

- adds and removes `.is-open` on `.site-nav`
- updates `aria-expanded` and `aria-label` on the toggle button
- closes the menu on Escape, on a click outside it, on a link click, and when the window grows past 1080px

Page stylesheets and pages do not need any other script. The More dropdown is a `<details>` element and is not handled by the script (see [bugs.md](bugs.md), item 2).