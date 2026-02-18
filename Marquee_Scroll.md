# Marquee Scroll Animation

Infinite left-scrolling image strip with smooth CSS animation.

## Rules
- Move all marquee CSS into `globals.css` — no inline `<style>` tags
- The track must be duplicated inside `.marquee-track` (two identical sets of images) so the loop is seamless
- Add `reverse` class to `.marquee-track` for right-to-left direction
- Use `will-change: transform` for GPU acceleration
- Images: `w-16` on mobile, `w-40` on md+ — never fixed pixel widths
- Gap: `gap-[40px]` mobile, `md:gap-[75px]` desktop

## globals.css
```css
.marquee { width: 100%; overflow: hidden; }
.marquee-track {
  display: inline-flex;
  align-items: center;
  gap: 40px;
  width: max-content;
  will-change: transform;
  animation: marquee 20s linear infinite;
}
.marquee-track.reverse { animation-direction: reverse; }
@media (min-width: 768px) { .marquee-track { gap: 75px; } }
@keyframes marquee {
  0%   { transform: translateX(0%); }
  100% { transform: translateX(-50%); }
}
```

## Component Pattern
```tsx
const images = [...]; // your image array

<div className="marquee">
  <div className="marquee-track">
    {[...images, ...images].map((src, i) => (
      <img key={i} src={src} alt="" className="block w-16 md:w-40 shrink-0" />
    ))}
  </div>
</div>
```

## Files
| File | Action |
|---|---|
| `src/app/globals.css` | Add marquee keyframes and track styles |
| Your component file | Use pattern above, remove inline `<style>` tag |