# Silangan

A responsive website for exploring Filipino food: top dishes, regional discoveries, recipes and more.

*Live site:* [silangan-ph.vercel.app](https://silangan-ph.vercel.app/)

<p align="center">
  <img src="assets/images/readme/silangan-hero.png"
       alt="Silangan homepage"
       width="100%">
</p>

## About

Silangan is a multi-page website made for BSIT Web Systems and Technologies (IT Elec 2-B). It celebrates Filipino food and culture by helping visitors discover popular dishes and recipes, learn about regional specialties, and explore a map of the Philippines. This project is for educational purposes only and is not a commercial website.

## Features

- Browse Filipino dishes, regional specialties and recipes
- Top foods, a random food finder and a map of the Philippines
- 15 pages that share one header and footer, with Home, Top Foods, All Foods, Regions and Near Me in the main nav and the rest under More
- Sticky glass header that blurs the page behind it
- Dark emerald and gold theme with top-down dish plates, set by CSS variables so colors can be changed in one place
- Fully responsive layout for desktop, tablet and mobile
- Hamburger menu on small screens
- Accessible by design: skip link, keyboard navigation, visible focus, and support for reduced motion and reduced transparency
- Custom 404 page
- Plain HTML and CSS with no framework and no build step

## Pages

- Home
- Top Foods
- Near Me
- Regions
- All Foods
- Recipes
- Map
- Random Food
- Food Details (coming soon)
- Log In, Sign Up and Forgot Password
- Terms of Service and Privacy Policy
- 404 Not Found

## Known limitations

This is a static site, so some features are placeholders. The pages say so where they appear.

- Dish search on the home page is not available yet
- Near Me shows Albay as a sample area and does not use the visitor's location
- Log In, Sign Up and Forgot Password are unavailable
- Dish cards link to one "Coming soon" page instead of individual dish pages

## Built with

- HTML
- CSS
- JavaScript (one small script for the mobile menu)
- Self-hosted fonts (Cardo for headings and Poppins for body text)
- Deployed on Vercel

## Run it locally

1. Download or clone this repository.
2. Open `index.html` in your browser.

Keep the folder structure (`css`, `js`, `pages`, `assets`) as it is, or the styles and images won't load.

## Project structure

```
index.html      home page
404.html        not-found page
pages/          the other pages
css/            style.css (shared) and one stylesheet per page
js/             nav.js (mobile menu)
assets/         fonts and images
md/             project documentation
```

## Documentation

The `md` folder describes how the site is built, so new pages and styles stay consistent.

| Document | Covers |
| --- | --- |
| [overview.md](md/overview.md) | What the site is, the file layout, and where to start |
| [html-writing-guide.md](md/html-writing-guide.md) | How HTML is written |
| [css-writing-guide.md](md/css-writing-guide.md) | How stylesheets are written |
| [css-property-guide.md](md/css-property-guide.md) | Order of properties inside a CSS rule |
| [global.md](md/global.md) | What the shared stylesheet already sets |
| [colors.md](md/colors.md) | Which variable changes the color of buttons, cards and full-width sections |
| [silangan-style-guide-compact.md](md/silangan-style-guide-compact.md) | The HTML, CSS and property-order guides merged into one short reference |
| [bugs.md](md/bugs.md) | Known issues and fixes |
| [improvement-plan.md](md/improvement-plan.md) | Planned improvements |

## Author

David Gregorio  
BSIT 2-B