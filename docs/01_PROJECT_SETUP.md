# AURUM — Project Setup Guide
**Brand:** AURUM (Luxury Jewellery E-Commerce)
**Mood:** Editorial · High-End · Timeless — inspired by Prada, Gucci, Bottega Veneta

---

## What We Are Building

A single-page luxury jewellery landing page with 6 sections, built in **React + Vite + TypeScript**.
This is a frontend-only project. No backend. No database. Stock images and videos for now.

---

## Tech Stack (Non-Negotiable)

| Tool | What It Does | Why We Use It |
|------|-------------|---------------|
| React 18 | UI framework | Component-based, easy to split work across team |
| Vite | Build tool | Fastest dev server, handles TypeScript out of the box |
| TypeScript | Language | Catches errors before they happen |
| GSAP + ScrollTrigger | Scroll animations | Industry standard for luxury-tier animation |
| Lenis | Smooth scroll | Removes the "cheap browser scroll" feel |
| Framer Motion | Component transitions | Page-level and element-level motion |
| CSS Modules | Scoped styles | Each component gets its own CSS file, no style conflicts |
| `globals.css` | Global design tokens | ONE file that controls the entire look of the site |

---

## Prerequisites — Install These First

```bash
Node.js v20+       → https://nodejs.org
Git                → https://git-scm.com
VS Code            → https://code.visualstudio.com
```

VS Code Extensions everyone must install:
- ESLint
- Prettier
- CSS Modules (clinyong.vscode-css-modules)

---

## How to Start the Project (Fresh Setup)

Run these commands in your terminal one by one:

```bash
# Step 1: Create the project
npm create vite@latest aurum -- --template react-ts

# Step 2: Enter the folder
cd aurum

# Step 3: Install base dependencies
npm install

# Step 4: Install animation libraries
npm install gsap @gsap/react lenis framer-motion

# Step 5: Install type definitions
npm install --save-dev @types/node

# Step 6: Start the dev server
npm run dev
```

You should see the site running at `http://localhost:5173`

---

## Folder Structure

This is the EXACT structure every team member must follow.
Do not create files outside this structure without team discussion.

```
aurum/
│
├── public/
│   ├── videos/
│   │   └── hero-loop.mp4          ← Hero background video (stock)
│   └── fonts/
│       ├── Canela-Light.woff2     ← Display serif font (see STYLE_GUIDE)
│       └── NHaasGrotesk-55.woff2  ← Body sans font
│
├── src/
│   │
│   ├── styles/
│   │   └── globals.css            ← ⚠️ THE SINGLE SOURCE OF TRUTH FOR ALL STYLES
│   │                                 Every colour, font, spacing lives here.
│   │                                 DO NOT hardcode colours or fonts anywhere else.
│   │
│   ├── components/
│   │   │
│   │   ├── Nav/
│   │   │   ├── Nav.tsx
│   │   │   └── Nav.module.css
│   │   │
│   │   ├── Hero/
│   │   │   ├── Hero.tsx
│   │   │   └── Hero.module.css
│   │   │
│   │   ├── CategoryGrid/
│   │   │   ├── CategoryGrid.tsx
│   │   │   └── CategoryGrid.module.css
│   │   │
│   │   ├── ProductShowcase/
│   │   │   ├── ProductShowcase.tsx
│   │   │   ├── ProductCard.tsx
│   │   │   └── ProductShowcase.module.css
│   │   │
│   │   ├── Carousel/
│   │   │   ├── Carousel.tsx
│   │   │   └── Carousel.module.css
│   │   │
│   │   └── Footer/
│   │       ├── Footer.tsx
│   │       └── Footer.module.css
│   │
│   ├── hooks/
│   │   ├── useScrollAnimation.ts  ← Reusable GSAP scroll hook
│   │   └── useLenis.ts            ← Smooth scroll setup hook
│   │
│   ├── data/
│   │   └── products.ts            ← Mock product data (name, price, image URL)
│   │
│   ├── types/
│   │   └── index.ts               ← TypeScript types shared across components
│   │
│   ├── App.tsx                    ← Assembles all 6 sections in order
│   └── main.tsx                   ← Entry point, imports globals.css HERE
│
├── index.html
├── vite.config.ts
├── tsconfig.json
└── package.json
```

---

## Critical File: `src/main.tsx`

This is the ONLY place where `globals.css` is imported.
Every team member benefits from it automatically. Do not import globals.css anywhere else.

```tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.tsx'
import './styles/globals.css'   // ← Global tokens, fonts, resets. Import ONCE here.

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

---

## Critical File: `src/App.tsx`

All 6 sections are assembled here in order. This is what the final page looks like:

```tsx
import Nav from './components/Nav/Nav'
import Hero from './components/Hero/Hero'
import CategoryGrid from './components/CategoryGrid/CategoryGrid'
import ProductShowcase from './components/ProductShowcase/ProductShowcase'
import Carousel from './components/Carousel/Carousel'
import Footer from './components/Footer/Footer'

function App() {
  return (
    <>
      <Nav />
      <main>
        <Hero />
        <CategoryGrid />
        <ProductShowcase />
        <Carousel />
      </main>
      <Footer />
    </>
  )
}

export default App
```

---

## Critical File: `src/hooks/useLenis.ts`

This enables smooth scrolling for the entire site.
Call this hook once inside `App.tsx`.

```ts
import { useEffect } from 'react'
import Lenis from 'lenis'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

