# Silangan Code Style Guide (compact)

> Merges the HTML guide, CSS guide and CSS property-order guide. Rules are kept, prose and repeated examples are cut. Follow it exactly. When unsure, copy the nearest existing pattern in `index.html` / `css/style.css`.
> Not included here: `global.md` (shared classes/variables list) and `colors.md`. Check them before inventing a class or variable.

---

# PART 1: HTML

## 1.1 Head (same on every page)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Page Name | Silangan</title>
    <meta name="description" content="One or two full sentences ending with a period.">

    <link rel="icon" href="assets/images/logo/favicon.svg" type="image/svg+xml">
    <link rel="preload" href="assets/fonts/Cardo-Regular.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="preload" href="assets/fonts/Poppins-Regular.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="stylesheet" href="css/style.css">
    <script src="js/nav.js" defer></script>
</head>
<body>
```

- Three blocks separated by blank lines (2 metas / title+description / links+script). Page stylesheet goes right after `style.css`, only if the page needs one.
- Every head tag on ONE line, however long. `<head>`/`<body>` at column 0, children indented 4.
- Home title: `Filipino Food &amp; Culture | Silangan`. Use `&amp;` in titles.
- Void tags have no closing slash. `<body>` has no class/attributes.
- Paths: `index.html` uses `pages/x.html`, `assets/...`, `css/...`. Files in `pages/` use `x.html`, `../assets/...`, `../index.html`, `../css/...`, `../js/...`. One prefix style per page; when copying header/footer/sections into `pages/`, add `../` to home/assets/CSS/JS and drop `pages/` from page links.

## 1.2 Page structure

```html
<body>
    <div class="container">
        <a href="#main" class="skip-link">Skip to main content</a>

        <header class="site-header">...</header>

        <main id="main" tabindex="-1">
            <section class="hero full-bleed">...</section>

            <section class="x-section full-bleed" aria-labelledby="x-title">...</section>
        </main>

        <footer class="footer full-bleed">...</footer>
    </div>
</body>
</html>
```

- `.container` is the only child of `<body>`. Skip link first (no blank line above, blank line after). Blank line between skip link / header / main / footer.
- Blank line between sections; none after `<main>` opens or before it closes.
- Every section has `full-bleed`. Section classes end in `-section` (hero is `hero`). First section is the hero with the only `<h1>`.
- Headings never skip: `h1` hero, `h2` sections, `h3` items and footer headings.
- Footer is last inside container, outside `<main>`. File ends at `</html>`.

## 1.3 Formatting

- 4 spaces, no tabs, no trailing spaces. One element per line.
- Short text stays on one line with its tag: `<h3>Kinunot</h3>`, `<li><a href="...">Top Foods</a></li>`.
- Longer than one line: text on its own lines inside the tag, hand-wrapped at ~60 chars.
- Text ALWAYS on own lines (even short): `a.text-link` in plates/regions/footer About, `a.button` (+`button-outline`), `a.site-nav-cta`, `a.footer-logo`, `span.footer-brand-tagline`, `p`/`span` in `footer-bottom`, `<summary>` (text then chevron svg), the `<em>` in the hero `<h1>`.
- Stay on one line: `a.site-nav-logo`, the two `text-link`s in `search-suggestions`, every `<li><a>`.
- Straight quotes/apostrophes only (`country's`, `"Halo-Halo"`), no curly quotes or `&quot;`. Type `©` and `♥` directly. No `<br>` on home page.

**Attributes:** 1-2 on one line; 3 on one line if short else split; 4+ always split. "Short" = whole tag incl. indent fits ~105 chars (a long 1-2 attribute tag also splits). Head tags exempt. Split tag: name on first line, one attribute per line indented +4, `>` ends the last attribute.

```html
<img
    loading="lazy"
    src="assets/images/plates/kinunot.png"
    width="800"
    height="800"
    alt="Kinunot, seen from above">
```

