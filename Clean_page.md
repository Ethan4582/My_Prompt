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



Prompt -2 

 We need to clean all this component for all the textual data or the card data use global.ts file to put all the textual data in that and allow all this component to use global.ts for the textual data.


Clean this component, remove the unnecessary classes name but do not change it. We have to implement exactly as it did but move it to either global.css or implement using tailwind.css. Use global.data to move the car data or other textual data into that to implement better code readability.



prompt -3 


Clean Component Code Readability
Rules
Remove all style={{}} — convert to Tailwind, keep inline only for truly dynamic values
Extract repeated class combos into named globals.css classes using @apply
Set shared fonts and colors once in globals.css — never per element
Consolidate symmetric padding shorthand: pt-x pr-y pb-x pl-y → py-x px-y
Remove wrapper divs that have no styles and only wrap a single child
Visual output must stay pixel-identical — no color, spacing, or layout changes
