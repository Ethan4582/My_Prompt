# Revealer Animation — Skill

## How it works
A full-screen `div` covers the page, then GSAP scales it to `0` — revealing the content underneath.  
Direction is controlled by `transform-origin` + which scale axis you animate.

---

## Default (used in this project)
Scales **upward** — origin at top, `scaleY: 0`

```tsx
// useReveal.tsx
CustomEase.create("hop", "0.9, 0, 0.1, 1");

gsap.to(".revealer", {
  scaleY: 0,
  duration: 1.25,
  delay: 1,
  ease: "hop",
});
```

```tsx
// Overlay div
<div className="revealer fixed inset-0 bg-black z-[9999] origin-top pointer-events-none" />
```

---

## Direction Variants

| Direction | `origin-*` class | animate |
|---|---|---|
| Up *(default)* | `origin-top` | `scaleY: 0` |
| Down | `origin-bottom` | `scaleY: 0` |
| Left | `origin-left` | `scaleX: 0` |
| Right | `origin-right` | `scaleX: 0` |

---

## Variant 1 — Single Page Reveal

Trigger on mount of one page component only.

```tsx
// useReveal.tsx — accept direction
type Direction = "up" | "down" | "left" | "right";

const config: Record<Direction, { prop: string; origin: string }> = {
  up:    { prop: "scaleY", origin: "top" },
  down:  { prop: "scaleY", origin: "bottom" },
  left:  { prop: "scaleX", origin: "left" },
  right: { prop: "scaleX", origin: "right" },
};

export function useRevealer(direction: Direction = "up") {
  const { prop, origin } = config[direction];
  useGSAP(() => {
    gsap.to(".revealer", {
      [prop]: 0,
      duration: 1.25,
      delay: 1,
      ease: "hop",
    });
  }, {});
}
```

```tsx
// Page1.tsx
export default function Page1() {
  useRevealer("up"); // only this page gets the reveal

  return (
    <div className="relative">
      <div
        className="revealer absolute inset-0 bg-black z-[9999] pointer-events-none"
        style={{ transformOrigin: "top" }}
      />
      {/* page content */}
    </div>
  );
}
```

> Use `absolute` (not `fixed`) so it's scoped to that page only.

---

## Variant 2 — Entire Project Reveal (current pattern)

One overlay in the root layout, covers everything on load.

```tsx
// home.tsx / layout.tsx
export default function Home() {
  useRevealer("up"); // "up" | "down" | "left" | "right"

  return (
    <main className="relative">
      {/* Full-screen overlay — fixed so it covers all pages */}
      <div
        className="revealer fixed inset-0 bg-black z-[9999] pointer-events-none"
        style={{ transformOrigin: "top" }} // change per direction
      />
      {/* rest of the app */}
    </main>
  );
}
```

> `fixed` + placed at root = entire project is revealed once on load.

---

## Key Rules
- `fixed` → whole project reveal
- `absolute` → single page/section reveal
- `origin-*` controls which edge it collapses toward
- `scaleY` for vertical, `scaleX` for horizontal
- Always `pointer-events-none` so overlay doesn't block interaction