**Attribute order:** identity (`class`, `id`, `href`, `src`, `type`, `name`), then size/content (`width`, `height`, `maxlength`, `alt`, `placeholder`), then `aria-*`, then booleans (`disabled`).
`a`: href, class, aria-* · `a` current: href, aria-current · `img`: loading, src, width, height, alt (first image has no `loading`) · `section`: class, aria-labelledby · `h2`: id · `nav`: class, aria-label · `main`: id, tabindex · `ul`: class, id · menu `button`: class, type, aria-label, aria-expanded, aria-controls · form `button`: type, disabled · `input`: type, name, maxlength, placeholder, aria-label, disabled · `form`: class, action, method · `svg`: class, viewBox, aria-hidden · decorative `span`: class, aria-hidden.

**Blank lines:** BLANK between: sections; top-level parts of a component (nav: logo / button / list / actions); image block and text block; each piece of a plate/text block (label / heading / description / link); sibling `<li class="plate">` / `<li class="region">`; hero parts (h1 / p / form / suggestions); two halves of hero/content; footer-main columns; footer-bottom parts.
NO blank between: section opening and its first child; `h2` and `p` in section-header; `h3` and `p.label` in region-name; form controls; suggestion row; footer-brand parts; footer-links `h3`+`ul`; footer-about `h3`,`p`,`a`; CTA block parts; the two menu SVGs; plain `<li><a>` rows; `<ul>` opening and first item.

## 1.4 Classes and ids

- Lowercase-hyphen, flat (no BEM, no `__`/`--`). Component prefix + suffix per part: `plate`, `plate-image`, `plate-description`. Lists: `plates-grid`, `regions-list`. Feature prefixes: `site-nav-`, `hero-`, `trivia-`, `near-me-`, `random-food-`, `footer-`.
- Section class = feature + `-section`; inner wrapper = feature + `-content`. Variation adds suffix and both are written: `button button-outline`. Utilities last: `hero full-bleed`, `site-nav-links list-reset`. State classes `is-` (JS only).
- Name by what it is, not how it looks. Shared classes: `.button .button-outline .full-bleed .list-reset .label .text-link .section-header .plate .plate-image .visually-hidden`.
- Ids never for styling. Only: skip target `main`, `aria-controls` target (`site-nav-menu`), `aria-labelledby` target = section name minus `-section` plus `-title` (`trivia-section` -> `trivia-title`).
- Image-beside-text blocks put the text in a plain classless `<div>` (in `.plate`, `.hero-content`, `.trivia-content`).

## 1.5 Header (copy to every page; change only `aria-current` and path prefix)

```html
<header class="site-header">
    <nav class="site-nav full-bleed" aria-label="Main navigation">
        <a href="index.html" class="site-nav-logo">SILANGAN</a>

        <button
            class="site-nav-toggle"
            type="button"
            aria-label="Open menu"
            aria-expanded="false"
            aria-controls="site-nav-menu">
            <svg class="icon-menu" viewBox="0 0 24 24" aria-hidden="true">
                <path d="M4 7h16M4 12h16M4 17h16"></path>
            </svg>
            <svg class="icon-close" viewBox="0 0 24 24" aria-hidden="true">
                <path d="M6 6l12 12M18 6L6 18"></path>
            </svg>
        </button>

        <ul class="site-nav-links list-reset" id="site-nav-menu">
            <li><a href="index.html" aria-current="page">Home</a></li>
            <li><a href="pages/top-foods.html">Top Foods</a></li>
            <li><a href="pages/foods.html">All Foods</a></li>
            <li><a href="pages/regions.html">Regions</a></li>
            <li><a href="pages/near-me.html">Near Me</a></li>
            <li>
                <details class="site-nav-dropdown">
                    <summary>
                        More
                        <svg class="site-nav-chevron" viewBox="0 0 24 24" aria-hidden="true">
                            <path d="M6 9l6 6 6-6"></path>
                        </svg>
                    </summary>
                    <ul class="site-nav-dropdown-menu list-reset">
                        <li><a href="pages/recipes.html">Recipes</a></li>
                        <li><a href="pages/map.html">Map</a></li>
                        <li><a href="pages/random-food.html">Random Food</a></li>
                    </ul>
                </details>
            </li>
        </ul>

        <div class="site-nav-actions">
            <span class="site-nav-divider" aria-hidden="true"></span>

            <a href="pages/login.html" class="site-nav-cta">
                Log In
            </a>
        </div>
    </nav>
</header>
```

