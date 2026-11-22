# HTML Writing Guide

> **Purpose:** Describes how HTML is written on Silangan: page structure, formatting, class names and accessibility. Every rule comes from the pages already in the project.
> **Read when:** Creating a page, adding a section, or editing markup.
> **Related:** [overview.md](overview.md) · [css-writing-guide.md](css-writing-guide.md) · [css-property-guide.md](css-property-guide.md) · [global.md](global.md)

---

## 1. Page skeleton

Every page has the same `<head>`. Only the title, the description, the page stylesheet and the path prefix change.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Near Me | Silangan</title>
    <meta name="description" content="Find Filipino food in your area.">

    <link rel="icon" href="../assets/images/logo/favicon.svg" type="image/svg+xml">
    <link rel="preload" href="../assets/fonts/Poppins-Regular.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="preload" href="../assets/fonts/Fraunces_72pt-Bold.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="stylesheet" href="../css/style.css">
    <link rel="stylesheet" href="../css/near-me.css">
    <script src="../js/nav.js" defer></script>
</head>
```

| Rule | Detail |
| --- | --- |
| Language | `<html lang="en">` on every page |
| Order | charset, viewport, title, description, favicon, two font preloads, `style.css`, the page stylesheet, `nav.js` |
| Grouping | Three blocks separated by blank lines: meta tags, title and description, then links and script |
| Title | `Page Name | Silangan`. The home page is `Filipino Food & Culture | Silangan`. |
| Script | `nav.js` only, in the head, with `defer` |
| Void tags | No closing slash: `<meta>`, `<link>`, `<img>`, `<input>`, `<br>` |

---

## 2. Page structure

All page content sits inside one `<div class="container">`.

```html
<body>
    <div class="container">
        <a href="#main" class="skip-link">Skip to main content</a>

        <header class="site-header">...</header>

        <main id="main" tabindex="-1">
            <section class="page-hero page-hero-centered full-bleed">...</section>
            <section class="near-spots-section full-bleed" id="near-spots">...</section>
        </main>

        <footer class="footer full-bleed">...</footer>
    </div>
