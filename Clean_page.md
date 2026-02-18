# Clean page.tsx

## Rules
- Remove all `style={{}}` — convert to Tailwind classes
- Define fonts and layout defaults once in `globals.css`, not per element
- Replace arbitrary values (`text-[16px]`, `gap-[0px]`) with Tailwind tokens where possible
- Move repeated class combos to `globals.css` using `@apply`
- Delete no-op classes (`gap-[0px]`, `leading-[normal]`, `scale(1)`)
- Visual output must stay identical

## Files
| File | Action |
|---|---|
| `src/app/page.tsx` | Remove `style={{}}`, clean redundant classes |
| `src/app/globals.css` | Add shared font, layout, and `@apply` patterns |