`aria-current="page"` only on the current page's link (remove from Home elsewhere). `<path>` always `<path ...></path>`, never self-closed. Logo text `SILANGAN` capitals.

## 1.6 Sections

**Layout A (header + list):** `<section>` > `<header class="section-header">` (`<h2 id>` + one `<p>`), blank line, `<ul class="plates-grid list-reset">` or `regions-list list-reset`, optional `<div class="<name>-footer">` holding a `button button-outline`. No `-content` wrapper.

**Layout B (single purpose):** `<section>` > one `<div class="<name>-content">` holding `<h2 id>` etc. No `section-header`.

```html
<section class="random-food-section full-bleed" aria-labelledby="random-food-title">
    <div class="random-food-content">
        <h2 id="random-food-title">Need an Idea for Your Next Meal?</h2>
        <p>Get a surprise Filipino dish, then browse more ideas.</p>
        <a href="pages/random-food.html" class="button">
            Get a surprise dish
        </a>
    </div>
</section>
```

Home order: 1 hero (B, `<h1 class="hero-title">`) · 2 `fresh-finds-section` (A) · 3 `popular-foods-section` (A) · 4 `regional-discoveries-section` (A) · 5 `trivia-section` (B) · 6 `near-me-section` (B) · 7 `random-food-section` (B). Hero has no `aria-labelledby`; all others do.

**Hero:** `section.hero.full-bleed` > `div.hero-content` > [text `<div>`: `h1.hero-title` (two lines, `<em>` on own line) / `p.hero-intro` / `form.hero-search action="#" method="get"` (input type=search name=q maxlength=100 placeholder aria-label disabled + `<button type="submit" disabled>Search</button>`) / `div.search-suggestions` (`span.search-suggestions-label` "Try a search:" + two `a.text-link`)] + [`div.hero-plate` > `figure.plate-image` > img (no `loading`, 800x800)]. No breadcrumb, input has no id/label (aria-label instead).

**Plate (a `<li>`, not article/card):**

```html
<li class="plate">
    <figure class="plate-image">
        <img
            loading="lazy"
            src="assets/images/plates/kinunot.png"
            width="800"
            height="800"
            alt="Kinunot, seen from above">
    </figure>

    <div>
        <p class="label">CAMARINES SUR, BICOL</p>

        <h3>Kinunot</h3>

        <p class="plate-description">
            A traditional Bicolano dish made with shredded
            stingray or shark meat cooked in coconut milk.
        </p>

        <a href="pages/food-details.html" class="text-link">
            Read about Kinunot
        </a>
    </div>
</li>
```

Label: capitals; regional dish = place, province/region; general dish = category (`FILIPINO CLASSIC`). Description: one sentence, 8-14 words, wrapped on two lines, starts with what the dish is. Link text `Read about {Dish}`. Image: 800x800 PNG in `assets/images/plates/`, lowercase-hyphen name.

**Region (`<li class="region">`):** `div.region-name` (`<h3>Luzon</h3>` then `<p class="label">NORTHERN PHILIPPINES</p>`, no blank line), blank, `p.region-description` (2 lines), blank, `a.text-link` "Explore {Region}" -> `pages/regions.html`. Labels: Luzon NORTHERN PHILIPPINES, Visayas CENTRAL PHILIPPINES, Mindanao SOUTHERN PHILIPPINES.

**Trivia (B):** `div.trivia-content` > `figure.plate-image` (lazy img), blank, text `<div>`: `p.label` / `h2#trivia-title` / `p.trivia-description` / `p.trivia-highlight` (blank lines between).

**Near me (B):** `div.near-me-content` > `div.near-me-info` (`p.label` SAMPLE / `h2#near-me-title` / two `p.near-me-description`, blank lines between), blank, `a.button.button-outline`. Placeholder sections say what is not available and why.

