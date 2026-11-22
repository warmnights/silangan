# Improvement Plan

> **Purpose:** Lists what can be added to Silangan next, from easiest to hardest, with the effort and prerequisites for each.
> **Read when:** Choosing what to build after the fixes in [bugs.md](bugs.md).
> **Related:** [overview.md](overview.md) · [bugs.md](bugs.md) · [css-property-guide.md](css-property-guide.md) · [global.md](global.md)

**Current state.** Silangan is a clean, accessible, static HTML and CSS site with one small script (`js/nav.js`). Every required fix from the audit guide is done.

**How the stages work.** Each stage builds on the one before. The site is finished after any stage, so work can stop at any point. Fix the items in [bugs.md](bugs.md) first, because a few are visible to visitors.

**Constraint.** The project keeps JavaScript to a minimum (see [overview.md](overview.md)). Stage 1 needs only HTML and CSS. Stage 2 needs beginner JavaScript and is optional. Stage 3 changes how the project is built.

| Label | Meaning |
|---|---|
| **Effort** | Small: under an hour. Medium: an evening. Large: several days. |
| **Needs** | What must be learned or added beyond HTML and CSS |
| **Skip if** | A reason not to do it |

---

## Stage 1: HTML and CSS only

### 1.1 Share previews and search engine basics
**Effort:** small. **Needs:** HTML only.

Pasting the link into Messenger, Discord or a group chat currently gives a plain link. Open Graph tags in the `<head>` of each page add a preview card:

```html
<meta property="og:title" content="Silangan | Filipino Food & Culture">
<meta property="og:description" content="Discover Filipino dishes, region by region.">
<meta property="og:image" content="https://silangan-ph.vercel.app/assets/images/readme/silangan-hero.png">
<meta property="og:url" content="https://silangan-ph.vercel.app/">
<meta property="og:type" content="website">
<meta name="theme-color" content="#DDC5A3">
```

Also:
- `<link rel="canonical" href="...">` on each page
- a `robots.txt` and a `sitemap.xml` in the root, listing the 14 public pages
- an Apple touch icon (a 180×180 PNG of the logo)

