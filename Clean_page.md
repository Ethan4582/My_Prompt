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



Imp - do not Change the layout, color or the styling or text color or font. it should look indetical 



Remove all style={{}} — convert to Tailwind classes
Define fonts and layout defaults once in globals.css, not per element

Clean the component, remove the unnecessary code and if there is a styling move, rewrite using Tailwind but do not change anything layout or color, font, everything. You just need to clean the component, remove unnecessary RGB color without changing the color of styling it should look very identical.

 Also break the component into smaller components following proper dev principle. If there is data for the cards, make it into an array to ensure that we follow proper dev principle and better code readability.