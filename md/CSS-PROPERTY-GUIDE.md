# CSS Property Order Guide

> **Purpose:** Defines the order in which properties are written inside a CSS rule. Checked against `css/style.css` (127 rules).
> **Read when:** Writing or editing any CSS rule.
> **Related:** [overview.md](overview.md) · [css-writing-guide.md](css-writing-guide.md) · [html-writing-guide.md](html-writing-guide.md) · [global.md](global.md)

---

## 1. The principle

Properties are written from the outside in: **where it sits, how it is laid out, how big it is, what it looks like, how its text looks, how it behaves.** Properties are grouped, and **one blank line separates groups**. There is no blank line inside a group.

The blank-line rule applies to every rule, including short `:hover` and `:focus` rules:

```css
.button:hover {
    border-color: var(--button-border-hover);

    color: var(--button-text-hover);
    background-color: var(--button-bg-hover);
}
```

---

## 2. The six groups

| # | Group | Properties |
| --- | --- | --- |
| 1 | **Position** (where it sits) | `position`, `top`, `right`, `bottom`, `left`, `inset`, `z-index` |
| 2 | **Display and layout** (how children are arranged) | `display`, `flex-direction`, `flex-wrap`, `justify-content`, `align-items`, `gap`, `grid-template-columns`, `grid-template-rows`, `flex`, `order`, `align-self`, `overflow` |
| 3 | **Box model** (size, spacing, border) | `width`, `min-width`, `max-width`, `height`, `min-height`, `max-height`, `aspect-ratio`, `margin`, `padding`, `box-sizing`, `border`, `border-radius` |
| 4 | **Colors and visuals** (how it looks) | `color`, `background`, `background-color`, `background-image`, `background-size`, `background-position`, `box-shadow`, `opacity`, `filter`, `object-fit` |
| 5 | **Typography** (text) | `font-family`, `font-size`, `font-weight`, `font-style`, `line-height`, `letter-spacing`, `text-align`, `text-transform`, `text-decoration`, `white-space` |
| 6 | **Effects and motion** (how it behaves) | `cursor`, `transition`, `transform`, `animation`, `pointer-events` |

Two things come **before group 1**, each followed by a blank line:

| Property | Where | Example |
| --- | --- | --- |
| `--custom-property` (a variable set on this element) | First in the rule | `--gutter-x: var(--page-gutter);` on `.full-bleed` |
| `content` | First in `::before` and `::after` | `content: "";` |

---

## 3. Properties not in the table

Each goes with the group it is closest to.

| Property | Group | Example |
| --- | --- | --- |
| `grid-auto-flow`, `grid-auto-columns`, `grid-area`, `grid-template-areas`, `grid-column`, `place-items`, `columns`, `list-style`, `isolation` | 2 (layout) | `place-items: center;` on `.plate-image` |
| `overflow-x`, `overflow-y`, `-webkit-overflow-scrolling`, `overscroll-behavior-x`, `scroll-snap-type`, `scroll-snap-align`, `scrollbar-width`, `scrollbar-color`, `scroll-behavior`, `scroll-padding-top` | 2 (layout) | the swipeable card rows |
| `margin-block-start`, `margin-block-end`, `margin-inline-start`, `margin-inline-end`, `padding-block`, `padding-inline`, `padding-block-start`, `border-block`, `border-block-start`, `border-block-end`, `border-color` and other long forms | 3 (box model) | `padding-block: 4rem 2rem;` on `.footer` |
| `outline`, `outline-offset` | 3, straight after `border` and `border-radius` | `:focus-visible` |
| `background-repeat`, `text-shadow`, `accent-color`, `backdrop-filter`, `-webkit-backdrop-filter`, `object-position`, `fill`, `stroke`, `stroke-width`, `stroke-linecap`, `stroke-linejoin`, `clip-path` | 4 (visuals) | `fill: none;` on the menu icon |
| `text-decoration-thickness`, `text-underline-offset`, `overflow-wrap`, `text-wrap`, `-webkit-font-smoothing`, `-webkit-text-size-adjust` | 5 (typography) | |
| `will-change`, `break-inside` | 6 (effects) | |

Rules for long forms and prefixes:

