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
| Quick controls | `--button-*`, `--card-*`, `--band-*` | Changes one kind of element only |

Each quick control starts as a base color, for example `--button-bg: var(--color-primary);`. To change only the buttons, replace the right-hand side with a color:

```css
--button-bg: #C9704A;
--band-bg-alt: #3A1114;
```

Hex colors are written in capitals and only inside `:root`. To go back to following the base palette, put the `var(--color-...)` value back.

---

## 2. Buttons

Used by `.button` and `.button-outline`, and by the page buttons: the Browse link on Log In, Sign Up and Forgot Password (`.auth-browse-link`), All Foods (`.regional-discovery-button`), Map (`.map-explore-button`), Near Me (`.near-explore-button`) and the Recipes cards (`.recipe-card-source-link`, `.recipe-card-method-link`).

| Variable | Changes | Starts as |
| --- | --- | --- |
| `--button-bg` | Filled button: background | `--color-card` (golden caramel) |
| `--button-text` | Filled button: text | `--color-text` (dark sangria) |
| `--button-border` | Filled `.button`: border. Page buttons have no border. | `--color-card` |
| `--button-bg-hover` | Every button: background on hover | `--color-secondary` (light caramel) |
| `--button-text-hover` | Every button: text on hover | `--color-ink` |
| `--button-border-hover` | Every `.button`: border on hover | `--color-secondary` |
| `--button-outline-text` | Outline button: text | `--color-secondary` |
| `--button-outline-border` | Outline button: border | `--color-secondary` |

Notes:

- An outline button (`button button-outline`) has a see-through background. It turns into the hover colors when hovered.
- `.recipe-card-method-link` is an outline-style button, so it uses the two outline variables. Its background stays tan and it uses dark sangria text and border instead, because light caramel would disappear on tan.
- `.recipe-card-source-link` sits on a golden caramel recipe card, where a caramel button would disappear. It is dark sangria (`--color-ink`) with cream text, and turns tan with dark text on hover.
- Disabled buttons (the Log In, Sign Up and Forgot Password submit buttons) have their own muted look and do not use these variables.

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
| 1 | `--card-title` | The card title (`h3`, `h4`) | `--color-text` |
| 2 | `--card-text` | The description | `--color-text` |
| 3 | `--card-label` | Category, location and other small captions | `--color-text` |
| 4 | `--card-link` | Links such as "Discover Kinunot", and the focus ring inside a card | `--color-text` |

Notes:

- `--card-title` is applied by giving each card its own `--color-heading`. Because of that, `--color-heading` is dark sangria inside a card, the same as the text, so small chips over photos (island numbers, region tags, recipe categories) use a tan background with `--color-text`, not the heading color.
- The trivia highlight and the restaurant status line use the base palette.
- A border exists only where a card already had one. The featured food and recipe panels, the island cards and the Terms and Privacy contents box have none. The All Foods and Regions food cards have a faint border of their own that does not follow `--card-border`.

---

## 4. Full-width bands and the page texture

Only the background. A band is a full-width section, such as a hero or a section with its own background.

| Variable | Changes | Starts as |
| --- | --- | --- |
| `--band-bg` | The first band color, used by every other section on a page | `transparent` |
| `--band-bg-alt` | The second band color, used by every page hero and every other section | `transparent` |
| `--page-texture` | The image painted on the page behind everything | `assets/images/backgrounds/texture-red.webp` |

Both band variables are transparent, so no section covers the page. The texture shows through behind every section. It is a dark red marbled pattern (`#280E10` to `#4E1A1E`) on a `--page-bg` base (`#361214`). The image repeats top to bottom with no visible seam, and it is at least 1200px wide, so on a phone the middle of it shows.

Keep the texture dark. Cream text is 10.2:1 on its lightest patch and light caramel text is 7.5:1, so a lighter patch would start to lose contrast. Text on the page uses `--color-text`, `--color-heading` and `--color-secondary` (light caramel). Dark sangria (`--color-ink`) is only for text on cards, buttons, the header, chips and form fields.

To bring the alternating bands back, set `--band-bg` and `--band-bg-alt` to `var(--color-background)` and `var(--color-background-secondary)`, and set the body `background-image` to `none`. The page text is cream now, so also set `--color-text` and `--color-heading` back to `var(--color-ink)`. To make one section solid, set the background in that section's own rule.

The navigation bar and the footer are full width too, but they are not bands. The navigation bar uses `--header-bg` (golden caramel, the same as the cards) and stays solid, and the footer uses `--color-surface` (smoked moss) with light text.

---

## 5. Not covered by these controls

These still use the base palette directly, so change them through `--color-*` or ask for a control:

- Navigation bar text, links and menu button. Only the bar's and dropdown's background has a control, `--header-bg`.
- Footer
- Form fields and labels
- Hero search box and its suggestion chips
- The "Social" sign-in buttons and the 404 link chips
- The mahogany "random food" box on the home page
- Image cards on All Foods (the category cards)
- Tags and number badges
- The skip link
- The focus ring, which has its own variable, `--focus-ring`. It is the primary color on the bands. Dark surfaces set it to a lighter color (see [global.md](global.md#7-variables)).

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
