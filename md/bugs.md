# Known Issues and Fixes

> **Purpose:** Records the problems found in the latest review of the site, in priority order, with the fix for each.
> **Read when:** Deciding what to fix next, or before submitting the project.
> **Related:** [overview.md](overview.md) · [improvement-plan.md](improvement-plan.md) · [global.md](global.md) · [css-property-guide.md](css-property-guide.md)

**Scope of the review.** The review covered 15 HTML pages, 14 CSS files and 1 JS file, checked against [global.md](global.md) and the guides. It was a read of the code only. The pages were not opened in a browser, so items marked **Check in browser** still need a visual check.

**Status.** Nothing here stops the site from working. The items can be done in order.

This file is a snapshot. When an item is fixed, remove it from the table and the list below.

| # | Problem | Where | Priority | Effort |
|---|---|---|---|---|
| 1 | The More menu does not close on Escape or outside click | `js/nav.js` | Medium | Small |
| 2 | The home page says search works, but search is disabled | `index.html` | Medium | Small |
| 3 | Four border radii are not variables | 3 CSS files | Low | Small |
| 4 | A stale comment in `style.css` | `style.css:264` | Low | 1 minute |
| 5 | The menu button has no `aria-controls` | 15 HTML files | Low | Small |
| 6 | The Log In page does not mark Log In as current | `login.html` | Low | 1 minute |
| 7 | Some home page buttons repeat `.button` styles | `style.css` | Low | Medium |
| 8 | Three `transition: all` rules | `style.css` | Low | Small |
| 9 | Stray blank line in the nav on every page | 15 HTML files | Cosmetic | Small |
| 10 | `isolation` sits in the position block of `.hero` | `style.css` | Cosmetic | 1 minute |
| 11 | Two short state rules skip the blank line between groups | `style.css`, `auth.css` | Cosmetic | 1 minute |

---

## 1. The More menu does not close on Escape or outside click

**Problem.** "More" in the nav is a `<details>` element. `js/nav.js` handles only the hamburger menu (`.is-open`). On a wide screen the More menu stays open when the page is clicked or Escape is pressed. It closes only when "More" is clicked again.

**Check in browser.** On a wide screen, open More, then click elsewhere on the page.

**Fix.** The project avoids JavaScript, so this is optional. The menu works with mouse and keyboard as it is. To add it, place this inside the `(function () { ... })();` in `js/nav.js`, before the closing line:

```js
    // The "More" dropdown: close it on Escape or when clicking outside
    var dropdown = nav.querySelector('.site-nav-dropdown');

    if (dropdown) {
        document.addEventListener('click', function (event) {
            if (!dropdown.contains(event.target)) {
                dropdown.removeAttribute('open');
            }
        });

        document.addEventListener('keydown', function (event) {
            if (event.key === 'Escape' && dropdown.hasAttribute('open')) {
                dropdown.removeAttribute('open');
                dropdown.querySelector('summary').focus();
            }
        });
    }
```

## 2. The home page says search works, but search is disabled

**Problem.** The home hero says "Search Filipino dishes by name or ingredient." The search input and the Search button both have `disabled`, so nothing can be typed. The two "Try a search" chips (Chicken Adobo, Sinigang) open the "coming soon" page.

**Impact.** Search is the first thing visitors try. A disabled box under a promise looks broken.

**Fix, simple.** Make the wording honest:
- Tagline: "Dish search is coming soon." or "Browse Filipino dishes by name, region or ingredient."
- Keep `disabled`, and change the chips label to "Popular dishes:".

**Fix, complete.** Make it work. This is Stage 2.2 of [improvement-plan.md](improvement-plan.md). A small script sends the search to the All Foods page and filters the cards there.

The README lists search as a known limitation.

## 3. Four border radii are not variables

[global.md](global.md) says to use the `--radius-*` variables instead of pixel values. These four still use pixels:

| File | Line | Selector | Value |
|---|---|---|---|
| `style.css` | 1286 | `.trivia-highlight` | `20px` |
| `regions.css` | 273 | `.featured-region-tag` | `20px` |
| `regions.css` | 452 | `.regional-food-image span` | `20px` |
| `auth.css` | 297 | `.auth-image` | `14px` |

**Fix.** The three `20px` ones are small labels and tags, so `var(--radius-pill)` looks almost the same. For `14px`, use `var(--radius-md)` (12px) or `var(--radius-lg)` (16px). Check each in the browser, because the shape changes slightly.

## 4. A stale comment in `style.css`

Line 264 has a banner that reads "NOT USED ON THIS PAGE (kept for other pages)" above `.section-eyebrow`. It dates from when the file served one page. `.section-eyebrow` is now used on many pages.

**Fix.** Rename the banner to `SECTION EYEBROW`, or remove it.

## 5. The menu button has no `aria-controls`