**Buttons/links:** main CTA `a.button`; secondary `a.button.button-outline`; in-content link `a.text-link`; header CTA `a.site-nav-cta`; list link = bare `<a>`. `<a>` for navigation, `<button>` only for on-page actions (menu toggle, form submit).

## 1.7 Footer (copy to every page, after `</main>`, inside container)

```html
<footer class="footer full-bleed">
    <div class="footer-main">
        <div class="footer-brand">
            <a href="index.html" class="footer-logo">
                SILANGAN
            </a>
            <p>
                Discover the Philippines, one bite at a time.
                Explore Filipino dishes, regional specialties,
                and the stories behind every flavor.
            </p>
            <span class="footer-brand-tagline">
                Sarap ng Pilipinas, tikman at tuklasin!
            </span>
        </div>

        <div class="footer-links">
            <h3>Explore</h3>
            <ul class="list-reset">
                <li><a href="pages/top-foods.html">Top Foods</a></li>
                <li><a href="pages/foods.html">All Foods</a></li>
                <li><a href="pages/recipes.html">Recipes</a></li>
                <li><a href="pages/random-food.html">Random Food</a></li>
            </ul>
        </div>

        <div class="footer-links">
            <h3>Discover</h3>
            <ul class="list-reset">
                <li><a href="pages/near-me.html">Near Me</a></li>
                <li><a href="pages/regions.html">Regions</a></li>
                <li><a href="pages/map.html">Food Map</a></li>
            </ul>
        </div>

        <div class="footer-about">
            <h3>About Silangan</h3>
            <p>
                More than just food, every Filipino dish
                tells a story of culture, tradition,
                and togetherness.
            </p>
            <a href="pages/foods.html" class="text-link">
                Start Exploring
            </a>
        </div>
    </div>

    <div class="footer-bottom">
        <p>
            © 2026 Silangan. All rights reserved.
        </p>

        <ul class="footer-legal list-reset">
            <li><a href="pages/terms.html">Terms of Service</a></li>
            <li><a href="pages/privacy.html">Privacy Policy</a></li>
        </ul>

        <span>
            Made with ♥ for Filipino food and culture.
        </span>
    </div>
</footer>
```

Only `footer-main` and `footer-bottom` wrap content. No `aria-current` in footer. Map is `Map` in header, `Food Map` in footer. A page with no `<h2>` adds `<h2 class="visually-hidden">More from Silangan</h2>` as first child of footer.

## 1.8 Elements, images, accessibility, content

- Use meaningful tags; `<div>` only as layout wrapper; repeated groups are always `<ul>`. Image = `<figure class="plate-image">` + `<img>`, no `<figcaption>`. Dropdown = `<details>/<summary>`. One `<h1>` per page. Emphasis `<em>`/`<strong>`, styling in CSS.
- Images: `width`/`height` = real file size (800x800 plates; svg = viewBox size). First image no `loading`; all later `loading="lazy"`. Class `plate-image` on `<figure>`, never `<img>`. Alt = dish + how shown (`Chicken adobo, seen from above`, `A glass of halo-halo`); repeated dish keeps its own alt. Decorative: `alt=""` + `aria-hidden="true"`.
- A11y: skip link first; nav `aria-label="Main navigation"`; menu button has aria-label/expanded/controls; decorative svg/span `aria-hidden="true"`; every non-hero section `aria-labelledby`; unlabeled fields get `aria-label`; unbuilt features `disabled`; link text meaningful alone (never "Read more"); screen-reader-only text uses `visually-hidden`.
- Content: headings Title Case; `h3` = plain name; footer headings 1-2 words Title Case; labels typed in CAPITALS (CSS never uppercases); only lowercase label-like text is `Try a search:`; descriptions one sentence ending in period; subtitles one sentence, sentence case, period; button text a short action; brand `SILANGAN` in logos, `Silangan` in running text; copyright `© 2026 Silangan. All rights reserved.`; unsourced regional claims say so in the section subtitle.

## 1.9 HTML never uses

Inline `style`, HTML comments, ids for styling, inline JS (`onclick`), layout tables, self-closing slashes on void tags or `<path>`, `<br>` on home, class on `<body>` or `<img>`, `<article class="card">` for dishes, `<figcaption>`, `text-transform` for labels, curly quotes/`&quot;`.

