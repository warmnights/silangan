# HTML Writing Guide

> **Purpose:** Describes how HTML is written on Silangan: page structure, formatting, class names, section patterns and accessibility. Every rule comes from `index.html`. Copy the snippets in this guide and change only the content.
> **Read when:** Creating a page, adding a section, or editing markup.
> **Related:** [overview.md](overview.md) · [css-writing-guide.md](css-writing-guide.md) · [css-property-guide.md](css-property-guide.md) · [global.md](global.md)

---

## 1. Page skeleton

Every page has the same `<head>`. Only the title, the description, the path prefix and (on pages that need one) the page stylesheet change.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Filipino Food &amp; Culture | Silangan</title>
    <meta name="description" content="Discover the Philippines one bite at a time. Explore Filipino dishes, regional specialties, recipes, and the stories behind every flavor.">

    <link rel="icon" href="assets/images/logo/favicon.svg" type="image/svg+xml">
    <link rel="preload" href="assets/fonts/Cardo-Regular.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="preload" href="assets/fonts/Poppins-Regular.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="stylesheet" href="css/style.css">
    <script src="js/nav.js" defer></script>
</head>
<body>
```

| Rule | Detail |
| --- | --- |
| Language | `<html lang="en">` on every page |
| Order | charset, viewport, title, description, favicon, font preload (Cardo), font preload (Poppins), `style.css`, a page stylesheet if the page has one, `nav.js` |
| Grouping | Three blocks separated by blank lines: the two meta tags, then title and description, then links and script |
| Indentation | `<head>` and `<body>` start at column 0. Their children are indented 4 spaces. |
| One line | Every tag in the `<head>` stays on one line, however long. The attribute-splitting rule in section 3 does not apply to `<meta>`, `<link>` or `<script>`. |
| Title | `Page Name | Silangan`. The home page is `Filipino Food &amp; Culture | Silangan`. |
| Ampersand | Written as `&amp;` in the title. In body text, avoid `&` or write `&amp;`. |
| Description | One or two full sentences that end with a period. It says what the page really offers. |
| Script | `nav.js` only, in the head, with `defer` |
| Page stylesheet | Added right after `style.css`, only when the page needs its own CSS. The home page has none. |
| Void tags | No closing slash: `<meta>`, `<link>`, `<img>`, `<input>`, `<br>` |
| Attribute order in the head | `<meta name content>`, `<link rel href type>`, `<link rel href as type crossorigin>`, `<script src defer>` |
| Body | `<body>` has no class and no attributes |

---

## 2. Page structure

All page content sits inside one `<div class="container">`. It is the only child of `<body>`.

```html
<body>
    <div class="container">
        <a href="#main" class="skip-link">Skip to main content</a>

        <header class="site-header">...</header>

        <main id="main" tabindex="-1">
            <section class="hero full-bleed">...</section>
            <section class="fresh-finds-section full-bleed" aria-labelledby="fresh-finds-title">...</section>
        </main>

        <footer class="footer full-bleed">...</footer>
    </div>
