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
| `--button-bg` | Filled button: background | `--color-primary` |
| `--button-text` | Filled button: text | `--color-background` |
| `--button-border` | Filled `.button`: border. Page buttons have no border. | `--color-primary` |
| `--button-bg-hover` | Every button: background on hover | `--color-accent` |
| `--button-text-hover` | Every button: text on hover | `--color-background` |
| `--button-border-hover` | Every `.button`: border on hover | `--color-accent` |
| `--button-outline-text` | Outline button: text | `--color-primary` |
| `--button-outline-border` | Outline button: border | `--color-primary` |

Notes:

- An outline button (`button button-outline`) has a see-through background. It turns into the hover colors when hovered.
- `.recipe-card-method-link` is an outline-style button, so it uses the two outline variables. Its background stays on the page background.
- `.recipe-card-source-link` uses the filled colors, but its hover stays on `--color-secondary`.
- Disabled buttons (the Log In, Sign Up and Forgot Password submit buttons) have their own muted look and do not use these variables.

---

## 3. Cards

Used by every card: `.card` (fresh finds, regional discoveries, popular foods, random food and recipe cards), the trivia card, the restaurant notice, the featured food and recipe panels, the recipe method boxes, the island cards, the regional food cards, and the All Foods and Top Foods cards.

### Backgrounds and border

| Variable | Changes | Starts as |
| --- | --- | --- |
| `--card-bg` | Background of a card on a darker band | `--color-background-secondary` |
| `--card-bg-alt` | Background of a card on a lighter band (`.card-on-secondary`, the All Foods and Top Foods cards) | `--color-background` |
| `--card-border` | Border of the cards that have one | `--color-border` |

A card sits on one kind of band, so the two backgrounds are separate. A card on a dark band uses `--card-bg`. A card on a light band uses `--card-bg-alt`, which is darker than the band so the card stands out.

### Text, from most to least important

| Level | Variable | Changes | Starts as |
| --- | --- | --- | --- |
| 1 | `--card-title` | The card title (`h3`, `h4`) | `--color-heading` |
| 2 | `--card-text` | The description | `--color-text` |
| 3 | `--card-label` | Category, location and other small captions | `--color-primary` |
| 4 | `--card-link` | Links such as "Discover Kinunot" | `--color-primary` |

Notes:

- `--card-title` is applied by giving each card its own `--color-heading`. Small tags inside a card that use the heading color (island numbers, recipe categories) follow it.
- The trivia highlight and the restaurant status line use the base palette.
- A border exists only where a card already had one. The featured food and recipe panels, the island cards and the Terms and Privacy contents box have none. The All Foods and Regions food cards have a faint border of their own that does not follow `--card-border`.

---

## 4. Full-width bands

Only the background. A band is a full-width section, such as a hero or a section with its own background.

| Variable | Changes | Starts as |
| --- | --- | --- |
| `--band-bg` | The darker band, and the page background behind everything | `--color-background` |
| `--band-bg-alt` | The lighter band that alternates with it, and every page hero | `--color-background-secondary` |

Bands alternate down each page. **L** is the lighter band (`--band-bg-alt`) and **D** is the darker one (`--band-bg`):

| Page | Bands, top to bottom |
| --- | --- |
| Home | image hero, then the page background (D) |
| All Foods | L D L D |
| Top Foods | L D L D |
| Regions | L D L D L |
| Recipes | L D L D D |
| Random Food | L D L D |
| Near Me | L D L |
| Map | L D D |
| Terms, Privacy | L D |
| Log In, Sign Up, Forgot Password, 404 | L |
| Food Details | D |

The navigation bar and the footer are full width too, but they are not bands. They use the base palette (`--color-background` and `--color-primary`).

To make one band different from the rest, set the background in that section's own rule instead of using the variables.

---

## 5. Not covered by these controls

These still use the base palette directly, so change them through `--color-*` or ask for a control:

- Navigation bar, dropdown and menu button
- Footer
- Form fields and labels
- Hero search box and its suggestion chips
- The "Social" sign-in buttons and the 404 link chips
- The caramel "random food" box on the home page
- Image cards on All Foods (the category cards)
- Tags and number badges
- Focus rings and the skip link

---

## 6. Check the contrast

When text and background colors change, check each pair with a contrast checker. Small text needs at least 4.5:1.

| Text | On |
| --- | --- |
| `--button-text` | `--button-bg` |
| `--button-text-hover` | `--button-bg-hover` |
| `--button-outline-text` | the band behind it, `--band-bg` and `--band-bg-alt` |
| `--card-title`, `--card-text`, `--card-label`, `--card-link` | both `--card-bg` and `--card-bg-alt` |
| `--color-text` and `--color-heading` | both bands |

The current pairs are listed in [global.md](global.md#7-variables).

---

## 7. Adding new things

- A new button uses `class="button"`, or `class="button button-outline"`. A page button written with its own class takes its colors from the `--button-*` variables.
- A new card uses `class="card"`, with `card-on-secondary` when it sits on a lighter band. A page card background uses `var(--card-bg)` or `var(--card-bg-alt)`, and its border uses `var(--card-border)`.
- A new full-width section sets `background-color: var(--band-bg)` or `var(--band-bg-alt)` and carries `full-bleed` in the HTML.
- The base colors are not used directly for these three things.