---

# PART 2: CSS

## 2.1 Files

- `style.css` = all shared styles, loaded everywhere. One stylesheet per page (or per related set, e.g. `auth.css`, `legal.css`), loaded AFTER `style.css`, holding only what differs; never repeat what `style.css` sets. Page-stylesheet class names start with the page/feature name.
- Order in `style.css`: (1) `@font-face` Cardo then Poppins (2) BREAKPOINTS note (3) `:root` variables (4) GLOBAL banner: `*`, `html`, `body`, `img`, `a`, form elements, headings, `p`, `:focus-visible`, `.container`, `.full-bleed`, `.list-reset`, `.visually-hidden` (5) components in page order: skip link, header/nav, buttons, labels and links (6) hero, plates (7) home sections: one shared spacing rule, then one banner per section in page order (8) footer (9) media queries. Page stylesheets: page order, media queries last.

## 2.2 Formatting

- 4 spaces, no tabs, no trailing spaces. `selector {` on one line, one declaration per line, space after colon, semicolon always.
- One blank line between rules. TWO blank lines before and after a section banner. Never three.
- Multiple selectors: each on its own line.
- Leading zero (`0.2s`), no unit on zero (`margin: 0`). Space after every comma in values. Hex colors UPPERCASE and only inside `:root`.
- Double quotes in selectors and `content` (`[aria-current="page"]`, `content: ""`). Single quotes for font names and `url()` (`'Cardo'`, `url('../assets/fonts/Cardo-Regular.woff2')`); only exception `"Segoe UI"`.

## 2.3 Comments (explain WHY, never repeat the code)

**Section banner** (name in CAPITALS, blank line above and below inside; 2 blank lines before and after the comment):

```css


/*


    SITE HEADER AND NAVIGATION BAR


*/


.site-header {
    ...
}
```

Name by what it shows; short working name in brackets (`FILIPINO DISHES TO EXPLORE (POPULAR FOODS)`); component banner may add colon + description (`PLATES: TOP-DOWN DISHES FLOATING ON THE GREEN`). The `BREAKPOINTS` banner holds its list after a blank line:

```css
    BREAKPOINTS

    This file uses three max-width breakpoints:

    1080px  the nav becomes a hamburger menu (nav.js checks 1081px)
    900px   the hero, the plates, the regions and the footer stack
    640px   small phones

    It also has two queries on pointer type, for comfortable touch targets.
```

**Note** (above the rule it explains, blank line before and after, hand-wrapped ~75 chars, text indented 4):

```css
/*

    The skip link moves focus to <main> (tabindex="-1"). It isn't
    interactive, so it doesn't need the focus ring the links and
    buttons get.

*/
```

**Usage note:** same format, shows how a class is used (`.button` filled, add `.button-outline` for outline: `class="button button-outline"`).

**Variable group note** (inside `:root`, one named line in CAPITALS ending in a colon, single-variable lines under it indented +4 more, not wrapped; `QUICK CONTROLS` is its own comment above the first group):

```css
    /*

        HEADER: The navigation bar is dark glass.
            --header-bg is the see-through tint
            --header-blur is how much the page behind it is blurred
            Keep --header-bg at 70% or more so the links stay readable.

    */
```

**Compact note** (1-2 sentences; one line, or continuation lines indented 3 spaces; blank line after; used under media query labels or inside queries):

```css
/* The site nav switches to a hamburger menu. The links and buttons are hidden
   until the menu button is pressed (nav.js adds/removes .is-open). */
```

**Media query label:** `/* ---------- Tablets landscape and smaller (up to 1080px) ---------- */`. Inside a query, short group labels: `/* Menu button */`.

## 2.4 Variables

All in `:root`, groups separated by a blank line.