</body>
</html>
```

- The skip link is the first element inside the container, with no blank line above it. A blank line follows it.
- `<main id="main" tabindex="-1">` is the skip link's target. Attribute order is `id`, then `tabindex`.
- A blank line separates the skip link, `<header>`, `<main>` and `<footer>`.
- The header is `<header class="site-header">` holding one `<nav class="site-nav full-bleed">`. It is copied from an existing page (section 5). Only `aria-current="page"` and the path prefix change.
- Every section carries `full-bleed`. The section is the full-width background, and its content sits inside an inner wrapper or directly inside the section (section 6).
- The first section in `<main>` is the hero. It holds the page's only `<h1>`.
- Section class names end in `-section`, except the hero, which is `hero`.
- A blank line separates one section from the next. There is no blank line after `<main>` opens or before it closes.
- Headings do not skip levels: `<h1>` in the hero, `<h2>` for each section, `<h3>` for items inside a section and for footer headings.
- The footer is the last element inside the container, outside `<main>`.
- The file ends with `</body>` and `</html>`, with no code after.

### Paths

| File location | Link to a page | Asset path | Link home | CSS and JS |
| --- | --- | --- | --- | --- |
| `index.html` | `pages/foods.html` | `assets/images/...` | `index.html` | `css/style.css`, `js/nav.js` |
| `pages/*.html` | `foods.html` | `../assets/images/...` | `../index.html` | `../css/style.css`, `../js/nav.js` |

Every link, image, font, stylesheet and script on a page uses the same prefix. When copying the header, footer or a section from `index.html` into a page inside `pages/`, add `../` to home, assets, CSS and JS paths and remove `pages/` from page links.

---

## 3. Formatting

### Indentation

- **4 spaces** per level, no tabs, no trailing spaces.
- One element per line, with children indented one level.
- Attributes of a split tag are indented one level deeper than the tag name (see Attributes below).

### Blank lines

Blank lines separate blocks that are different in purpose. Tightly paired elements stay together with no blank line.

| Blank line between | Example |
| --- | --- |
| Sections in `<main>` | hero / Bicol Specialties / Dishes to Explore |
| The top-level parts of a component | `site-nav`: logo / toggle button / link list / actions |
| An image block and its text block | `<figure>` / `<div>` in a plate, in the hero and in the trivia section |
| Each piece of a plate or text block | label / heading / description / link |
| Sibling `<li class="plate">` and `<li class="region">` items | one blank line between items, none after `<ul>` opens |
| Hero parts | `h1` / `p` / `form` / suggestions |
| Paragraphs in `near-me-info` | two `near-me-description` paragraphs |
| Footer `footer-main` columns | brand / Explore / Discover / About |
| Footer `footer-bottom` parts | copyright / legal list / made-with line |
| A hero or content area's two halves | text `<div>` / `hero-plate` `<div>` |

| No blank line between | Example |
| --- | --- |
| `<section>` opening and its first child | `<section ...>` then `<header class="section-header">` |
| Section header parts | `<h2>` and its `<p>` |
| The name and label of a region | `<h3>` and `<p class="label">` inside `region-name` |
| Form controls | input and button inside `hero-search` |
| Search suggestion row | label span and the two links |
| Brand parts | `footer-logo`, paragraph and tagline inside `footer-brand` |
| Link column parts | `<h3>` and `<ul>` inside `footer-links`; `<h3>`, `<p>`, `<a>` inside `footer-about` |
| CTA block parts | `<h2>`, `<p>`, `<a>` inside `random-food-content` |
| The two SVG icons in the menu button | `icon-menu` and `icon-close` |
| List items in a plain link list | `<li><a>` rows in the nav, dropdown and footer lists |

### Text content

Short text stays on one line with its tag:

```html
<li><a href="pages/top-foods.html">Top Foods</a></li>
<h3>Kinunot</h3>
<p>Get to know three dishes and the flavors behind them.</p>
<p class="trivia-highlight">A little mix of everything!</p>
<button type="submit" disabled>Search</button>
```

A tag's text goes on its own lines inside the tag, wrapped by hand at about 60 characters of text, when it is longer than one line:

```html
<p class="plate-description">
    A traditional Bicolano dish made with shredded
    stingray or shark meat cooked in coconut milk.
</p>
```

These always put their text on its own lines, even when it is short:

| Element | Example |
| --- | --- |
| `a.text-link` in a plate, region or the footer About column | `Read about Kinunot` |
| `a.button` (including `button-outline`) | `Get a surprise dish` |
| `a.site-nav-cta` | `Log In` |
| `a.footer-logo` | `SILANGAN` |
| `span.footer-brand-tagline` | the Filipino tagline |
| `<p>` and `<span>` in `footer-bottom` | copyright, made-with line |
| `<summary>` | `More`, followed by the chevron SVG |
| Heading `<h1>` with an `<em>` | `<em>` goes on its own line |

These stay on one line: `a.site-nav-logo` (`SILANGAN`), the two `text-link` anchors in `search-suggestions`, and every `<li><a>` in a list.

Text rules:

- Apostrophes and quotation marks are plain straight characters: `country's`, `doesn't`, `"Halo-Halo"`.
- `©` and `♥` are typed directly, not as entities.
- `&amp;` is used in the title.
- `<br>` is not used on the home page.

### Attributes

| Attributes on the tag | Layout |
| --- | --- |
| 1 or 2 | One line |
| 3 | One line when short, otherwise split |
| 4 or more | Always split |

"Short" means the whole tag, with its indentation, fits in about 105 characters. A tag with 1 or 2 attributes that would be longer than that is split too. Head tags (section 1) are exempt.

A split tag keeps its name on the first line. Each attribute goes on its own line, one level deeper, and the closing `>` ends the last attribute:

```html
<img
    loading="lazy"
    src="assets/images/plates/kinunot.png"
    width="800"
    height="800"
    alt="Kinunot, seen from above">
```

A long section tag splits the same way:

```html
<section
    class="regional-discoveries-section full-bleed"
    aria-labelledby="regional-discoveries-title">
```

The section above splits because its single line is 113 characters. `<section class="popular-foods-section full-bleed" aria-labelledby="popular-foods-title">` is 100 characters and stays on one line.

### Attribute order

The order is: identity (`class`, `id`, `href`, `src`, `type`, `name`), then size and content (`width`, `height`, `maxlength`, `alt`, `placeholder`), then `aria-*`, then boolean attributes such as `disabled`.

| Element | Order | Example |
| --- | --- | --- |
| `<a>` | `href`, `class`, `aria-*` | `<a href="pages/foods.html" class="text-link">` |
| `<a>` current page | `href`, `aria-current` | `<a href="index.html" aria-current="page">` |
| `<img>` | `loading`, `src`, `width`, `height`, `alt`. The first image has no `loading`. | |
| `<section>` | `class`, `aria-labelledby` | |
| `<h2>` | `id` only | `<h2 id="trivia-title">` |
| `<nav>` | `class`, `aria-label` | |
| `<main>` | `id`, `tabindex` | |
| `<ul>` with an id | `class`, `id` | `<ul class="site-nav-links list-reset" id="site-nav-menu">` |
| `<button>` (menu) | `class`, `type`, `aria-label`, `aria-expanded`, `aria-controls` | |
| `<button>` (form) | `type`, `disabled` | `<button type="submit" disabled>` |
| `<input>` | `type`, `name`, `maxlength`, `placeholder`, `aria-label`, `disabled` | |
| `<form>` | `class`, `action`, `method` | |
| `<svg>` | `class`, `viewBox`, `aria-hidden` | |
| `<span>` decorative | `class`, `aria-hidden` | |

---

## 4. Class names

Class names are lowercase words joined with hyphens. They are flat: there is no BEM, so no `__` or `--`.

| Pattern | Example |
| --- | --- |
| A component name is the prefix, and each part adds a suffix | `plate`, `plate-image`, `plate-description`; `region`, `region-name`, `region-description` |
| A list of components is the plural name plus the kind | `plates-grid`, `regions-list` |
| Page and feature classes start with the feature name | `site-nav-`, `hero-`, `trivia-`, `near-me-`, `random-food-`, `footer-` |
| A section's class is the feature name plus `-section` | `fresh-finds-section`, `popular-foods-section` |
| A section's inner wrapper is the feature name plus `-content` | `hero-content`, `trivia-content`, `near-me-content`, `random-food-content` |
| A variation adds a suffix to its base class, and both are written | `button button-outline` |
| Utility classes go last | `hero full-bleed`, `site-nav full-bleed`, `site-nav-links list-reset` |
| State classes start with `is-` and are added by JavaScript only | `is-open` |

Names describe what a thing is, not how it looks. Before inventing a class, check the shared ones in [global.md](global.md): `.button`, `.button-outline`, `.full-bleed`, `.list-reset`, `.label`, `.text-link`, `.section-header`, `.plate`, `.plate-image`, `.visually-hidden`.

### Ids

Ids are never used for styling. They exist for three jobs:

| Job | Example |
| --- | --- |
| Skip link target | `id="main"` |
| `aria-controls` target | `id="site-nav-menu"` |
| `aria-labelledby` target | `id="trivia-title"` on the section's `<h2>` |

The heading id is the section name without `-section`, plus `-title`:

| Section class | Heading id |
| --- | --- |
| `fresh-finds-section` | `fresh-finds-title` |
| `popular-foods-section` | `popular-foods-title` |
| `regional-discoveries-section` | `regional-discoveries-title` |
| `trivia-section` | `trivia-title` |
| `near-me-section` | `near-me-title` |
| `random-food-section` | `random-food-title` |

### Plain `<div>` text blocks

When a block has an image on one side and text on the other, the text sits in a plain `<div>` with no class. This happens in `.plate`, in `.hero-content` and in `.trivia-content`. The parent's CSS positions it. Do not add a class unless the CSS needs one.

---

## 5. Header and navigation

Copy this block into every page. On `index.html` the paths look like this:

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

| Rule | Detail |
| --- | --- |
| Order inside the nav | logo, menu button, link list, actions |
| Link order | Home, Top Foods, All Foods, Regions, Near Me, then the More dropdown |
| More dropdown | Recipes, Map, Random Food, inside `<details>` and `<summary>`, no JavaScript |
| Current page | `aria-current="page"` goes on that page's link only, after `href`. Remove it from Home on other pages. |
| Logo text | `SILANGAN`, typed in capitals |
| Menu button | Starts as `aria-label="Open menu"` and `aria-expanded="false"`. `nav.js` changes them. |
| Icons | Both SVGs use `viewBox="0 0 24 24"` and `aria-hidden="true"`. The paths are listed above and are not changed. |
| Paths | `<path d="..."></path>` is written with a closing tag, never self-closed |
| Divider | An empty `<span class="site-nav-divider" aria-hidden="true"></span>` |

---

## 6. Sections

`index.html` uses two section layouts. Pick the one that matches the content.

### Layout A: header and list

Used by Bicol Specialties, Dishes to Explore and From Around the Philippines. The `<section>` holds a `<header class="section-header">` and then a list. There is no `-content` wrapper.

```html
<section class="popular-foods-section full-bleed" aria-labelledby="popular-foods-title">
    <header class="section-header">
        <h2 id="popular-foods-title">Filipino Dishes to Explore</h2>
        <p>Get to know three dishes and the flavors behind them.</p>
    </header>

    <ul class="plates-grid list-reset">
        ...plates...
    </ul>

    <div class="popular-foods-footer">
        <a href="pages/foods.html" class="button button-outline">
            Explore All Filipino Foods
        </a>
    </div>
</section>
```

- `<header class="section-header">` is a `<header>` element with `<h2 id="...">` and one `<p>`.
- A section can have a closing `<div class="<name>-footer">` for a button below the list (`popular-foods-footer`).
- A section's `<ul>` is `plates-grid list-reset` or `regions-list list-reset`.
- A section's note paragraph, such as the Bicol disclaimer, goes in the section header's `<p>`.

### Layout B: single-purpose section

Used by the hero, trivia, near me and random food sections. The `<section>` holds one wrapper named `<name>-content`.

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

In this layout the `<h2>` sits inside the content wrapper. There is no `section-header`.

### Section order on the home page

| # | Section | Class | Layout | Heading |
| --- | --- | --- | --- | --- |
| 1 | Hero | `hero` | B | `<h1 class="hero-title">` |
| 2 | Bicol specialties | `fresh-finds-section` | A | `Bicol Specialties` |
| 3 | Popular dishes | `popular-foods-section` | A | `Filipino Dishes to Explore` |
| 4 | Regions | `regional-discoveries-section` | A | `From Around the Philippines` |
| 5 | Food fact | `trivia-section` | B | `The Meaning Behind Halo-Halo` |
| 6 | Near me sample | `near-me-section` | B | `Albay Sample Area` |
| 7 | Random dish prompt | `random-food-section` | B | `Need an Idea for Your Next Meal?` |

The hero has no `aria-labelledby`, because its `<h1>` is already in the section. Every other section has `aria-labelledby` pointing to its `<h2>`.

### Hero

```html
<section class="hero full-bleed">
    <div class="hero-content">
        <div>
            <h1 class="hero-title">
                Discover the Philippines,
                <em>One Bite at a Time.</em>
            </h1>

            <p class="hero-intro">
                Explore Filipino dishes, regional specialties,
                and the stories behind every flavor.
            </p>

            <form class="hero-search" action="#" method="get">
                <input
                    type="search"
                    name="q"
                    maxlength="100"
                    placeholder="Adobo or garlic"
                    aria-label="Search Filipino dishes or ingredients"
                    disabled>
                <button type="submit" disabled>Search</button>
            </form>

            <div class="search-suggestions">
                <span class="search-suggestions-label">Try a search:</span>
                <a href="pages/food-details.html" class="text-link">Chicken Adobo</a>
                <a href="pages/food-details.html" class="text-link">Sinigang</a>
            </div>
        </div>

        <div class="hero-plate">
            <figure class="plate-image">
                <img
                    src="assets/images/plates/chicken-adobo.png"
                    width="800"
                    height="800"
                    alt="Chicken adobo, seen from above">
            </figure>
        </div>
    </div>
</section>
```

- The hero has no breadcrumb.
- The `<h1>` is split over two lines, and the emphasized part is in an `<em>` on its own line.
- The search form has `action="#"` and `method="get"`. The input has no `id` and no `<label>`, and it uses an `aria-label` instead.
- The search input and button are `disabled` while search is not built.
- The hero image is the first image on the page, so it has no `loading="lazy"`.
- The text `<div>` and `hero-plate` are the two halves of `hero-content`.

### Plate (dish card)

A plate is a list item with a floating image and a text block. It is not an `<article>` or a card.

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

| Rule | Detail |
| --- | --- |
| Parent | `<ul class="plates-grid list-reset">` |
| Order | image figure, then the text `<div>` |
| Text order | label, `<h3>` dish name, description, link |
| Label | One line in capitals. For a regional dish it is the place, then the province or region (`CAMALIG, ALBAY`, `NAGA CITY, CAMARINES SUR`). For a general dish it is a category (`FILIPINO CLASSIC`, `COMFORT FOOD`, `FESTIVE FAVORITE`). |
| Description | One sentence of about 8 to 14 words, wrapped over two lines |
| Link text | `Read about {Dish Name}`. The dish name makes each link unique for screen readers. |
| Link target | `pages/food-details.html` |
| Image | 800 by 800 PNG in `assets/images/plates/`, named for the dish in lowercase with hyphens (`chicken-adobo.png`) |

### Regions list

```html
<ul class="regions-list list-reset">
    <li class="region">
        <div class="region-name">
            <h3>Luzon</h3>
            <p class="label">NORTHERN PHILIPPINES</p>
        </div>

        <p class="region-description">
            Discover rich culinary traditions, hearty specialties, and
            regional favorites from the country's largest island group.
        </p>

        <a href="pages/regions.html" class="text-link">
            Explore Luzon
        </a>
    </li>
</ul>
```

- Here the label comes after the `<h3>`, inside `region-name`.
- The three regions are Luzon (`NORTHERN PHILIPPINES`), Visayas (`CENTRAL PHILIPPINES`) and Mindanao (`SOUTHERN PHILIPPINES`).
- Link text is `Explore {Region}` and the link goes to `pages/regions.html`.

### Image and text section (trivia)

```html
<section class="trivia-section full-bleed" aria-labelledby="trivia-title">
    <div class="trivia-content">
        <figure class="plate-image">
            <img
                loading="lazy"
                src="assets/images/plates/halo-halo.png"
                width="800"
                height="800"
                alt="A glass of halo-halo">
        </figure>

        <div>
            <p class="label">FILIPINO FOOD FACT</p>

            <h2 id="trivia-title">The Meaning Behind Halo-Halo</h2>

            <p class="trivia-description">
                The name "Halo-Halo" literally means "mix-mix" in Filipino.
                It comes from the way its ingredients are combined before
                eating, bringing together different textures and flavors
                in one refreshing dessert.
            </p>

            <p class="trivia-highlight">A little mix of everything!</p>
        </div>
    </div>
</section>
```

The image block comes first, then the text `<div>`. The text order is label, `<h2>`, description, highlight line.

### Information section (near me)

```html
<section class="near-me-section full-bleed" aria-labelledby="near-me-title">
    <div class="near-me-content">
        <div class="near-me-info">
            <p class="label">SAMPLE</p>

            <h2 id="near-me-title">Albay Sample Area</h2>

            <p class="near-me-description">
                Near Me doesn't use your location yet. Albay is shown as a
                sample area, and no restaurants are listed or recommended.
            </p>

            <p class="near-me-description">
                No listings available. Restaurant names and details are left
                out until they can be backed by reliable sources.
            </p>
        </div>

        <a href="pages/near-me.html" class="button button-outline">
            See the Albay Sample Area
        </a>
    </div>
</section>
```

A section can be a placeholder. Its label says `SAMPLE`, and its text says what is not available and why.

### Call to action section

See the random food example in Layout B. It has an `<h2>`, one short unclassed `<p>`, and one `a.button` (solid, not `button-outline`).

### Buttons and links

| Need | Markup |
| --- | --- |
| Main call to action | `<a href="..." class="button">` |
| Secondary call to action | `<a href="..." class="button button-outline">` |
| Link inside content | `<a href="..." class="text-link">` |
| Header call to action | `<a href="..." class="site-nav-cta">` |
| Link in a list of links | `<a href="...">` with no class inside `<li>` |

A link that goes somewhere is `<a>`. A `<button>` is only for actions on the page (the menu toggle and form submit).

---

## 7. Footer

Copy this block into every page. It sits after `</main>`, still inside the container.

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

| Rule | Detail |
| --- | --- |
| Columns | Brand, Explore, Discover, About Silangan |
| Column headings | `<h3>`. A page with an `<h2>` needs nothing else. If a page has no `<h2>`, add `<h2 class="visually-hidden">More from Silangan</h2>` as the first child of the footer. |
| Link labels | The map link is `Map` in the header and `Food Map` in the footer |
| Footer links | No `aria-current` |
| Bottom row | Copyright, legal links, made-with line |
| Wrapper classes | `footer-main` and `footer-bottom` are the only two wrappers inside `<footer>` |

---

## 8. Elements

| Need | Element |
| --- | --- |
| A section of the page | `<section>` with `aria-labelledby` pointing to its `<h2>` |
| A heading block for a section | `<header class="section-header">` |
| An image | `<figure class="plate-image">` containing `<img>`, no `<figcaption>` |
| A list of dishes or regions | `<ul class="... list-reset">` with `<li>` children |
| Navigation | `<nav>` with an `aria-label` |
| A dropdown without JS | `<details>` and `<summary>` |
| A search field | `<form>` with `<input type="search">` and a submit `<button>` |
| A small label above a heading | `<p class="label">` |
| An icon | inline `<svg>` with `aria-hidden="true"` |
| Layout wrappers | `<div>` |
| One decorative or inline note | `<span>` |

- `<div>` is for layout wrappers. If a meaningful tag exists, use it.
- One `<h1>` per page, in the hero.
- Emphasis uses `<em>` (the hero headline) and `<strong>`. Visual styling belongs in CSS.
- A repeated group (plates, regions, nav links, footer links) is always a `<ul>`.

---

## 9. Images

- Every `<img>` has `width` and `height` set to the file's real pixel size (800 by 800 for plate images, the `viewBox` size for an SVG). These only reserve space so the page does not jump. CSS controls the displayed size.
- The first image on a page has no `loading` attribute. Every image below the first screen has `loading="lazy"`.
- All dish images sit in a `<figure class="plate-image">`. The class goes on the `<figure>`, never on the `<img>`.
- Alt text names the dish and how it is shown: `Chicken adobo, seen from above`. Add a short descriptor when the name alone is unclear: `Kinalas noodle soup, seen from above`. A dish that is not a plate says what it is: `A glass of halo-halo`.
- The same dish can appear more than once on a page (the hero and the plates grid both show chicken adobo), and each image keeps its own alt.
- An image that only decorates uses `alt=""` and `aria-hidden="true"`.

---

## 10. Accessibility

- A skip link is first in the container, pointing to `#main`.
- The current page is marked `aria-current="page"` on its navigation link.
- The navigation has `aria-label="Main navigation"`.
- The menu button has `aria-label`, `aria-expanded` and `aria-controls` pointing to the id of the link list.
- Decorative SVG icons and decorative spans have `aria-hidden="true"`.
- Every section except the hero has `aria-labelledby` pointing to its `<h2>`.
- A form field with no visible label has an `aria-label` that says what it searches or asks for.
- Interactive features that are not built yet are `disabled`.
- Link text makes sense alone: `Read about Kinunot`, not `Read more`.
- Text that only screen readers need uses `class="visually-hidden"`.

---

## 11. Content

- Headings are in Title Case: `Filipino Dishes to Explore`, `From Around the Philippines`, `Need an Idea for Your Next Meal?`.
- Dish and region names in `<h3>` are the plain name: `Kinunot`, `Luzon`.
- Footer column headings are one or two words in Title Case: `Explore`, `Discover`, `About Silangan`.
- Labels are typed in capitals in the HTML (`CAMARINES SUR, BICOL`, `FILIPINO FOOD FACT`). CSS adds only letter spacing, not `text-transform`.
- The only lowercase label-like text is the search row prefix, `Try a search:`, in sentence case with a colon.
- Descriptions are one sentence that ends with a period, and they start with what the dish is (`A traditional Bicolano dish...`, `Tender chicken simmered...`).
- Section subtitles are one sentence in sentence case that ends with a period.
- Button text is a short action: `Explore All Filipino Foods`, `Get a surprise dish`, `See the Albay Sample Area`.
- A placeholder feature is described as a placeholder. The Near Me section says it does not use the location, that Albay is a sample, and that no restaurants are listed.
- Regional claims that are not sourced say so in the section's subtitle.
- The brand name in the logo and the footer logo is `SILANGAN` in capitals. In running text it is `Silangan`.
- The footer tagline is in Filipino: `Sarap ng Pilipinas, tikman at tuklasin!`
- The copyright line is `© 2026 Silangan. All rights reserved.`

---

## 12. Class reference

Every class used on `index.html`, where it goes, and which element carries it.

| Class | Element | Where |
| --- | --- | --- |
| `container` | `div` | wraps all page content |
| `skip-link` | `a` | first element |
| `site-header` | `header` | holds the nav |
| `site-nav` | `nav` | main navigation |
| `site-nav-logo` | `a` | logo link |
| `site-nav-toggle` | `button` | mobile menu button |
| `icon-menu`, `icon-close` | `svg` | menu button icons |
| `site-nav-links` | `ul` | menu links |
| `site-nav-dropdown` | `details` | More dropdown |
| `site-nav-chevron` | `svg` | arrow in `<summary>` |
| `site-nav-dropdown-menu` | `ul` | dropdown links |
| `site-nav-actions` | `div` | divider and Log In |
| `site-nav-divider` | `span` | empty decorative divider |
| `site-nav-cta` | `a` | Log In link |
| `hero` | `section` | first section |
| `hero-content` | `div` | hero wrapper |
| `hero-title` | `h1` | headline |
| `hero-intro` | `p` | text under the headline |
| `hero-search` | `form` | search form |
| `search-suggestions` | `div` | row of suggested searches |
| `search-suggestions-label` | `span` | `Try a search:` |
| `hero-plate` | `div` | hero image half |
| `plate-image` | `figure` | every dish image |
| `section-header` | `header` | title block of a section |
| `plates-grid` | `ul` | list of plates |
| `plate` | `li` | one dish |
| `label` | `p` | small capital label |
| `plate-description` | `p` | dish description |
| `text-link` | `a` | link inside content |
| `button`, `button-outline` | `a` | calls to action |
| `popular-foods-footer` | `div` | button row under the plates |
| `regions-list` | `ul` | list of regions |
| `region` | `li` | one region |
| `region-name` | `div` | region heading and label |
| `region-description` | `p` | region description |
| `trivia-content`, `trivia-description`, `trivia-highlight` | `div`, `p`, `p` | food fact section |
| `near-me-content`, `near-me-info`, `near-me-description` | `div`, `div`, `p` | near me section |
| `random-food-content` | `div` | call to action wrapper |
| `footer` | `footer` | page footer |
| `footer-main`, `footer-bottom` | `div` | footer rows |
| `footer-brand`, `footer-logo`, `footer-brand-tagline` | `div`, `a`, `span` | brand column |
| `footer-links` | `div` | Explore and Discover columns |
| `footer-about` | `div` | About column |
| `footer-legal` | `ul` | Terms and Privacy links |
| `list-reset` | `ul` | removes bullets and spacing |
| `full-bleed` | `nav`, `section`, `footer` | full-width background |

---

## 13. Not used

- Inline `style="..."` attributes
- HTML comments in page files
- Ids for styling
- Inline JavaScript such as `onclick`
- Tables for layout
- Self-closing slashes on void tags or on `<path>`
- `<br>` on the home page
- A class on `<body>`
- A class on `<img>`
- `<article class="card">` for dishes (plates are `<li class="plate">`)
- `<figcaption>`
- `text-transform` for labels (type them in capitals)
- Curly quotes or `&quot;` in text

---

## 14. Page checklist

- [ ] Title is `Page Name | Silangan` and a meta description is written
- [ ] The head has the same order and the same two font preloads
- [ ] A page stylesheet, if used, is linked after `style.css`
- [ ] Path prefixes are correct throughout (`../` on pages in `pages/`)
- [ ] The skip link is first, and `<main id="main" tabindex="-1">` follows the header
- [ ] The header and footer match the copies in this guide
- [ ] `aria-current="page"` is on the correct navigation link
- [ ] One `<h1>` in the hero, `<h2>` per section, `<h3>` for items, with no skipped levels
- [ ] Every section has `full-bleed`, a class ending in `-section`, and `aria-labelledby` matching its `<h2>` id
- [ ] Section layout is A (`section-header` and list) or B (`-content` wrapper)
- [ ] Plates and regions use the structure in section 6
- [ ] Labels are typed in capitals
- [ ] Every `<img>` has `width`, `height` and `alt`, and `loading="lazy"` below the first screen
- [ ] Tags with four or more attributes are split one per line (head tags excepted)
- [ ] Text longer than about 60 characters is wrapped on its own lines
- [ ] Blank lines follow the tables in section 3
- [ ] Indentation is 4 spaces
