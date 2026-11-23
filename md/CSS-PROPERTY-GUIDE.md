# CSS Property Order Guide

> **Purpose:** Defines the order in which properties are written inside a CSS rule. Checked against all 14 stylesheets in `css/` (about 860 rules).
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
| `--custom-property` (a variable set on this element) | First in the rule | `--gutter-x: 4%;` on `.full-bleed` |
| `content` | First in `::before` and `::after` | `content: "";` |

---

## 3. Properties not in the table

Each goes with the group it is closest to.

| Property | Group | Example |
| --- | --- | --- |
| `grid-auto-flow`, `grid-auto-columns`, `grid-area`, `grid-template-areas`, `columns`, `list-style`, `isolation` | 2 (layout) | `list-style: none;` on `.list-reset` |
| `overflow-x`, `overflow-y`, `-webkit-overflow-scrolling`, `overscroll-behavior-x`, `scroll-snap-type`, `scroll-snap-align`, `scrollbar-width`, `scrollbar-color`, `scroll-behavior`, `scroll-padding-top` | 2 (layout) | the swipeable card rows |
| `margin-top`, `padding-inline`, `padding-block`, `border-right`, `border-color` and other long forms | 3 (box model) | `padding-block: 3rem 5rem;` |
| `outline`, `outline-offset` | 3, straight after `border` and `border-radius` | `.form-group input` |
| `text-shadow`, `accent-color`, `backdrop-filter`, `-webkit-backdrop-filter`, `object-position`, `fill`, `stroke`, `stroke-width`, `stroke-linecap`, `stroke-linejoin`, `clip-path` | 4 (visuals) | `fill: none;` on the menu icon |
| `text-decoration-thickness`, `text-underline-offset`, `overflow-wrap`, `text-wrap`, `-webkit-font-smoothing`, `-webkit-text-size-adjust` | 5 (typography) | |
| `will-change`, `break-inside` | 6 (effects) | |

Rules for long forms and prefixes:

- Use the short form (`margin`, `padding`, `border`) when setting all sides. Use a long form only for a single side or axis.
- A property is never set twice in one rule, with one exception: a fallback pair for older browsers (`max-height: 100vh;` followed by `max-height: 100dvh;` on the open phone menu). When `padding` and `padding-block` are both needed, the broad one comes first.
- A vendor-prefixed property sits right after its standard version, or at the end of its group.
- `border` and `border-color` both belong to the box model group.

---

## 4. Within a group

These orders repeat throughout the stylesheets.

| Group | Order |
| --- | --- |
| Layout | `display`, `flex-direction`, `flex-wrap`, `justify-content`, then `align-items`, `gap`, flex and grid item properties, then `overflow`. `gap` comes before `grid-template-columns`. |
| Box model | width (`width`, `min-width`, `max-width`), height (`height`, `min-height`, `max-height`), `aspect-ratio`, then `margin`, `padding`, `border`, `border-radius`, `outline` |
| Colors | `color`, `text-shadow`, then `background`, then `box-shadow`, `opacity`, then `object-fit` |
| Typography | `font-family`, `font-size`, `font-weight`, `font-style`, `line-height`, `letter-spacing`, `text-align`, `white-space`, `text-wrap`, then `text-decoration` and its offset |
| Effects | `pointer-events`, `cursor`, `transition`, then `transform` |

`transition` is written before `transform`, even though the transform is what it animates.

---

## 5. Examples

A normal rule:

```css
.card {
    --color-heading: var(--card-title);
    --focus-ring: var(--card-link);

    overflow: hidden;
    border: 1px solid var(--card-border);
    border-radius: var(--radius-md);

    color: var(--card-text);
    background-color: var(--card-bg);

    transition: transform 0.2s ease, box-shadow 0.2s ease;
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
    background-color: var(--color-surface);

    font-size: 1rem;
    line-height: 1.6;

    transition: transform 0.2s ease;
}
```

A pseudo-element starts with `content`:

```css
.site-nav-dropdown > summary::after {
    content: "";

    width: 0.35rem;
    height: 0.35rem;
    border-right: 1.5px solid currentColor;
    border-bottom: 1.5px solid currentColor;

    transition: transform 0.2s ease;
    transform: rotate(45deg);
}
```

A rule with custom properties:

```css
.page-hero-split {
    --gutter-x: 8%;

    display: grid;
    align-items: center;
    gap: 4rem;
    grid-template-columns: 1fr 1fr;

    min-height: 580px;
    padding-block: 5rem;
}
```

---

## 6. Order of rules in a file

1. Base selector (`.card`)
2. `:hover`
3. `:focus-visible`
4. Child selectors (`.card-title`)
5. Media queries, at the bottom of the file

Inside a media query, rules are indented one more level and keep the same property order. File structure, comments and the rest of the formatting are in [css-writing-guide.md](css-writing-guide.md).

---

## 7. Checklist

- [ ] Custom properties, then `content`, come first
- [ ] Groups run position, layout, box model, visuals, typography, effects
- [ ] One blank line between groups, none inside a group
- [ ] `justify-content` before `align-items`, and `gap` before `grid-template-columns`
- [ ] `margin`, `padding`, `border`, `border-radius` in that order
- [ ] `color` before `background-color`
- [ ] `transition` before `transform`