| Kind | Naming |
| --- | --- |
| Fonts | `--font-*` |
| Page background | `--page-*` |
| Colors | `--color-*` |
| Quick controls | `--button-* --header-* --link-* --label-* --field-* --plate-* --footer-bg` |
| Type scale | `--text-*` (`base small h1 h2 h3`) |
| Spacing / repeated / sizes | named for the thing (`--page-gutter --section-space --section-gap --border-line --transition --container-width --site-nav-height --hero-max-height`) |
| Radius | `--radius-*` (`md`, `pill`); `50%` allowed for circles |
| Shadows / focus | `--shadow-*`, `--focus-ring` |

- Base palette is small and hex-only in `:root` (three page greens, cream text, gold primary, black for shade). Every other color is derived.
- Colors in rules ALWAYS come from variables: quick controls for buttons/header/links/labels/fields/plates/footer, else `var(--color-...)`. No hex/rgb outside `:root`.
- Soft/see-through color = `color-mix(in srgb, var(--color-primary) 35%, transparent)`, never a new hex. A color that is just another name is an alias (`--color-heading: var(--color-text);`).
- Quick controls start as base colors (`--button-bg: var(--color-primary);`), each group under a block comment.
- A repeated value becomes a variable; a one-off size is `rem` in its rule.
- A variable set inside a rule (`--gutter-x` on `.full-bleed`) is for per-element/per-breakpoint values and is the first line of the rule; a media query may reset it.
- Names describe purpose, not appearance. Every variable in `:root` is used; delete unused ones.

## 2.5 Selectors

- Classes only. No ids, no `!important`.
- Global rules use tags (`h1`, `a`, `img`). Tags inside a component are styled via the component class (`.region-name h3`, `.footer-links h3`, `.hero-title em`).
- `>` for direct children only (`.footer-brand > p`). A second class raises specificity only when one component sits in another (`.hero-plate .plate-image`).
- Rule order: base, `:hover`, `:active`, `:focus`/`:focus-visible`, pseudo-elements, modifier (with its own states under it), child selectors, media queries at file bottom.
  Example: `.button`, `.button:hover`, `.button:active`, `.button-outline`, `.button-outline:hover`.
- Attribute selectors for state the HTML carries: `[aria-current="page"]`, `[aria-expanded="true"]`, `[open]`. Forms: `:disabled`, `::placeholder`.
- Selectors sharing identical declarations share one rule, one per line. `:nth-child` rare, always with a why-note.

## 2.6 Units and values

| Unit | Use |
| --- | --- |
| `rem` | font sizes, padding, margin, gap, most max-widths |
| `px` | borders, icon sizes, 44px touch size, fixed bars, blur, breakpoints |
| `em` | letter-spacing, underline offset |
| `fr` | grid columns (`1.1fr 0.9fr`, `1fr 2fr auto`) |
| `clamp()` | fluid headings, gutters, section space |
| `min()`/`calc()` | mixing units or capping |
| `vw` | fluid sizes inside `clamp()`, full-bleed width |

- **Logical properties** for sides/axes: `margin-block-start`, `margin-inline-start`, `padding-block`, `padding-inline`, `border-block-start`, `border-block-end`. NEVER `margin-top`, `padding-left`, `border-bottom`. Shorthand when all sides are set. `top/right/bottom/left` stay for positioned elements.
- Newer unit gets an older line first (`min-height` `100vh` then `100svh`; `max-height: 100vh` then `100dvh`). Only case of a property appearing twice. Vendor prefix goes right after the standard (`backdrop-filter`, `-webkit-backdrop-filter`).
- Font weights only those that load: Cardo 400/700, Poppins 400/600/700, both italic 400. Headings 400, buttons 600.
- Touch targets at least 44px (`min-height: 44px` on buttons and on links in the hit-areas query).
- `img`/`svg`: `display: block; max-width: 100%; height: auto`. Round/fixed frames use `aspect-ratio` (`.plate-image`), the image inside is sized by `width`.

## 2.7 Transitions and motion

- Use `var(--transition)` (`0.2s ease`), list only changing properties: `transition: background-color var(--transition), border-color var(--transition), color var(--transition);`. Never `transition: all`.
- Hover changes colors only; nothing lifts/moves on hover. `.button:active` moves down `1px`.
- Anything that moves (`transform`, `scroll-behavior`) is disabled in `@media (prefers-reduced-motion: reduce)`. `style.css` has one block (smooth scroll, button press, menu arrow) placed after the button rules under its own note naming what it disables. A page stylesheet that moves things carries its own block. Color-only fades need none. A transform that only places (skip link) is not disabled.

