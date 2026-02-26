# Scroll Stack Cards — Skill

## What it does
- Sticky title animates in letter by letter on scroll into view
- Cards stack on top of each other as you scroll, each one scaling down slightly
- Built with **Framer Motion** `useScroll` + `useTransform`

---

## Mental Model

```
[mainContainer]  min-h-[200vh]  ← tall enough to scroll through all cards
  [sticky hero]  sticky top-0 h-screen  ← title stays while cards scroll over
  [cards]        absolutely positioned, stack via scale
```

The scroll progress of `mainContainer` drives each card's scale.

---

## 1. Setup — Scroll Progress

```tsx
const mainContainer = useRef(null);
const { scrollYProgress } = useScroll({
  target: mainContainer,
  offset: ["start start", "end end"],
});
```

Pass `scrollYProgress` down to each Card.

---

## 2. Staggered Title Animation

Split word into letters, stagger with Framer Motion variants.

```tsx
const letters = Array.from("Projects");

const container = {
  hidden: { opacity: 0 },
  visible: { opacity: 1, transition: { staggerChildren: 0.2 } },
};

const child = {
  hidden: { opacity: 0, y: 100, rotateZ: 45 },
  visible: { opacity: 1, y: 0,   rotateZ: 0,  transition: { type: "spring" } },
};

// Trigger on scroll into view
const ref = useRef(null);
const isInView = useInView(ref, { once: true });

<motion.div variants={container} initial="hidden" animate={isInView ? "visible" : "hidden"}>
  {letters.map((l, i) => (
    <motion.span key={i} variants={child}>{l}</motion.span>
  ))}
</motion.div>
```

**Outline duplicate** — render the same letters again with `text-transparent` + `WebkitTextStroke` layered on top for a fill/stroke overlap effect.

```tsx
<motion.span style={{ WebkitTextStroke: "1px #fdfdfd" }} className="text-transparent">
  {letter}
</motion.span>
```

---

## 3. Card — Scale on Scroll

Each card gets a `targetScale` based on its index (each one slightly smaller).

```tsx
// In parent
const targetScale = 1 - (totalCards - index) * 0.1;

<Card
  progress={scrollYProgress}   // shared scroll progress
  range={[index * 0.25, 1]}    // when this card starts scaling
  targetScale={targetScale}    // how small it gets
/>
```

```tsx
// Card.tsx
import { useTransform, motion } from "framer-motion";

export function Card({ progress, range, targetScale }) {
  const scale = useTransform(progress, range, [1, targetScale]);

  return (
    <motion.div
      style={{ scale, top: `calc(10vh + ${index * 25}px)` }}
      className="sticky"
    >
      {/* card content */}
    </motion.div>
  );
}
```

---

## 4. Container Height Rule

```
min-h = cards.length * scroll distance per card
```

With 3 cards, `min-h-[200vh]` gives enough runway.  
More cards → increase `min-h` proportionally.

---

## Data Shape

```ts
type Project = {
  title: string;
  src: string;       // image path
  year: number;
  text: string;      // category label
};
```

---

## Key Rules
- `mainContainer` must be `relative` + tall (`min-h-[200vh]+`)
- Sticky hero must be `sticky top-0 h-screen` inside that container
- `range` per card = `[index * 0.25, 1]` — stagger when each starts shrinking
- `targetScale` = `1 - (total - index) * 0.1` — bottom cards shrink most
- Always `slice` to the number of cards you want stacked (e.g. `slice(0, 3)`)

---

## Source Code

### works.tsx
```tsx
"use client";
import React, { useRef } from "react";
import { motion, useInView, useScroll } from "framer-motion";
import { Card } from "./card";
import { PROJECTS } from "@/constant/projects";
import Link from "next/link";

const Work = () => {
  const ref = React.useRef(null);
  const isInView = useInView(ref, { once: true });
  const mainContainer = useRef(null);
  const { scrollYProgress } = useScroll({
    target: mainContainer,
    offset: ["start start", "end end"],
  });
  const letters = Array.from("Projects");
  const container = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: { staggerChildren: 0.2 },
    },
  };
  const child = {
    hidden: {
      opacity: 0,
      x: 0,
      y: 100,
      rotateZ: 45,
      transition: { type: "spring" },
    },
    visible: {
      opacity: 1,
      x: 0,
      y: 0,
      rotateZ: 0,
      transition: { type: "spring" },
    },
  };
  return (
    <>
      <div ref={mainContainer} className="relative min-h-[200vh]">
        <div ref={ref} className="top-[500px] absolute w-[400px]" />
        <div className="top-0 left-0 sticky w-screen h-screen">
          <div className="relative flex flex-col justify-center items-center mx-auto h-screen">
            <motion.div
              variants={container}
              initial="hidden"
              animate={isInView ? "visible" : "hidden"}
              className={`px-2 absolute uppercase overflow-hidden font-bold tracking-tighter flex text-[calc(15vw)] md:text-[calc(13vw)] lg:text-[calc(11vw)]`}
            >
              {letters.map((letter, index) => (
                <motion.span key={index} variants={child} className="text-[#fff]">
                  {letter === " " ? "\u00A0" : letter}
                </motion.span>
              ))}
            </motion.div>
            <motion.div
              className={`px-2 absolute uppercase overflow-hidden font-bold tracking-tighter flex text-[calc(15vw)] md:text-[calc(13vw)] lg:text-[calc(11vw)]`}
            >
              {letters.map((letter, index) => (
                <motion.span
                  style={{ WebkitTextStroke: "1px #fdfdfd" }}
                  key={index}
                  className="text-transparent"
                >
                  {letter === " " ? "\u00A0" : letter}
                </motion.span>
              ))}
            </motion.div>
          </div>
        </div>
        {PROJECTS.slice(0, 3).map((project, index) => {
          const targetScale = 1 - (PROJECTS.slice(0, 3).length - index) * 0.1;
          return (
            <Card
              key={index}
              project={project}
              progress={scrollYProgress}
              range={[index * 0.25, 1]}
              targetScale={targetScale}
            />
          );
        })}
      </div>
      <div className="flex flex-col justify-center items-center gap-4 py-4 text-[#6d6d6d] text-center text-xl md:text-2xl">
        <p className="uppercase">Elevating Digital Experiences with Cutting-Edge Innovation</p>
        <Link
          href={"/project"}
          className="border-white px-4 py-2 border rounded-[20px] text-base text-white"
        >
          See All Works
        </Link>
      </div>
    </>
  );
};
export { Work };
```

### projects.ts
```ts
export type Project = {
  title: string;
  src: string;
  year: number;
  text: string;
};

export const PROJECTS: Project[] = [
  { title: "Matthias Leidinger", src: "/images/first.jpg",  year: 2023, text: "Photography" },
  { title: "Clément Chapillon",  src: "/images/second.jpg", year: 2022, text: "Photography" },
  { title: "Zissou",             src: "/images/third.jpg",  year: 2023, text: "Photography" },
  { title: "Lily Radziemski",    src: "/images/fourth.jpg", year: 2021, text: "Visual Arts" },
  { title: "Nina Wolff",         src: "/images/fifth.jpg",  year: 2020, text: "Graphic Design" },
  { title: "Oscar Lindh",        src: "/images/sixth.jpg",  year: 2023, text: "Digital Illustration" },
];
```