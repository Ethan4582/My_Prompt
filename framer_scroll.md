# Scroll Reveal Animation

Elements animate into view on first scroll or page load — from bottom, left, or right — using Framer Motion.

## Install
```bash
npm install framer-motion
```

## Rules
- Use `whileInView` + `initial` on each element — not `animate` (that triggers immediately, not on scroll)
- Set `viewport={{ once: true }}` so animation only plays once per page load
- Define reusable variants outside the component — do not inline animation objects in JSX
- Use `transition` with `ease` and `duration` inside the variant's `visible` state
- Wrap the section in a `motion.section` or `motion.div` — animate children individually for stagger effect
- Direction is controlled by the `hidden` state's `x` or `y` offset:
  - Bottom to top → `y: 40`
  - Left to right → `x: -40`
  - Right to left → `x: 40`

## Variant Pattern
```ts
const fadeInUp = {
  hidden: { opacity: 0, y: 40 },
  visible: { opacity: 1, y: 0, transition: { duration: 0.7, ease: "easeOut" } },
}

const fadeInLeft = {
  hidden: { opacity: 0, x: -40 },
  visible: { opacity: 1, x: 0, transition: { duration: 0.7, ease: "easeOut" } },
}
```

## Usage Pattern
```tsx
<motion.div
  variants={fadeInUp}
  initial="hidden"
  whileInView="visible"
  viewport={{ once: true }}
>
  ...
</motion.div>
```

## Apply To
- Hero headings → `fadeInUp`
- Side images or cards → `fadeInLeft` / `fadeInRight`
- Sections entering on scroll → `fadeInUp` with slight `delay` offset per child