export function useLenis() {
  useEffect(() => {
    const lenis = new Lenis()

    // Connect Lenis to GSAP's ScrollTrigger so they work together
    lenis.on('scroll', ScrollTrigger.update)

    gsap.ticker.add((time) => {
      lenis.raf(time * 1000)
    })

    gsap.ticker.lagSmoothing(0)

    return () => {
      lenis.destroy()
    }
  }, [])
}
```

Then inside `App.tsx`, add this one line:

```tsx
function App() {
  useLenis() // ← Add this
  return ( ... )
}
```

---

## Critical File: `src/hooks/useScrollAnimation.ts`

A reusable hook every team member can use to make elements animate on scroll.

```ts
import { useEffect, useRef } from 'react'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

// Usage: const ref = useScrollAnimation()
// Attach ref to any element and it will fade+slide up when scrolled into view

export function useScrollAnimation() {
  const ref = useRef<HTMLElement>(null)

  useEffect(() => {
    const el = ref.current
    if (!el) return

    gsap.fromTo(
      el,
      { opacity: 0, y: 40 },
      {
        opacity: 1,
        y: 0,
        duration: 1,
        ease: 'power3.out',
        scrollTrigger: {
          trigger: el,
          start: 'top 85%',
          once: true,
        },
      }
    )

    return () => ScrollTrigger.getAll().forEach(t => t.kill())
  }, [])

  return ref
}
```

---

## Critical File: `src/types/index.ts`

Shared TypeScript types. Every team member imports from here.

```ts
export type Product = {
  id: string
  name: string
  price: string          // e.g. "₹ 48,000"
  imageUrl: string
  category: string       // e.g. "Ring", "Necklace", "Bracelet"
}

export type Category = {
  id: string
  label: string
  imageUrl: string
  href: string
}
```

---

## Critical File: `src/data/products.ts`

Mock data. Replace image URLs with real stock image URLs (Unsplash/Pexels).

```ts
import { Product, Category } from '../types'

export const PRODUCTS: Product[] = [
  {
    id: 'p1',
    name: 'Soleil Ring',
    price: '₹ 48,000',
    imageUrl: 'https://images.unsplash.com/photo-1605100804763-247f67b3557e?w=600',
    category: 'Ring',
  },
  {
    id: 'p2',
    name: 'Arc Pendant',
    price: '₹ 62,000',
    imageUrl: 'https://images.unsplash.com/photo-1599643478518-a784e5dc4c8f?w=600',
    category: 'Necklace',
  },
  {
    id: 'p3',
    name: 'Meridian Cuff',
    price: '₹ 35,000',
    imageUrl: 'https://images.unsplash.com/photo-1611591437281-460bfbe1220a?w=600',
    category: 'Bracelet',
  },
  {
    id: 'p4',
    name: 'Veil Earrings',
    price: '₹ 28,500',
    imageUrl: 'https://images.unsplash.com/photo-1535632066927-ab7c9ab60908?w=600',
    category: 'Earrings',
  },
]

export const CATEGORIES: Category[] = [
  { id: 'c1', label: 'Rings',     imageUrl: '...', href: '/rings' },
  { id: 'c2', label: 'Necklaces', imageUrl: '...', href: '/necklaces' },
  { id: 'c3', label: 'Bracelets', imageUrl: '...', href: '/bracelets' },
  { id: 'c4', label: 'Earrings',  imageUrl: '...', href: '/earrings' },
]
```

---

## `vite.config.ts` — Final Version

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
})
```

This allows everyone to import using `@/components/Nav/Nav` instead of `../../components/Nav/Nav`.

---

## Git Workflow — Everyone Must Follow This

```bash
# Before starting any work, pull the latest changes
git pull origin main

# Create your own branch named after your section
git checkout -b section/hero
git checkout -b section/nav
git checkout -b section/category-grid
git checkout -b section/product-showcase
git checkout -b section/carousel-footer

# Commit often with clear messages
git commit -m "feat(hero): add video background and headline animation"

# Push your branch and open a Pull Request into main
git push origin section/hero
```

Never push directly to `main`. Always open a Pull Request.

---

## Stock Assets — Where to Get Images and Videos

**Free Sources:**
- Unsplash → https://unsplash.com (search: "luxury jewellery", "gold ring", "diamond necklace")
- Pexels → https://pexels.com/videos (search: "jewellery slow motion" for hero video)
- Coverr → https://coverr.co (cinematic free videos)

**Image Naming Convention:**
```
hero-bg.jpg
category-rings.jpg
category-necklaces.jpg
product-soleil-ring.jpg
carousel-01.jpg
```

Place all images in `public/images/` and all videos in `public/videos/`.
Reference them as `/images/hero-bg.jpg` (no `src/` prefix needed for public folder).

---

## Final Checklist Before You Start Coding

- [ ] Node.js v20+ installed
- [ ] Project created with `npm create vite@latest`
- [ ] All npm packages installed
- [ ] `globals.css` file exists at `src/styles/globals.css`
- [ ] `globals.css` imported ONLY in `main.tsx`
- [ ] Your component folder created (e.g. `src/components/Hero/`)
- [ ] You have read `02_STYLE_GUIDE.md` fully before writing a single line of CSS
- [ ] You have read `03_TEAM_BRIEF.md` to understand your section assignment