The preview image must be a full URL. A dedicated 1200×630 picture works best, rather than reusing the README screenshot. New head tags go in the order described in [html-writing-guide.md](html-writing-guide.md#1-page-skeleton).

### 1.2 Resize and convert the photos
**Effort:** medium. **Needs:** an image tool such as Squoosh (free, in the browser).

This is the largest speed gain left. The photos are 1000 to 2500 pixels wide but show in boxes about 220 to 450 pixels tall. For example, `lechon-kawali.jpg` is 2500×1500 and 850 KB, and the whole `assets/images` folder is about 6.8 MB.

1. Export each photo at about 800px wide as **WebP**, usually 50 to 70% smaller than JPG.
2. For the hero and large images, also export a 1400px version.
3. Use `srcset` so phones download the small one:
   ```html
   <img
       src="../assets/images/foods/sinigang-800.webp"
       srcset="../assets/images/foods/sinigang-800.webp 800w,
               ../assets/images/foods/sinigang-1400.webp 1400w"
       sizes="(max-width: 768px) 90vw, 400px"
       width="1300"
       height="566"
       alt="Filipino Sinigang">
   ```
4. Keep `width` and `height` at the shape of the full-size image.

**Skip if** short on time. Doing only the five biggest files gives most of the gain.

### 1.3 Print styles for recipes
**Effort:** small. **Needs:** CSS only.

A `@media print` block in `recipes.css` that hides the nav, footer and buttons and switches to black text on white. People print recipes.

### 1.4 A dark theme
**Effort:** medium. **Needs:** CSS only.

The site is light now: a tan page with golden caramel cards. Every color is a variable, so a dark version for visitors who prefer it is one block:

```css
@media (prefers-color-scheme: dark) {
    :root {
        --color-background: #2C0E11;
        /* ... the other colors ... */
    }
}
```

Contrast must be re-checked for every color pair with a contrast checker, including the quick controls in [colors.md](colors.md). Photos that carry text (`.auth-image-overlay`, `.page-hero-split-image-caption`) already sit on a dark overlay. This is worth doing only as a learning exercise.

### 1.5 Page transitions
**Effort:** small. **Needs:** CSS only.

```css
@view-transition {
    navigation: auto;
}
```

In supporting browsers, moving between pages fades instead of flashing white. Other browsers ignore it. Check current support on caniuse.com, and wrap it in `@media (prefers-reduced-motion: no-preference)` so it respects reduced motion.

### 1.6 Cache headers on Vercel
**Effort:** small. **Needs:** one config file.

A `vercel.json` caches fonts and images for a long time:

```json
{
  "headers": [
    {
      "source": "/assets/(.*)",
      "headers": [
        { "key": "Cache-Control", "value": "public, max-age=31536000, immutable" }
      ]
    }
  ]
}
```

**Skip if** files are edited without renaming them. Visitors keep the old copy for a year, so a changed file needs a new name (for example `sinigang-v2.webp`).

### 1.7 Test and measure
**Effort:** small. **Needs:** Chrome.

The scores make useful evidence for reviewers and show what to fix next.
- **Lighthouse** (Chrome DevTools, Lighthouse tab): performance, accessibility, best practices, SEO
- **W3C HTML validator** (validator.w3.org) and **CSS validator** (jigsaw.w3.org/css-validator)
- **WAVE** or **axe DevTools** browser extensions for accessibility
- Manual tests at 360px, 768px and 1280px wide, in portrait and landscape
- Tabbing through every page with only the keyboard

---

## Stage 2: a little JavaScript

None of these needs a framework. Each is 20 to 60 lines in `js/`, loaded with `defer` like `nav.js`, and each is built so the page still works if the script fails (progressive enhancement). Adding any of them lifts the "one script" constraint in [overview.md](overview.md), so that document and [global.md](global.md) are updated in the same change.

### 2.1 Close the More menu properly
**Effort:** small. The fix is in [bugs.md](bugs.md), item 1. It is a short first script with a visible result.

### 2.2 Dish search and filter
**Effort:** medium. **Needs:** `querySelectorAll`, `addEventListener`, `data-` attributes.

The home page promises search. Styles for a dish search were once in `foods.css` and were removed as unused.
1. Put the dish name and main ingredients in `data-name` and `data-ingredients` on each card in `foods.html`.
2. On input, hide the cards that do not contain the text (`card.hidden = true`).
3. Show a "No dishes found" message when nothing matches.
4. Enable the home search box and send it to `foods.html?q=adobo`, then read `q` from the URL on the foods page.

### 2.3 A real "Random food" button
**Effort:** small. **Needs:** an array and `Math.random()`.

Keep a list of dishes (name, region, image, link) in a script and pick one on click. If `random-food.html` currently shows fixed cards, this makes the page do what its name says.

### 2.4 Recipe helpers
**Effort:** medium. **Needs:** `localStorage`.
- A servings scaler (2, 4, 6) that multiplies ingredient amounts
- Tick-off checkboxes for ingredients and steps that remember what was ticked
- A "Print recipe" button (`window.print()`) that works with 1.3

### 2.5 Favorites
**Effort:** medium. **Needs:** `localStorage`, arrays.

A heart button on dish cards that saves the dish name in `localStorage`, plus a "My favorites" section. This gives the disabled Log In page a reason to exist later.

### 2.6 Back-to-top and "you are here" navigation
**Effort:** small. A back-to-top button on long pages (foods, recipes), and highlighting the current section in a table of contents with `IntersectionObserver`.

### 2.7 Near Me with the browser's location
**Effort:** medium to large. **Needs:** the Geolocation API.

Ask for permission, then sort a list of restaurants (stored in an array in the script) by distance. "Permission denied" is always handled, and the static Albay sample stays as the fallback. The location is never sent anywhere.

### 2.8 A real map
**Effort:** medium to large. **Needs:** an outside library.

Replace the static map PNG with a Leaflet map using OpenStreetMap tiles, with pins for regions or restaurants. The OpenStreetMap credit must stay on the map. Leaflet loads from a CDN, and the static image stays as the fallback.

---

## Stage 3: bigger steps

These change how the project is built, and reviewers may expect plain files. They are worth doing only to learn them.

### 3.1 Put the dishes in one data file
**Effort:** large. **Needs:** JSON and JavaScript, or a static site generator.

Every dish currently appears by hand in several pages (home, foods, top foods, regions), and 53 links point to the same placeholder. A single `foods.json` with every dish (name, region, category, image, ingredients, steps) can build the cards, so changing a dish is one edit.

### 3.2 Real dish detail pages
**Effort:** large. **Needs:** 3.1, or a lot of copy and paste.

Replace `food-details.html` with a page for each dish, either `food-details.html?dish=sinigang` reading from the JSON or one HTML file per dish. The 53 links are updated to match. Starting with the six most popular dishes keeps it manageable.

### 3.3 Shared header and footer with a build step
**Effort:** large. **Needs:** Node and a tool such as Eleventy.

The header and footer are copied into all 15 pages. A build step could include them from one file. It pays off only when the site grows, and the submitted files would be the built output. For a school project handed in as plain files, leaving them copied is fine.

### 3.4 Working accounts and saved favorites
**Effort:** large. **Needs:** a backend service.

Use a service such as Supabase or Firebase Auth. **Never write custom password storage.** Follow the service's sign-up flow, then save favorites per user. `privacy.html` must be updated to match what is actually stored.

### 3.5 Installable and offline
**Effort:** large. **Needs:** a web app manifest and a service worker.

Lets people install Silangan on a phone's home screen and read saved recipes offline. It suits a recipe site. It comes after 3.1 so the files to cache are known.

### 3.6 Filipino and English
**Effort:** large.

A language toggle (`lang="fil"` and `lang="en"`) with the text kept in two JSON files. Worth doing only when the Filipino text is written by, or checked by, a native speaker.

---

## Content ideas

These need no code.

- More dishes per region. Visayas and Mindanao have fewer than Luzon.
- A short story for each dish: where it comes from, what it tastes like, when it is served.
- A glossary page (kinilaw, sinigang, sawsawan, merienda).
- A "where to try it" list tied to the Near Me page.
- A credits and sources page for the photo sources and the recipe sites linked (Kawaling Pinoy, Inquirer, Naga City tourism).
- More descriptive alt text, such as "A bowl of sour pork sinigang with tamarind broth".

## Developer habits

- **Stylelint** with the `stylelint-order` plugin can check property order automatically from [css-property-guide.md](css-property-guide.md). It runs on the author's computer and does not change the site.
- **Prettier** keeps indentation and spacing consistent.
- Short, clear **commit messages**, with one commit per fix.
- A **changelog** or GitHub Releases to mark the submitted version (`v1.0`).
- **Vercel branch previews**, so each change is tested on its preview link before merging to `main`.

## Suggested order

1. Fix [bugs.md](bugs.md) items 1 and 2.
2. Do 1.1 (share previews) and 1.7 (run Lighthouse and the validators).
3. Do 1.2 (resize images), the largest performance win left.
4. Pick one Stage 2 feature, probably 2.2 (dish search), because the home page already promises it.
5. Add dish pages (3.1 and 3.2) if time allows before the deadline.
6. Leave accounts, offline support and translation for a later version.