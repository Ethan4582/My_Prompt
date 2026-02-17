# Global Lenis Smooth Scrolling

Add Lenis smooth scrolling globally via a client provider in the App Router.

## Install
```bash
npm install lenis
```

## Files
- `src/components/LenisProvider.jsx` — create this
- `src/app/layout.jsx` — wrap children with the provider

## Code

### `src/components/LenisProvider.jsx`
```jsx
"use client";
import { useEffect, useRef } from "react";
import Lenis from "lenis";

export default function LenisProvider({ children }) {
  const lenisRef = useRef(null);

  useEffect(() => {
    if (!lenisRef.current) {
      lenisRef.current = new Lenis({ lerp: 0.1, smooth: true });

      function raf(time) {
        lenisRef.current.raf(time);
        requestAnimationFrame(raf);
      }
      requestAnimationFrame(raf);
    }

    return () => {
      if (lenisRef.current) {
        lenisRef.current.destroy();
        lenisRef.current = null;
      }
    };
  }, []);

  return <>{children}</>;
}
```

### `src/app/layout.jsx`
```jsx
import LenisProvider from "@/components/LenisProvider";
import "./globals.css";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <LenisProvider>{children}</LenisProvider>
      </body>
    </html>
  );
}
```

## Notes
- `layout.jsx` is a Server Component — Lenis logic must live in a separate `"use client"` file.
- Optionally add `html { scroll-behavior: auto; }` to `globals.css` to prevent native scroll conflicts.