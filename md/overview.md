# Silangan: Project Overview

> **Purpose:** Start here. This page describes what the site is, how the project is laid out, and which document answers which question.
> **Audience:** The site's author, teachers and reviewers, and anyone who needs to work on the site without breaking its conventions.

---

## 1. What Silangan is

Silangan is a Filipino food and culture website made as a school project. It lets visitors browse dishes, regions and recipes. It is built as plain files, with no framework and no build step.

| Item | Detail |
| --- | --- |
| Languages | HTML and CSS |
| JavaScript | One small script, `js/nav.js`, for the mobile menu button |
| Fonts | Fraunces (headings) and Poppins (body), self-hosted as `.woff2` |
| Look | Dark maroon background, gold primary color, orange accent |
| Hosting | Vercel (static) |
| Submission | The files inside the root folder are uploaded to a Google Drive folder, and the folder link is sent to the instructor |
| Pages | 15 HTML files: `index.html`, `404.html`, and 13 in `pages/` |

### Design principle

**Everything is written the same way on every page.** The header and footer are identical, every page has the same head and structure, every color comes from a variable, and every CSS rule lists its properties in the same order. The documents in this folder describe those conventions so a new page or stylesheet is indistinguishable from an existing one.

### Constraints

These are deliberate and apply to all new work:

- **JavaScript is kept to a minimum.** The author is still learning it. The only script is `js/nav.js`. Anything CSS or HTML can do (the More dropdown, hover states, focus styles) is done there.
- **No frameworks, no build tools.** The submitted project is plain files that open in a browser.
- **No `!important`, no id selectors, no inline `style` attributes.**
- **No hex or rgb colors outside `:root`.** Use the `--color-*` variables.
- **The header and footer are copied into every page.** There are no includes, so a change to them is made in all 15 files.

---

## 2. Project layout

```
index.html                 home page
404.html                   not-found page (uses root paths, see global.md)
pages/                     the other 13 pages
css/
├── style.css              shared styles, loaded on every page
└── <page>.css             one file per page or group of pages
js/
└── nav.js                 mobile menu button
assets/
├── fonts/                 .woff2 files
└── images/                logo, foods, backgrounds, icons
md/                        these documents
```

### Pages and their stylesheets

Every page loads `style.css` first, then its own file.

| Page | Title | Own stylesheet |
| --- | --- | --- |
| `index.html` | Filipino Food & Culture | none (home styles live in `style.css`) |
| `404.html` | Page Not Found | `not-found.css` |
| `pages/top-foods.html` | Filipino Food Favorites | `top-foods.css` |
| `pages/foods.html` | Explore Filipino Foods | `foods.css` |
| `pages/food-details.html` | Coming Soon | `food-details.css` |
| `pages/regions.html` | Explore Regions | `regions.css` |
| `pages/near-me.html` | Near Me | `near-me.css` |
| `pages/recipes.html` | Filipino Recipes | `recipes.css` |
| `pages/map.html` | Explore the Philippines | `map.css` |
| `pages/random-food.html` | Random Food Finder | `random-food.css` |
| `pages/login.html` | Log In | `auth.css` |
| `pages/signup.html` | Sign Up | `auth.css`, `signup.css` |
| `pages/forgot-password.html` | Forgot Password | `auth.css`, `forgot-password.css` |
| `pages/privacy.html` | Privacy Policy | `legal.css` |
| `pages/terms.html` | Terms of Service | `legal.css` |

### Features that are placeholders

Some features are not built yet. The pages say so in plain text and use `disabled` controls instead of pretending to work.

- Dish search on the home page
- Log In, Sign Up and Forgot Password
- Near Me (shows Albay as a sample area, not the visitor's location)
- Dish detail pages (cards link to one "Coming Soon" page)

See [bugs.md](bugs.md) and [improvement-plan.md](improvement-plan.md) for how these are tracked.

---

## 3. Documentation map

| Document | Answers the question |
| --- | --- |
| [overview.md](overview.md) | What is this site, and where is everything? |
| [html-writing-guide.md](html-writing-guide.md) | How is HTML written, and how are pages structured and classes named? |
| [css-writing-guide.md](css-writing-guide.md) | How is a stylesheet organized, commented and written? |
| [css-property-guide.md](css-property-guide.md) | In what order are properties written inside a rule? |
| [global.md](global.md) | What does `style.css` already provide, so page files do not repeat it? |
| [bugs.md](bugs.md) | What is wrong right now, and how is it fixed? |
| [improvement-plan.md](improvement-plan.md) | What is planned next, in what order? |

The first five describe **how the site is built** and change rarely. The last two describe **the state of the site** and change often.

When two documents seem to disagree, the order of authority is: the code in `css/` and the HTML pages, then `global.md`, then the two guides. Then fix whichever is wrong.

---

## 4. Naming

| Thing | Convention | Example |
| --- | --- | --- |
| Files and folders | lowercase, words joined with hyphens | `top-foods.html`, `near-me.css` |
| Documents | the same, lowercase | `css-property-guide.md` |
| CSS classes | the same, flat, no `__` or `--` | `fresh-find-card` |
| CSS variables | `--group-name` | `--color-primary`, `--radius-md` |

- Lowercase everywhere avoids problems between systems. Windows and macOS (by default) ignore case in file names, while Vercel and Linux do not. A link written `Top-Foods.html` works on a laptop and fails after deploy.
- `README.md` and `LICENSE` are the only exceptions, because GitHub and other tools look for those names in capitals.
- Documents link to each other by their lowercase file names, so renaming one means updating every link to it.

---

## 5. Making a change

1. **Find out what already exists.** Check [global.md](global.md) before writing any CSS. If `style.css` already sets it, do not repeat it in a page file.
2. **Match a neighbor.** Copy the closest existing section, card or page, then change the content. Page structure and the header and footer are copied, not rewritten.
3. **Follow the guides.** HTML in [html-writing-guide.md](html-writing-guide.md), stylesheets in [css-writing-guide.md](css-writing-guide.md), property order in [css-property-guide.md](css-property-guide.md).
4. **Check paths.** Pages in `pages/` use `../` to reach the root. `index.html` uses none.
5. **Test.** Resize to 360px, 768px and 1280px, tab through the page with the keyboard, and open the page from a fresh copy of the project folder.
6. **Update the documents.** If a convention changes, change the guide in the same edit.