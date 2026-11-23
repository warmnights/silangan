# Global CSS Reference

> **Purpose:** Lists what `css/style.css` already provides on every page: variables, global rules, utilities and shared components. It is the source of truth for what page stylesheets do **not** need to repeat.
> **Read when:** Before writing CSS in a page stylesheet, or when cleaning one up.
> **Related:** [overview.md](overview.md) · [colors.md](colors.md) · [css-writing-guide.md](css-writing-guide.md) · [css-property-guide.md](css-property-guide.md) · [html-writing-guide.md](html-writing-guide.md)

`style.css` is the final authority. This document summarizes it.

---

## 1. How to use this reference

- A declaration in a page stylesheet is a **duplicate** when it sets the same property to the same value that is already set here, directly or by inheritance. Duplicates are removed.
- A page stylesheet may **override** a global value with a different one. That is not a duplicate.
- A hard-coded value that matches a variable in `:root` (a hex or rgb color, a repeated shadow, a border radius equal to a `--radius-*` value) should use the variable.
- Buttons, cards and full-width sections take their colors from the `--button-*`, `--card-*` and `--band-*` variables. See [colors.md](colors.md).
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
| `:focus-visible` | 3px solid `var(--focus-ring)` outline with a 3px offset. `--focus-ring` is the primary color on the light bands. Dark surfaces (the footer, the random food box) set it to a lighter color so the ring stays visible. Cards are caramel now, so their ring is dark sangria. |
| `main:focus` | no outline |

Utility and shared classes: `.container`, `.full-bleed`, `.list-reset`, `.visually-hidden`, `.section-eyebrow`, `.skip-link`.

### Components styled in `style.css`

Page stylesheets do not restyle these.

- **Site header and navigation:** `.site-header`, `.site-nav`, `.site-nav-logo`, `.site-nav-toggle`, `.site-nav-links`, `.site-nav-dropdown`, `.site-nav-dropdown-menu`, `.site-nav-actions`, `.site-nav-cta`. This includes the hamburger menu below 1080px, where `nav.js` toggles `.is-open`. The header is sticky at `z-index: 10`, and the More menu is `z-index: 11`. The bar is dark glass: a `::before` layer on `.site-header`, as wide as the screen the way `.full-bleed` is, holds the see-through tint and the blur (`--header-bg` and `--header-blur`). The More menu and the open phone menu use the almost solid `--header-menu-bg`. A solid `--page-bg` bar replaces the glass in browsers without `backdrop-filter` and for visitors who ask for less transparency. The bar is 68px tall, or 61px at 1080px and below. That height is `--site-nav-height`, so anything that has to sit below the header (the Terms and Privacy contents box, the hero height, anchor offsets) uses the variable instead of a pixel value.
- **Footer:** `.footer`, `.footer-main`, `.footer-logo`, `.footer-brand`, `.footer-brand-tagline`, `.footer-links`, `.footer-about`, `.footer-explore-link`, `.footer-bottom`, `.footer-legal`. Footer text is `var(--color-background)` and `var(--color-background-secondary)` on a `var(--color-surface)` background.
- **Restaurant notice:** `.restaurant-preview-notice` and its `p` (used on the home page and Near Me).
- **Home page sections:** `.hero`, `.near-me-section`, `.fresh-finds-section`, `.trivia-section`, `.regional-discoveries-section`, `.popular-foods-section`, `.random-food-section` and their parts. These live in `style.css`, not in a page file. Each one is a full-width band (`full-bleed`) with a transparent background, so the page texture shows through, like the other pages. The bottom of the hero dissolves into the page: `.hero::after` fades the bottom of the hero into a solid mahogany base (`--mountain-base`), and `.near-me-section::before` fades that red into the page. The sun is masked just above the faded part so it does not show through. `--mountain-height` sizes the fade and the sun.

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
    border: 1px solid var(--button-border);
    border-radius: var(--radius-sm);

    color: var(--button-text);
    background-color: var(--button-bg);

    font-size: 0.9rem;
    font-weight: 600;
    line-height: 1.5;

    transition: background-color 0.2s ease, border-color 0.2s ease, color 0.2s ease;
}

