# Top Nav Bar

## Structure
- Outer: fixed/absolute, full width, high z-index
- Inner: max-width container, centered, frosted or solid bg, rounded
- Three zones: logo left · nav links center · CTA button right
- Mobile: hide nav + CTA, show hamburger — toggle dropdown with `useState`

## Rules
- No inline `style={{}}` — all colors and spacing via Tailwind
- Nav link active/brand colors belong in the data array, not as conditionals in JSX
- Mobile menu renders below the bar as a card — includes all links + CTA
- Logo wrapped in `<a href="/">`, CTA is a styled `<a>` or `<button>`
- Use `hidden md:flex` / `block md:hidden` to switch between desktop and mobile layouts

## Data Shape
```ts
{ label: string, href: string, className: string }
```
Store per-link color class on the item, apply it directly — no `if/switch` per label.

## Files
| File | Action |
|---|---|
| `components/Navbar.tsx` | Create — implement full desktop + mobile nav |
| `globals.css` | Add shared nav color tokens if used across multiple components |