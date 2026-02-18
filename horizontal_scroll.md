# Infinite Marquee Scroll — Skill

## Core idea
Duplicate the logo list, animate the track by `-50%`, so it loops back to the start seamlessly.

---

## Structure

```
[outer wrapper] overflow-hidden, relative
  [left fade overlay]   absolute, gradient → transparent
  [right fade overlay]  absolute, gradient → transparent
  [track]               flex, animation: marquee
    [...logos x2]
```

---

## The animation

```css
@keyframes marquee {
  0%   { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}
```

Apply to the track: `animation: marquee 18s linear infinite`

---

## The fades

Match the gradient color to your **background color**.

```jsx
/* Left */
background: linear-gradient(to right, #BG_COLOR, transparent)

/* Right */
background: linear-gradient(to left, #BG_COLOR, transparent)
```

Both overlays: `position: absolute`, `z-index: 2`, `width: 120px`, `pointer-events: none`

---

## Logo format exceptions

| Case | How to handle |
|---|---|
| SVG icon | Inline `<svg>` inside the logo component |
| Image file | `<img src="..." />` inside the logo component |
| CSS shapes | `<div>` with border/transform tricks |

The marquee doesn't care — each logo is just a `<div className="shrink-0">` wrapper.

---

## Key rules
- Always duplicate the list: `[...logos, ...logos]` → render `[...list, ...list]`
- `-50%` = one full set width, so the loop is invisible
- Outer wrapper needs `overflow: hidden`
- Speed: lower `s` = faster