.button:hover {
    border-color: var(--button-border-hover);

    color: var(--button-text-hover);
    background-color: var(--button-bg-hover);
}

.button-outline {
    border-color: var(--button-outline-border);

    color: var(--button-outline-text);
    background-color: transparent;
}
```

- Hover is the same on every button: `--button-bg-hover`, `--button-text-hover` and `--button-border-hover`, which start as the primary color. There is no per-page hover color.
- The page buttons (Log In, All Foods, Map, Near Me and Recipes) take the same `--button-*` variables. The full list is in [colors.md](colors.md#2-buttons).
- Button text is dark sangria (`--color-text`) on the golden caramel button, and light (`--color-background-secondary`) on the primary hover color.

### Cards

`.card` is the shared base for home page cards: border, radius, background and hover lift. Each card keeps its own class for its image height and text. `.card-on-secondary` is used when the card sits on a band that has its own background (`--band-bg-alt`). Both card backgrounds are golden caramel now, so it looks the same as `.card`, but the class stays so a different background can be set again. Card colors come from the `--card-*` variables. See [colors.md](colors.md#3-cards). `.trivia-card` stays separate from `.card` on purpose.

```css
.card {
    overflow: hidden;
    border: 1px solid var(--card-border);
    border-radius: var(--radius-md);

    color: var(--card-text);
    background-color: var(--card-bg);

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
| `.page-hero` | Background `var(--band-bg-alt)` (transparent). Always used with `full-bleed`. |
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

    --page-bg: #361214;

    --color-background: #DDC5A3;
    --color-background-secondary: #F2D8B5;
    --color-surface: #322C25;
    --color-card: #B28558;

    --color-text: #F2D8B5;
    --color-text-muted: #F2D8B5CC;
    --color-heading: #F2D8B5;

    --color-ink: #2C0E11;
    --color-ink-muted: #2C0E11B3;

    --color-primary: #60241E;
    --color-secondary: #D8B98B;
    --color-accent: #8E2F3C;

    --color-white: #FFFFFF;
    --color-black: #000000;

    --color-border: rgba(182, 139, 80, 0.35);

    --focus-ring: var(--color-secondary);

    /*

        QUICK COLOR CONTROLS

    */

    /*

        BUTTONS: Used by .button, .button-outline and the page buttons (log in, all foods, map, near me, recipes).

    */

    --button-bg: var(--color-card);
    --button-text: var(--color-ink);
    --button-border: var(--color-card);

    --button-bg-hover: var(--color-primary);
    --button-text-hover: var(--color-background-secondary);
    --button-border-hover: var(--color-primary);

    --button-outline-text: var(--color-surface);
    --button-outline-border: var(--color-surface);

    /*

        CARDS: Used by .card and the page cards (fresh finds, trivia, recipes, regions, top foods, all foods).

    */

    --card-bg: var(--color-card);
    --card-bg-alt: var(--color-card);
    --card-border: var(--color-border);

    --card-title: var(--color-ink);
    --card-text: var(--color-ink);
    --card-label: var(--color-ink);
    --card-link: var(--color-ink);

    /*

        HEADER: The navigation bar is dark glass over the page. Keep --header-bg at 75% or more so the links stay readable.

    */

    --header-bg: color-mix(in srgb, var(--page-bg) 80%, transparent); /* the glass tint of the bar */
    --header-menu-bg: color-mix(in srgb, var(--page-bg) 96%, transparent); /* the dropdown and the open phone menu */
    --header-blur: 14px; /* how blurred the page is behind the bar */

    /*

        FULL-WIDTH BANDS: Only the background. Used by every full-width section and the page hero. Both are transparent, so the page texture shows through. Set a color to make a band solid.

    */

    --band-bg: transparent;
    --band-bg-alt: transparent;

    --page-texture: url('../assets/images/backgrounds/texture-red.webp');

    --shadow-card: 0 8px 24px rgba(14, 4, 5, 0.5);

    --radius-sm: 8px;
    --radius-md: 12px;
    --radius-lg: 16px;
    --radius-pill: 999px;

    --container-width: 1400px;
    --site-nav-height: 68px;
}
```

| Radius | Used for |
| --- | --- |
| `--radius-sm` (8px) | Buttons, menus |
| `--radius-md` (12px) | Cards |
| `--radius-lg` (16px) | Large cards and images |
| `--radius-pill` (999px) | Tags and pills |

The three groups after `--color-border` are the quick color controls. Each starts as a base color, so a base color change still reaches everything that has not been overridden. What each one changes is in [colors.md](colors.md).

Notes:

- `--gutter-x` is not in `:root`. It is set per element (`.full-bleed` defaults it to `4%`). `--mountain-height` and `--sun-size` belong only to `.hero`.
- `--site-nav-height` is the real height of the header: 68px on desktop and 61px at 1080px and below, where the 44px menu button and its padding set it. The hero and the scroll offsets use it. When the header's padding, logo or button size changes, change this value to match.
- `--color-white` and `--color-black` are only for text over photos and for shadows. White is never used as text on `--color-accent`.
- Palette names: `--color-background` is Tan (`#DDC5A3`), the page and the darker band. `--color-background-secondary` is Cream, the lighter band and every page hero. `--color-card` is Golden Caramel (`#B28558`), the background of cards, buttons and the navigation bar. `--color-surface` is Smoked Moss (`#322C25`), the footer. `--page-bg` is the dark red base of the page (`#361214`), shown under the red texture. `--color-text` and `--color-heading` are Cream, the text on the red page. `--color-ink` is Dark Sangria (`#2C0E11`), the text on the light parts: cards, buttons, the header, chips and form fields. The hero keeps dark text because it sits on its own light sky, so `.hero` sets `--color-text` and `--color-heading` back to `--color-ink`. `--color-primary` is Mahogany, `--color-secondary` is Light Caramel (`#D8B98B`, used on the dark footer) and `--color-accent` is Wine.
- The page is dark red, so tan and cream are now used for text and for light chips and fields. Golden Caramel and Smoked Moss are the main colors. The other colors are chosen to keep the contrast below.
- `--focus-ring` is the color of the keyboard focus ring. It is light caramel on the red page (the hero sets it back to the primary color). `.card`, `.trivia-card`, `.restaurant-preview-notice` and the page cards set it to `--card-link` (dark sangria), the footer sets it to light caramel, and the random food box sets it to cream, so the ring stays visible on dark surfaces.
- Contrast on the red page texture (WCAG AA needs 4.5:1 for small text), measured on its lightest patch (`#4E1A1E`): cream text 10.2:1, muted cream about 8.2:1, light caramel (eyebrows, links, spans) 7.5:1. Mahogany (1.2:1) and wine (1.8:1) are too dark to use as text on the page, so page links and highlights use light caramel instead.
- Contrast on tan (chips and form fields): dark sangria 10.7:1.
- Contrast on `--color-card` (cards, buttons and the navigation bar): dark sangria text 5.4:1. Muted text and mahogany are under 4.5:1 there, so every card text level uses full dark sangria.
- Contrast on `--color-surface` (the footer): cream 10.0:1, tan 8.3:1, light caramel 7.4:1.
- Filled buttons: dark sangria on golden caramel 5.4:1, dark sangria on the light caramel hover color 9.5:1. Outline buttons are light caramel on the red page. Tan on primary (the random food box heading and the featured dish label) 7.1:1.
- The wine accent is 4.8:1 on tan and 5.8:1 on cream, so it still works on light chips and form fields (input focus border, social button hover). It is only 1.8:1 on the red page, so on the page the accent spans, eyebrows and focus rings use light caramel.
- Golden caramel is used only as a fill with dark sangria text. Cream on golden caramel is 2.4:1 and wine on it is 2.4:1, so neither is used on cards. Navigation hover and current-page links use dark sangria with an underline or bold weight, because mahogany on golden caramel is only 3.6:1. Button hover is light caramel with dark sangria text (9.5:1).
- Small labels over photos (island numbers, region tags, recipe categories) use a tan or caramel chip with `--color-ink`, never `--color-text` or `--color-heading`, because those are cream on the red page.
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

Page stylesheets and pages do not need any other script. The More dropdown is a `<details>` element and is not handled by the script (see [bugs.md](bugs.md), item 1).