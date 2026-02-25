# wiremocks

Static HTML/CSS wireframes for client presentations.

## Project Purpose

This project contains mockup screens to show clients before development begins. Each screen is a plain HTML file — no build tools, no package managers, no frameworks. Files open directly in a browser.

## File & Folder Structure

```
wiremocks/
├── CLAUDE.md
├── styles/
│   └── base.css          # shared styles (optional; prefer self-contained files)
├── assets/               # icons, logos, images
└── <screen-name>.html    # one file per screen
```

- Name files after the screen they represent: `login.html`, `dashboard.html`, `settings.html`
- Keep every `.html` file self-contained so it can be opened standalone in a browser
- If styles are shared across many screens, put them in `styles/base.css` and link from each file

## Workflow

1. User provides a screen name and a description of its layout and elements
2. Claude creates a single `.html` file for that screen
3. User opens the file in a browser and gives feedback
4. Iterate

## Design Conventions

- **Color palette:** neutral wireframe aesthetic — whites, light grays (`#f5f5f5`, `#e0e0e0`), dark gray text (`#333`). No brand colors unless specified.
- **Placeholder images:** use `https://placehold.co/<width>x<height>` (e.g., `https://placehold.co/400x200`)
- **Placeholder text:** use short realistic-looking labels, not Lorem Ipsum, so the client understands the intent
- **Interactive elements:** render buttons, inputs, dropdowns, etc. visually — they do not need to work
- **Annotations:** add an HTML comment above each major section to label it (e.g., `<!-- Sidebar navigation -->`)
- **Typography:** system fonts only — `font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`
- **No animations or transitions** unless explicitly requested

## CSS Rules

- Write CSS inline in a `<style>` block inside each `.html` file (keeps files self-contained)
- Only move styles to `styles/base.css` when multiple screens share the same component
- Keep selectors flat and readable — no deep nesting
- Use `rem` for font sizes, `px` for borders and small spacing, `%` or `fr` for layout widths

## Do / Don't

| Do | Don't |
|----|-------|
| Keep HTML semantic (`<nav>`, `<main>`, `<section>`, `<header>`) | Add JavaScript unless asked |
| Comment major layout sections | Use Bootstrap, Tailwind, or any CDN framework unless asked |
| Use placeholder images and realistic labels | Create new files beyond what was asked |
| Make screens openable with a double-click in Finder | Require a local server to view |