The hamburger button has `aria-expanded` but does not name the element it controls. Screen readers use `aria-controls` to link the button to the menu.

**Fix.** Give the nav list an id and point the button at it, on every page:

```html
<button
    class="site-nav-toggle"
    type="button"
    aria-label="Open menu"
    aria-expanded="false"
    aria-controls="site-nav-menu">
```

```html
<ul id="site-nav-menu" class="site-nav-links list-reset">
```

## 6. The Log In page does not mark Log In as current

On `login.html` the **Log In** button in the nav links to the same page but has no `aria-current="page"`.

**Fix.**

```html
<a href="login.html" class="site-nav-cta" aria-current="page">
```

Sign Up is not in the nav, so `signup.html` needs nothing.

## 7. Some home page buttons repeat `.button` styles

`.random-food-button` has its own padding, radius, weight and hover. `.fresh-finds-link` looks like `.button-outline` (primary border, small radius, 0.9rem, weight 600). The home page was written before the shared `.button`, so these were not moved over.

**Fix (optional).** Replace them with `class="button"` and `class="button button-outline"` in `index.html`, and delete the old rules. `.near-me-link` and the other link-style buttons on the home page need the same check. Do this last and look at the home page afterwards, because sizes change slightly (padding and font size differ by a few pixels).

## 8. Three `transition: all` rules

`style.css` lines 982, 1082 and 1481 use `transition: all 0.2s ease`. It animates every property that changes, which can include layout properties by accident.

**Fix.** List the properties that change on hover, for example `transition: color 0.2s ease, background-color 0.2s ease;`.

## 9. Stray blank line in the nav

Every page has an empty line inside `<div class="site-nav-actions">`, between the Log In link and the closing `</div>`. It has no visible effect.

**Fix.** Delete the blank line in each page.

## 10. `isolation` sits in the position block of `.hero`

[css-property-guide.md](css-property-guide.md) puts `isolation` in the layout group. In `.hero`, it is written under `position`.

**Fix.** Move `isolation: isolate;` to the layout block, after `display` and `align-items` and before `overflow`.

## 11. Two short state rules skip the blank line between groups

The property guide separates groups with a blank line in every rule. Two short rules keep a box-model property and a visual property together:

- `.form-group input:focus` in `style.css` (`border-color`, then `box-shadow`)
- `.social-button:hover:not(:disabled)` in `auth.css`

**Fix.** Add a blank line between the two groups.

---

## Checked and fine

- Every local `href` and `src` resolves, including the `assets/` files, and every `#anchor` has a matching `id`.
- No duplicate ids. Every `aria-controls`, `aria-labelledby` and `aria-describedby` that exists points to a real id.
- Every class in the HTML has a CSS rule. The only unused class in the CSS is `is-open`, which `nav.js` adds.
- No undefined or unused CSS variables, no hex or rgb colors outside `:root`, no `!important`, no empty rules, no tabs, and balanced braces in all 14 CSS files.
- All 15 pages have one `<h1>`, a unique `<title>` in the `Page | Silangan` format, a meta description, `lang`, a viewport tag, the skip link, `id="main"`, one `.container` and `nav.js`.
- The header and footer are identical on all pages apart from the path prefix and `aria-current`.
- All 81 images have `width` and `height`, and every input has a label or `aria-label`.
- Font weights used (400, 600, 700) match the weights loaded.
- CSS property order matches [css-property-guide.md](css-property-guide.md) in all 14 files, apart from items 10 and 11.
- Only the documented breakpoints are used, plus a landscape height query and pointer-type queries.

## Informational, not issues

- **`404.html` uses root paths** (`/css/style.css`). This is correct on Vercel. It looks unstyled when the file is opened straight from a folder, so test it through the live site or a local server.
- **Orange eyebrow text on the secondary background** has 4.67:1 contrast. That passes WCAG AA (4.5:1) with little room, so the text is not darkened or shrunk.
- **Open Graph tags** (the preview card when the link is shared) are not set. This is an improvement, not a fix. See [improvement-plan.md](improvement-plan.md).

## Check in browser before submitting

1. Open the More menu, then click outside it and press Escape (item 1).
2. Home hero: the search box, the tagline and the nav logo (item 2).
3. Hover the buttons on Log In, Near Me and the 404 page. White text must not appear on orange.
4. Foods, top foods and regions cards at 360px wide. Link text such as "Discover La Paz Batchoy →" should fit on one line.
5. Reload a long page such as foods with the Network tab open. Content should not jump as images load, and `.woff2` fonts should load with no 404s.
6. Turn on "reduce motion" in the system settings. Smooth scrolling and card lifts should stop.
7. Download the shared Google Drive folder to a new location and open `index.html`. The `css`, `pages`, `js` and `assets` folders must still be in place, and the styles, images and fonts must load.