```css
@media (prefers-reduced-motion: reduce) {

    html {
        scroll-behavior: auto;
    }

    .button:active {
        transform: none;
    }
}
```

## 2.8 Breakpoints and media queries

Desktop-first, `max-width`. Breakpoints: **1080px** nav becomes hamburger (CSS and `nav.js` use 1081px as desktop check) · **900px** two-column blocks stack, footer to two columns · **640px** small phones. No other breakpoints without a reason.

Other intentional queries: `(max-width: 900px), (any-pointer: coarse)` (44px hit areas) · `(min-width: 1081px) and (any-pointer: coarse)` (center nav links in taller hit area; goes last) · `@supports not (backdrop-filter ...)` and `prefers-reduced-transparency` (glass header becomes solid bar).

- Media queries sit at file end under a `MEDIA QUERIES` banner, EXCEPT in `style.css` two groups sit under the rules they change: the glass-header fallbacks (after header rules, one note) and the `prefers-reduced-motion` block (after button rules).
- Width queries run largest to smallest; touch `min-width` query last. Each has a label comment and a blank line after the opening brace. Inside, rules indented one more level, grouped with short comments, same property order. Repeat only changed properties; a changing variable is set on the rule's first line.
- Stacked grids use `grid-template-columns: minmax(0, 1fr)`.

