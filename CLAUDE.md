## About

Sales pitch deck for **askmanu** (always lowercase).
askmanu is an AI copilot that monitors code releases and automatically updates documentation via GitHub issues and pull requests.

- Website: askmanu.co
- Free for open source, commercial plans for teams

## How askmanu works

1. A new release is published on a repo where askmanu is installed
2. askmanu detects the release and opens a GitHub **issue** outlining what docs need updating
3. askmanu creates a **single PR** with incremental doc changes
4. Once that PR is merged, askmanu creates the next PR — either continuing the planned work or finding something new to do
5. Only one PR at a time. Always sequential, always reviewable.

This release → issue → PR → merge → next PR loop is the core workflow.

## Structure

```
index.html              <- main entry point, slide engine + config
slides/
  title.html            <- intro slide with logo and tagline
  team.html             <- 4 placeholder team members
  problem.html          <- why docs are always outdated
  solution.html         <- before/after comparison
  how-it-works.html     <- 3-step install flow
  features.html         <- 4 key features grid
  demo.html             <- GitHub screenshots of the product
  cta.html              <- call to action with install links
```

## Slide system

- Slide order and visibility is controlled by the `SLIDES` array in `index.html`
- Each slide is a standalone HTML fragment in `slides/`
- To customize per customer: comment/uncomment/reorder lines in the `SLIDES` array
- Add new slides by creating a new `.html` file in `slides/` and adding it to the array
- File names have no number prefix — order comes from the array, not filenames

## Tech

- Single `index.html`, no build step required
- Vue 3 (CDN) for slide navigation logic
- Tailwind CSS v3 (CDN) with the **Oatmeal** theme colors (olive palette)
- Fonts: Instrument Serif (display) + Inter (body) via Google Fonts
- Theme source: `../tailwind themes/` (Tailwind Plus Oatmeal kit)
- Navigation: arrow keys, space bar, Home/End, or on-screen arrows
- Requires a local server to run (fetch for slide loading): `python3 -m http.server 8000`

## Conventions

- "askmanu" is always lowercase
- Slides are customer-facing sales material — tone is benefit-driven
- Keep slide files as plain HTML fragments (no `<html>`, `<body>` tags)
- Use Tailwind utility classes matching the olive color palette
