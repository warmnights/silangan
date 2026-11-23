# Color Controls

> **Purpose:** Lists the variables that control the colors of buttons, cards and full-width sections, what each one changes, and where it is used. Use it to try a new palette quickly.
> **Read when:** Changing colors, trying a palette, or adding a new button, card or section.
> **Related:** [overview.md](overview.md) · [global.md](global.md) · [css-writing-guide.md](css-writing-guide.md)

The variables are in `:root` at the top of `css/style.css`, in the block called **QUICK COLOR CONTROLS**. Every page uses them, so one change shows up on every page.

---

## 1. How to change colors

There are two layers, and both live in `:root`.

| Layer | Variables | What a change does |
| --- | --- | --- |
| Base palette | `--color-background`, `--color-primary`, `--color-accent` and the others | Changes everything that uses that color, including parts with no quick control (nav, footer, forms) |
| Quick controls | `--button-*`, `--card-*`, `--header-*`, `--band-*` | Changes one kind of element only |

Each quick control starts as a base color, for example `--button-bg: var(--color-card);`. To change only the buttons, replace the right-hand side with a color:

```css
--button-bg: #C9704A;
--band-bg-alt: #3A1114;
```

Hex colors are written in capitals and only inside `:root`. To go back to following the base palette, put the `var(--color-...)` value back.

---

## 2. Buttons

Used by `.button` and `.button-outline`, and by the page buttons: the Browse link on Log In, Sign Up and Forgot Password (`.auth-browse-link`), the submit buttons on those pages (`.auth-button`), the Search button in the home hero (`.hero-search button`), All Foods (`.regional-discovery-button`), Map (`.map-explore-button`) and Near Me (`.near-explore-button`).

| Variable | Changes | Starts as |
| --- | --- | --- |
| `--button-bg` | Filled button: background | `--color-card` (golden caramel) |
| `--button-text` | Filled button: text | `--color-ink` (dark sangria) |
| `--button-border` | Filled `.button`: border. Page buttons have no border. | `--color-card` |
| `--button-bg-hover` | Every button: background on hover | `--color-secondary` (light caramel) |
| `--button-text-hover` | Every button: text on hover | `--color-ink` |
| `--button-border-hover` | Every `.button`: border on hover | `--color-secondary` |
| `--button-outline-text` | Outline button: text | `--color-secondary` |
| `--button-outline-border` | Outline button: border | `--color-secondary` |

Notes:

- An outline button (`button button-outline`) has a see-through background. It turns into the hover colors when hovered.
- The two Recipes card links do not use these variables, because they sit on golden caramel recipe cards, where a caramel button would disappear. `.recipe-card-source-link` is dark sangria (`--color-ink`) with cream text, and turns tan with dark text on hover. `.recipe-card-method-link` is tan with dark sangria text, and turns cream on hover.
- The Log In, Sign Up and Forgot Password submit buttons (`.auth-button`) take the filled colors, but while they are disabled they show a muted look of their own: dark sangria text on tan, with a border.

---

## 3. Cards

Used by every card: `.card` (fresh finds, regional discoveries, popular foods, random food and recipe cards), the trivia card, the restaurant notice, the featured food and recipe panels, the recipe method boxes, the island cards, the regional food cards, and the All Foods and Top Foods cards.

### Backgrounds and border

| Variable | Changes | Starts as |
| --- | --- | --- |
| `--card-bg` | Background of a card on the darker band | `--color-card` (golden caramel) |
| `--card-bg-alt` | Background of a card on the lighter band (`.card-on-secondary`, the All Foods and Top Foods cards) | `--color-card` |
| `--card-border` | Border of the cards that have one | `--color-border` |

A card sits on one kind of band, so the two backgrounds are separate controls. Both start as golden caramel, which stands out on the dark red page. Set `--card-bg-alt` to another color to make cards on the lighter band different.

### Text, from most to least important

| Level | Variable | Changes | Starts as |
| --- | --- | --- | --- |
| 1 | `--card-title` | The card title (`h3`, `h4`) | `--color-ink` |
| 2 | `--card-text` | The description | `--color-ink` |
| 3 | `--card-label` | Category, location and other small captions | `--color-ink` |
| 4 | `--card-link` | Links such as "Discover Kinunot", and the focus ring inside a card | `--color-ink` |

Notes:

- `--card-title` is applied by giving each card its own `--color-heading`, and `--card-link` by giving it its own `--focus-ring`. Because of that, headings and the keyboard focus ring inside a card are dark sangria, the same as the text. Small chips over photos (island numbers, region tags, recipe categories) use a tan or light caramel background with `--color-ink`, never the heading color, which is cream on the red page.
- The trivia highlight follows `--card-label`.
- A border exists only where a card already had one. The featured food and recipe panels, the island cards and the Terms and Privacy contents box have none. The All Foods and Regions food cards have a faint border of their own that does not follow `--card-border`.