- Use the short form (`margin`, `padding`, `border`) when setting all sides. Use a long form only for a single side or axis, and write it with the logical name (`margin-block-start`, `border-block-end`), never `margin-top` or `border-bottom`. A long form sorts like its short form: margin, then padding, then border.
- A property is never set twice in one rule, with one exception: a fallback pair for older browsers, older value first. `.hero` has `min-height` with `100vh`, then again with `100svh`. The open phone menu has `max-height: 100vh;` followed by `max-height: 100dvh;`. When `padding` and `padding-block` are both needed, the broad one comes first.
- A vendor-prefixed property sits right after its standard version, or at the end of its group.
- `border` and `border-color` both belong to the box model group.

---

## 4. Within a group

These orders repeat throughout the stylesheets.

| Group | Order |
| --- | --- |
| Layout | `display`, `flex-direction`, `flex-wrap`, `justify-content`, then `align-items`, `gap`, flex and grid item properties, then `overflow`. `gap` comes before `grid-template-columns`. |
| Box model | width (`width`, `min-width`, `max-width`), height (`height`, `min-height`, `max-height`), `aspect-ratio`, then `margin`, `padding`, `border`, `border-radius`, `outline`. Long forms keep that order: `margin-block-start`, `padding-block-start`, `border-block-start`. |
| Colors | `color`, `text-shadow`, then `background` (`background-color`, `background-image`, `background-repeat`), then `backdrop-filter` and its `-webkit-` twin, then `box-shadow`, `opacity`, then `object-fit` |
| Typography | `font-family`, `font-size`, `font-weight`, `font-style`, `line-height`, `letter-spacing`, `text-align`, `white-space`, `text-wrap`, then `text-decoration` and its offset, then `-webkit-font-smoothing` |
| Effects | `pointer-events`, `cursor`, `transition`, then `transform` |

`transition` is written before `transform`, even though the transform is what it animates.

---

## 5. Examples

A normal rule:

```css
.site-nav-dropdown-menu {
    position: absolute;
    top: calc(100% + 0.85rem);
    left: 0;
    z-index: 11;

    min-width: 11rem;
    margin: 0;
    padding: 0.4rem 1.1rem;
    border: var(--border-line);
    border-radius: var(--radius-md);

    background-color: var(--header-menu-bg);
    box-shadow: var(--shadow-menu);
}
```

A rule that uses every group (an illustration of the order, not copied from the project):

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
```

A pseudo-element starts with `content` (an illustration, not copied from the project):

```css
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

A rule with custom properties:

```css
.full-bleed {
    --gutter-x: var(--page-gutter);

    width: 100vw;
    margin-inline-start: calc(50% - 50vw);
    padding-inline: calc((100vw - 100%) / 2 + var(--gutter-x));
}
```

A rule with long forms, in the order margin, padding, border:

```css
.footer-bottom {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: 0.75rem 2rem;

    margin-block-start: 3rem;
    padding-block-start: 1.5rem;
    border-block-start: var(--border-line);
}
```

A rule with a fallback pair:

```css
.hero {
    display: grid;
    align-items: center;

    min-height: min(calc(100vh - var(--site-nav-height)), var(--hero-max-height));
    min-height: min(calc(100svh - var(--site-nav-height)), var(--hero-max-height));
    padding-block: clamp(3rem, 8vw, 6rem);
}
```

---

## 6. Order of rules in a file

1. Base selector (`.button`)
2. `:hover`
3. `:active`
4. `:focus` or `:focus-visible`
5. Pseudo-elements
6. Modifier (`.button-outline`), then its own states under it
7. Child selectors (`.button svg`)
8. Media queries, at the bottom of the file

Two groups sit under the rules they change instead of at the bottom: the `prefers-reduced-motion` block and the glass header fallbacks. Inside a media query, rules are indented one more level and keep the same property order. File structure, comments and the rest of the formatting are in [css-writing-guide.md](css-writing-guide.md).

---

## 7. Checklist

- [ ] Custom properties, then `content`, come first
- [ ] Groups run position, layout, box model, visuals, typography, effects
- [ ] One blank line between groups, none inside a group
- [ ] `justify-content` before `align-items`, and `gap` before `grid-template-columns`
- [ ] `margin`, `padding`, `border`, `border-radius` in that order, and long forms in the same order
- [ ] Single sides use logical names (`margin-block-start`), not `margin-top`
- [ ] `color` before `background-color`
- [ ] `transition` before `transform`
- [ ] A property appears twice only as a fallback pair, older value first