</body>
```

- The skip link is the first element, and `<main id="main" tabindex="-1">` is its target. `id="main"` is on every page.
- The header is `<header class="site-header">` holding `<nav class="site-nav full-bleed">`. It is copied from an existing page, and only `aria-current="page"` and the path prefix change.
- A section that needs a full-width background carries `full-bleed`. Its content goes in an inner wrapper (`near-spots-container`, `near-explore-content`) that sets the reading width.
- The first section is a hero (`page-hero`, or `hero` on the home page). It holds the breadcrumb and the page's only `<h1>`.
- Sections below the hero have class names ending in `-section`.
- Headings do not skip levels. If a page has no `<h2>` but the footer uses `<h3>`, add `<h2 class="visually-hidden">`.

### Paths

| File location | Link to a page | Asset path | Link home |
| --- | --- | --- | --- |
| `index.html` | `pages/foods.html` | `assets/images/...` | |
| `pages/*.html` | `foods.html` | `../assets/images/...` | `../index.html` |
| `404.html` | `/pages/foods.html` | `/assets/...` | `/index.html` |

`404.html` uses root paths because it can be served from any URL. This works only when the site is served from a domain root, as on Vercel.

---

## 3. Formatting

### Indentation and spacing

- **4 spaces** per level, no tabs, no trailing spaces.
- One element per line, with children indented one level.
- A blank line separates sibling blocks (a heading and its paragraph, the parts of a card, sections).
- Related lines stay together, such as `<li>` items in a list.

### Text content

Short text stays on one line with its tag:

```html
<li><a href="top-foods.html">Top Foods</a></li>
<h3>Kinunot</h3>
```

Longer text goes on its own lines inside the tag, wrapped by hand at about 60 characters:

```html
<p class="fresh-find-description">
    A traditional Bicolano dish made with shredded
    stingray or shark meat cooked in coconut milk.
</p>
```

Links styled as buttons or calls to action also put their text on its own line.

### Attributes

| Attributes on the tag | Layout |
| --- | --- |
| 1 or 2 | One line |
| 3 | One line when short, otherwise split |
| 4 or more | Always split |

A split tag keeps its name on the first line. Each attribute goes on its own line, one level deeper, and the closing `>` ends the last attribute:

```html
<img
    loading="lazy"
    src="assets/images/foods/kinunot.jpg"
    width="1300"
    height="731"
    alt="Traditional Bicolano Kinunot">
```

### Attribute order

The order is: identity (`class`, `id`, `href`, `src`, `type`), then size and content (`width`, `height`, `alt`, `placeholder`), then `aria-*`, then boolean attributes such as `disabled`.

| Element | Usual order |
| --- | --- |
| `<a>` | `href`, `class`, `aria-*` |
| `<img>` | `src`, `width`, `height`, `alt`. `loading` and `class` go before `src`. |
| `<section>` | `class`, `id`, `aria-labelledby` |
| `<button>` | `class`, `type`, `aria-label`, `aria-expanded`. Form buttons use `type`, `class`, `disabled`. |
| `<input>` | `type`, `id`, `placeholder`, `disabled` |

---

## 4. Class names

Class names are lowercase words joined with hyphens. They are flat: there is no BEM, so no `__` or `--`.

| Pattern | Example |
| --- | --- |
| A component name is the prefix, and each part adds a suffix | `fresh-finds-grid`, `fresh-find-card`, `fresh-find-info`, `fresh-find-category` |
| Page-specific classes start with the page or feature name | `near-spots-`, `near-explore-`, `trivia-` |
| A variation adds a suffix to its base class, and both are written | `button button-outline`, `card card-on-secondary`, `page-hero page-hero-centered` |
| Utility classes go last | `page-hero page-hero-centered full-bleed`, `site-nav-links list-reset` |
| State classes start with `is-` and are added by JavaScript only | `is-open` |
| Sections below the hero end in `-section` | `near-me-section` |

Names describe what a thing is, not how it looks. `restaurant-preview-notice` is a good name, `gold-box` is not. Before inventing a class, check the shared ones in [global.md](global.md): `.button`, `.card`, `.full-bleed`, `.list-reset`, `.visually-hidden`.

---

## 5. Elements

| Need | Element |
| --- | --- |
| A card in a grid | `<article class="card ...">` |
| A group of content with a heading | `<section>` |
| A side note | `<aside>` |
| Navigation | `<nav>` with an `aria-label` |
| A breadcrumb | `<nav class="breadcrumb">` containing an `<ol class="breadcrumb-list list-reset">`. The last item is `<li aria-current="page">`. |
| A dropdown or expandable block without JS | `<details>` and `<summary>` |
| A list of links, or any list without bullets | `<ul class="list-reset">` |
| A field label | `<label for="...">` with a matching `id` on the input, or a wrapping label |

- `<div>` is for layout wrappers. If a meaningful tag exists, use it.
- One `<h1>` per page, in the hero.
- `<br>` appears only to break a large headline in two.
- Emphasis uses `<strong>` and `<em>`. Visual styling belongs in CSS.

---

## 6. Images

- Every `<img>` has `width` and `height` set to the file's real pixel size (the `viewBox` size for an SVG). These only reserve space so the page does not jump. CSS controls the displayed size.
- If CSS gives an image a fixed height, it also sets `width: 100%` or `width: auto`.
- Images below the first screen use `loading="lazy"`. The logo, the home hero art and the first image on a page are not lazy.
- Meaningful images have a descriptive `alt`.
- Decorative images use `alt=""`, plus `aria-hidden="true"` when they sit inside a link or are an icon.
- When a card has both an image link and a text link to the same place, the image link gets `tabindex="-1"` and `aria-hidden="true"`. The text link names the dish.

---

## 7. Accessibility

- A skip link is first on every page.
- The current page is marked `aria-current="page"` on its navigation link and on the last breadcrumb item.
- Every `<nav>` has an `aria-label` (`Main navigation`, `Breadcrumb`, `Table of contents`).
- Decorative SVG icons have `aria-hidden="true"`. A button that holds only an icon has an `aria-label`.
- Every form field has a visible `<label>` or an `aria-label`.
- Interactive features that are not built yet are `disabled`, and the page explains why in text.
- Text that only screen readers need uses `class="visually-hidden"`.

---

## 8. Content

- Headings are in Title Case: `Every Place Has a Flavor to Share.`
- Small category labels are typed in capitals in the HTML (`REGIONAL SPECIALTY`). CSS adds only letter spacing, not `text-transform`.
- Descriptions say what the page really does. A placeholder feature is described as a placeholder.

---

## 9. Not used

- Inline `style="..."` attributes
- HTML comments in page files
- Ids for styling (ids exist only for skip links, anchors such as `#near-spots`, and form labels)
- Inline JavaScript such as `onclick`
- Tables for layout
- Self-closing slashes on void tags

---

## 10. Page checklist

- [ ] Title is `Page Name | Silangan` and a meta description is written
- [ ] The page stylesheet is linked after `style.css`
- [ ] Path prefixes are correct throughout
- [ ] One `<h1>`, and heading levels do not skip
- [ ] `aria-current="page"` is on the correct navigation link
- [ ] Every `<img>` has `width`, `height` and `alt`, with lazy loading below the first screen
- [ ] Tags with four or more attributes are split one per line
- [ ] Indentation is 4 spaces