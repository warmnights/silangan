# Known Issues and Fixes

> **Purpose:** Records the problems found in the latest review of the site, in priority order, with the fix for each.
> **Read when:** Deciding what to fix next, or before submitting the project.
> **Related:** [overview.md](overview.md) · [improvement-plan.md](improvement-plan.md) · [global.md](global.md) · [css-property-guide.md](css-property-guide.md)

**Scope of the review.** The review covered 15 HTML pages, 14 CSS files and 1 JS file, checked against [global.md](global.md) and the guides. It was a read of the code only. The pages were not opened in a browser, so items marked **Check in browser** still need a visual check.

**Status.** Nothing here stops the site from working. Three items remain. Two need JavaScript, so they wait for the JavaScript stage in [improvement-plan.md](improvement-plan.md). The third needs the page texture file to measure.

This file is a snapshot. When an item is fixed, remove it from the table and the list below.

| # | Problem | Where | Priority | Effort |
|---|---|---|---|---|
| 1 | The More menu does not close on Escape or outside click | `js/nav.js` | Medium | Small |
| 2 | The home page says search works, but search is disabled | `index.html` | Medium | Small |
| 3 | The page texture contrast has not been measured | `texture-burgundy.webp` | Low | Small |

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

## 3. The page texture contrast has not been measured

**Problem.** The page background is `texture-burgundy.webp` with a 30% black overlay. The contrast numbers in [global.md](global.md#7-variables) are measured on the flat base color `--page-bg` (cream text 12.2:1, light caramel 9.0:1). The texture has lighter patches, and the lightest one has not been measured. The earlier red texture was measured at its lightest patch, so those numbers do not apply to this file.

**Fix.** Open the texture in an image editor, find its lightest patch, add the 30% black overlay, and check cream text, muted cream and light caramel against it with a contrast checker. Each must stay at 4.5:1 or more. Then write the lightest patch color and the results into [global.md](global.md#7-variables) and [colors.md](colors.md#4-full-width-bands-and-the-page-texture).

---

## Checked and fine

- Every local `href` and `src` resolves, including the `assets/` files, and every `#anchor` has a matching `id`.
- No duplicate ids. Every `aria-controls`, `aria-labelledby` and `aria-describedby` that exists points to a real id.
- Every class in the HTML has a CSS rule. The only unused class in the CSS is `is-open`, which `nav.js` adds.
- No undefined or unused CSS variables, no hex or rgb colors outside `:root`, no `!important`, no empty rules, no tabs, and balanced braces in all 14 CSS files.
- All 15 pages have one `<h1>`, a unique `<title>` in the `Page | Silangan` format, a meta description, `lang`, a viewport tag, the skip link, `id="main"`, one `.container` and `nav.js`.
- The header and footer are identical on all pages apart from the path prefix and `aria-current`.
- All 77 images have `width` and `height`, and every input has a label or `aria-label`.
- Font weights used (400, 600, 700) match the weights loaded.
- CSS property order matches [css-property-guide.md](css-property-guide.md) in all 14 files.
- No pixel border radii (apart from `50%` circles), no `transition: all`, and the menu button names the menu it controls (`aria-controls`).
- The home page buttons use the shared `.button` and `.button-outline` classes.
- Only the documented breakpoints are used, plus a landscape height query, pointer-type queries and the glass header queries.

## Informational, not issues

- **`404.html` uses root paths** (`/css/style.css`). This is correct on Vercel. It looks unstyled when the file is opened straight from a folder, so test it through the live site or a local server.
- **Wine accent on the red page** is only 2.1:1, so page-level accent text and focus rings use light caramel (`--color-secondary`, 9.0:1). The wine accent stays on light parts such as form field focus borders.
- **The hero artwork is gone.** The home hero no longer uses the sky, sun and mountain images, so no page references them. If `sky.svg`, `sun.svg`, `mountains-back.svg` or `mountains-front.svg` are still in `assets/images/backgrounds/`, they can be deleted.
- **Open Graph tags** (the preview card when the link is shared) are not set. This is an improvement, not a fix. See [improvement-plan.md](improvement-plan.md).

## Check in browser before submitting

1. Open the More menu, then click outside it and press Escape (item 1).
2. Home hero: the search box, the tagline and the nav logo (item 2).
3. Hover the buttons on Log In, Near Me and the 404 page. They are golden caramel with dark sangria text and turn light caramel on hover, so check that the change is easy to see.
4. Home hero: the headline, tagline, search box and chips are cream on the page texture. Check that they read clearly, including the italic tagline and the placeholder text in the search box.
5. Tab through a page with cards. The focus ring inside a golden caramel card should be dark sangria. On the red page it should be light caramel, in the footer light caramel, and in the random food box cream.
6. Home page buttons (Near Me, fresh finds, popular foods and the surprise dish) at 1280px and 360px wide. They use the shared button classes, so size and hover color changed slightly.
7. The photo corners on the Log In page, now `--radius-lg`.
8. Foods, top foods and regions cards at 360px wide. Link text such as "Discover La Paz Batchoy →" should fit on one line.
9. Reload a long page such as foods with the Network tab open. Content should not jump as images load, and `.woff2` fonts should load with no 404s.
10. Turn on "reduce motion" in the system settings. Smooth scrolling and card lifts should stop.
11. Download the shared Google Drive folder to a new location and open `index.html`. The `css`, `pages`, `js` and `assets` folders must still be in place, and the styles, images and fonts must load.