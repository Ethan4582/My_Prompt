# Dynamic Works Pages

Convert static `/works` routes to dynamic using a shared data file.

## Principles
- **Single source of truth** — all project data lives in `src/data/works.js`. Both routes import from it. Never duplicate data.
- **Shared helper** — export a `getWorkById(id)` function from the same file. The `[id]` route uses it to find the current project.
- **Static generation** — use `generateStaticParams()` in `[id]/page.jsx` to pre-render all project routes at build time.
- **Tailwind only** — no inline styles. Match your existing spacing and typography tokens exactly.
- **Grid layout** — `/works/page.jsx` must render exactly 6 projects in a `grid-cols-3 grid-rows-2` layout. Do not alter structure.
- **notFound()** — if a project ID doesn't exist, call Next.js `notFound()` immediately.

## Files to Create / Update
| File | Action |
|---|---|
| `src/data/works.js` | Create — add 6 project objects with: `id, title, category, year, cover, description, tags` |
| `src/app/works/page.jsx` | Update — import `works`, map over it, render the grid |
| `src/app/works/[id]/page.jsx` | Update — import `getWorkById`, render single project by `params.id` |