---

## 4. Full-width bands and the page texture

Only the background. A band is a full-width section, such as a hero or a section with its own background.

| Variable | Changes | Starts as |
| --- | --- | --- |
| `--band-bg` | The first band color, used by every other section on a page | `transparent` |
| `--band-bg-alt` | The second band color, used by every page hero and every other section | `transparent` |
| `--page-texture` | The image painted on the page behind everything | `assets/images/backgrounds/texture-burgundy.webp` |
| `--page-overlay` | A dark layer painted over the texture, to keep the page dark | `rgba(0, 0, 0, 0.3)` (30% black) |

Both band variables are transparent, so no section covers the page. The texture shows through behind every section. It is a dark burgundy marbled pattern that repeats as a 754px by 955px tile, left to right and top to bottom, from the top center of the page. The `body` paints it over the `--page-bg` base (`#361214`) with the 30% black `--page-overlay` on top.

Keep the texture dark. On the base color `--page-bg` cream text is 12.2:1, muted cream 8.1:1 and light caramel text 9.0:1, and the overlay only darkens the page more. A lighter patch in the texture lowers these. The lightest patch of `texture-burgundy.webp` has not been measured yet (see [bugs.md](bugs.md)), so check cream text on the lightest patch before changing the texture or the overlay. Text on the page uses `--color-text`, `--color-heading` and `--color-secondary` (light caramel). Dark sangria (`--color-ink`) is only for text on cards, buttons, chips and form fields.

To bring the alternating bands back, set `--band-bg` and `--band-bg-alt` to `var(--color-background)` and `var(--color-background-secondary)`, and set the body `background-image` to `none`. The page text is cream now, so also set `--color-text` and `--color-heading` back to `var(--color-ink)`. To make one section solid, set the background in that section's own rule.

The navigation bar and the footer are full width too, but they are not bands. The navigation bar is dark glass and the footer uses `--color-surface` (smoked moss) with light text. The glass runs the full screen width and has three controls: `--header-bg` is the see-through tint over the page (the page color at 80%), `--header-blur` is how much the page behind it is blurred (14px), and `--header-menu-bg` is the almost solid tint (96%) behind the dropdown and the open phone menu. The tint is tested at 80%. Over pure white, the worst case, cream links are 6.6:1 and the light caramel hover is 4.9:1. Over a golden caramel card they are 9.1:1 and 6.7:1. Below 80% the hover can drop under 4.5:1 over very light content (4.1:1 at 75%), so check the links before lowering it. Browsers without `backdrop-filter`, and visitors who ask for less transparency, get a solid bar in `--page-bg`.

---

## 5. Not covered by these controls

These still use the base palette directly, so change them through `--color-*` or ask for a control:

- Navigation bar text, links and menu button. Only the bar's and dropdown's background has controls, `--header-bg`, `--header-menu-bg` and `--header-blur`.
- Footer
- Form fields and labels
- Hero search box and its suggestion chips
- The "Social" sign-in buttons and the 404 link chips
- The mahogany "random food" box on the home page
- Image cards on All Foods (the category cards)
- Tags and number badges
- The skip link
- The focus ring, which has its own variable, `--focus-ring`. It is light caramel (`--color-secondary`) on the red page. Cards set it to dark sangria, the footer keeps light caramel, and the random food box sets it to cream, so it stays visible on every surface (see [global.md](global.md#7-variables)).

---

## 6. Check the contrast

When text and background colors change, check each pair with a contrast checker. Small text needs at least 4.5:1.

| Text | On |
| --- | --- |
| `--button-text` | `--button-bg` |
| `--button-text-hover` | `--button-bg-hover` |
| `--button-outline-text` | the red page |
| `--card-title`, `--card-text`, `--card-label`, `--card-link` | both `--card-bg` and `--card-bg-alt` |
| `--color-text`, `--color-heading` | the red page, at its lightest patch |
| `--color-text` and `--color-heading` | both bands |

The current pairs are listed in [global.md](global.md#7-variables).

---

## 7. Adding new things

- A new button uses `class="button"`, or `class="button button-outline"`. A page button written with its own class takes its colors from the `--button-*` variables.
- A new card uses `class="card"`, with `card-on-secondary` when it sits on a lighter band. A page card background uses `var(--card-bg)` or `var(--card-bg-alt)`, and its border uses `var(--card-border)`.
- A new full-width section carries `full-bleed` in the HTML and needs no background, because the page texture shows through. Add `background-color: var(--band-bg)` only to make it solid.
- The base colors are not used directly for these three things.