```css
/* ---------- Small tablets (up to 900px) ---------- */

@media (max-width: 900px) {

    /* Hero, plates and the halo-halo fact stack into one column. */

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

## 2.9 Reusable patterns

- Full-width section: `full-bleed` in HTML; CSS sets only `--gutter-x` and `padding-block` (gutter defaults to `--page-gutter`).
- Home sections share one rule with `padding-block: var(--section-space)`; add a new home section to that list.
- `.section-header`: centered, `40rem` wide, ends with `--section-gap`.
- Two columns: `display: grid; align-items: center; gap: 3rem; grid-template-columns: 1.1fr 0.9fr;` -> `minmax(0, 1fr)` at 900px.
- `.plate-image`: thin ring, `aspect-ratio: 1; border-radius: 50%`, size `--plate-size`, fill `--plate-fill`.
- `.plates-grid`: 3 columns, 1 at 900px; middle plate sits lower, offset reset at 900px.
- `.regions-list`: top border on list, bottom border on each `.region`; columns `1fr 2fr auto`, one column at 900px.
- `.button` filled pill, `.button-outline` added alongside; colors from `--button-*`. `.label` small gold line (no `text-transform`). `.text-link` gold + thin underline, cream on hover. `.list-reset` does not reset margin: add `margin: 0` where needed.

## 2.10 CSS never uses

`!important`, id selectors, hex/rgb outside `:root`, `margin-top/bottom/left/right` (same for padding), `border-top/bottom/left/right`, `transition: all`, `text-transform`, JS for what CSS can do, a removed focus outline without replacement (only exception `main:focus { outline: none; }` with a note), unused variables, repeated shared styles in a page stylesheet.

---

# PART 3: CSS PROPERTY ORDER (inside every rule)

Outside in: **position, layout, box model, visuals, typography, effects.** ONE blank line between groups, NONE inside a group. Applies to every rule including short `:hover`/`:focus`.

**Before group 1** (each followed by a blank line): `--custom-property` set on the element, then `content` (first in `::before`/`::after`).

| # | Group | Properties (in order) |
| --- | --- | --- |
| 1 | Position | `position`, `top`, `right`, `bottom`, `left`, `inset`, `z-index` |
| 2 | Layout | `display`, `flex-direction`, `flex-wrap`, `justify-content`, `align-items`, `gap`, `grid-template-columns`, `grid-template-rows`, `flex`, `order`, `align-self`, `overflow` |
| 3 | Box model | `width`, `min-width`, `max-width`, `height`, `min-height`, `max-height`, `aspect-ratio`, `margin`, `padding`, `border`, `border-radius`, `outline`, `outline-offset`, `box-sizing` |
| 4 | Visuals | `color`, `text-shadow`, `background`/`background-color`/`background-image`/`background-size`/`background-position`/`background-repeat`, `backdrop-filter` then `-webkit-backdrop-filter`, `box-shadow`, `opacity`, `filter`, `object-fit` |
| 5 | Typography | `font-family`, `font-size`, `font-weight`, `font-style`, `line-height`, `letter-spacing`, `text-align`, `text-transform`, `white-space`, `text-wrap`, `text-decoration`, `text-decoration-thickness`, `text-underline-offset`, `-webkit-font-smoothing` |
| 6 | Effects | `pointer-events`, `cursor`, `transition`, `transform`, `animation`, `will-change` |

Unlisted properties join the closest group:
- Layout (2): `grid-auto-flow`, `grid-auto-columns`, `grid-area`, `grid-template-areas`, `grid-column`, `place-items`, `columns`, `list-style`, `isolation`, `overflow-x/y`, `overscroll-behavior-x`, `scroll-snap-type/align`, `scrollbar-width/color`, `scroll-behavior`, `scroll-padding-top`, `-webkit-overflow-scrolling`.
- Box (3): long forms (`margin-block-start`, `padding-block`, `padding-inline`, `border-block-start`, `border-color`, ...). They sort like their short form: margin, then padding, then border.
- Visuals (4): `accent-color`, `object-position`, `fill`, `stroke`, `stroke-width`, `stroke-linecap`, `stroke-linejoin`, `clip-path`.
- Typography (5): `overflow-wrap`, `-webkit-text-size-adjust`. Effects (6): `break-inside`.

Key within-group orders: `justify-content` before `align-items`; `gap` before `grid-template-columns`; `color` before `background-color`; `transition` BEFORE `transform`; width props, then height props, then `aspect-ratio`, then margin/padding/border/radius/outline.

Rules:
- All sides = short form (`margin`, `padding`, `border`). One side/axis = logical long form, never `margin-top`/`border-bottom`.
- A property is set twice only as a fallback pair, older value first. If `padding` and `padding-block` both appear, the broad one first.
- `border` and `border-color` are both group 3.

Example using every group:

```css
.badge {
    --badge-size: clamp(320px, 40vw, 660px);

    position: absolute;
    bottom: 0;
    z-index: 2;

    display: flex;
    justify-content: center;

    width: var(--badge-size);
    aspect-ratio: 1;
    padding: 1rem;
    border-radius: 50%;

    color: var(--color-text);
    background-color: var(--color-primary);

    font-size: 1rem;
    line-height: 1.6;

    transition: transform var(--transition);
}

.badge::after {
    content: "";

    width: 0.35rem;
    height: 0.35rem;
    border-right: 1.5px solid currentColor;
    border-bottom: 1.5px solid currentColor;

    transition: transform var(--transition);
    transform: rotate(45deg);
}
```

(In real code the `::after` uses logical long forms too; the `border-right`/`-bottom` above are copied from the original guide's illustration.)

---

# CHECKLISTS

**HTML:** title `Page Name | Silangan` + description · head order and both font preloads · page CSS after `style.css` · correct path prefixes · skip link first, `main#main[tabindex=-1]` after header · header/footer match this guide, correct `aria-current` · one `h1`, `h2` per section, `h3` items, no skipped levels · every section: `full-bleed`, `-section`, `aria-labelledby` = its `h2` id · layout A or B · labels in capitals · every `img` has width/height/alt, `lazy` below first screen · 4+ attributes split, head exempt · text over ~60 chars wrapped on own lines · blank lines per tables · 4-space indent.

**CSS:** sections in page order with banners · colors from variables only, `color-mix()` for soft versions · `var(--radius-*)` · property order per Part 3 · logical properties · `var(--transition)` naming properties · states directly under base rule · media queries at bottom (largest first, labeled) · anything that moves has a reduced-motion rule · every `:root` variable used · no `!important`, no ids, nothing already in `style.css`.
