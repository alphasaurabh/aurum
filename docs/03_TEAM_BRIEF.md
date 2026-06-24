# AURUM — Team Brief & Section Assignments

**Brand:** AURUM | Luxury Jewellery E-Commerce
**Total Sections:** 6
**Team Size:** 5 Members
**Stack:** React + Vite + TypeScript + GSAP + Lenis + Framer Motion

---

## Before You Touch Code — Read This First

1. Read `01_PROJECT_SETUP.md` and set up the project on your machine.
2. Read `02_STYLE_GUIDE.md` fully. This is your design bible. Every colour and font decision is in there.
3. Create your branch: `git checkout -b section/your-section-name`
4. Your CSS must only use variables from `globals.css`. No hardcoded colours or sizes.
5. Use the shared hook `useScrollAnimation` for scroll reveal animations.
6. When done, open a Pull Request into `main` and tag the team lead for review.

---

## Section Assignment Overview

| Member | Section | Components to Build | Approx. Complexity |
|--------|---------|---------------------|--------------------|
| Saurabh & Vaibhav | Nav + Hero | `Nav`, `Hero` | High (animation-heavy) |
| Pratham | Category Grid | `CategoryGrid` | Medium-High |
| Rachna | Product Showcase | `ProductShowcase`, `ProductCard` | Medium-High |
| Akshay & Manan | Carousel | `Carousel` | High (drag/swipe interaction) |
| Saurabh & Vaibhav | Footer + Global Setup | `Footer`, `globals.css`, `App.tsx` hooks | Medium |

Saurabh & Vaibhav also handles the one-time global setup (hooks, types, data) before other members start coding.

---

## Saurabh & Vaibhav — Start First (Global Setup Lead)

> **Priority:** Complete this before anyone else starts. Others depend on your files.

**Your job:** Set up the project foundation that everyone else builds on.

**Files you own:**
- `src/styles/globals.css` — Copy from Style Guide exactly
- `src/main.tsx` — Entry point, import globals.css here
- `src/App.tsx` — Assemble all 6 sections
- `src/hooks/useLenis.ts` — Smooth scroll setup
- `src/hooks/useScrollAnimation.ts` — Shared scroll reveal hook
- `src/types/index.ts` — Shared TypeScript types
- `src/data/products.ts` — Mock product data
- `src/components/Footer/Footer.tsx` + `Footer.module.css`

---

### Footer Design Brief

**Inspiration:** Aesop + Prada footer
**Mood:** Dark, quiet, very restrained. The user has scrolled to the bottom of a luxury experience. This section should feel like closing a beautiful book.

**Layout:**
```
┌─────────────────────────────────────────────────────┐
│                                                     │
│  AURUM                                              │
│  Fine Jewellery Since 2024                          │
│                                                     │
│  ─────────────────── (gold thin line) ──────────── │
│                                                     │
│  Shop          About          Care           Legal  │
│  Rings         Our Story      Jewellery Care Privacy│
│  Necklaces     Craftsmen      Sizing Guide  Terms   │
│  Bracelets     Sustainability Returns       Cookies  │
│  Earrings      Contact        FAQ                   │
│                                                     │
│  ─────────────────── (gold thin line) ──────────── │
│                                                     │
│    [Email address____________________] → SUBSCRIBE  │
│                                                     │
│  © 2024 AURUM. All Rights Reserved.                 │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Exact Styling Rules:**
- Background: `var(--bg-dark)` — dark, not pitch black
- All text: `var(--text-on-dark)`
- Brand name "AURUM" at top: `var(--font-display)`, `var(--text-display)`, font-weight 300
- Column headings (Shop, About, Care, Legal): `var(--text-micro)`, letter-spacing `var(--tracking-caps)`, uppercase, colour `var(--text-accent)` (gold)
- Link items: `var(--text-small)`, colour `var(--text-secondary)`, hover changes to `var(--text-on-dark)` with transition
- The two horizontal gold lines: `1px solid var(--text-accent)`, full width
- Newsletter section: single `<input type="email">` + text button "SUBSCRIBE"
  - Input: no border except bottom, background transparent, colour `var(--text-on-dark)`
  - Button: `var(--text-micro)` size, gold colour, uppercase
- Copyright line: `var(--text-micro)`, colour `var(--text-secondary)`
- Section padding: `var(--space-section)` top and bottom

**Animation:** Fade in on scroll using `useScrollAnimation` hook. Nothing complex.

---

---

## Saurabh & Vaibhav — Navigation + Hero

**Branch:** `git checkout -b section/nav-hero`

**Files you own:**
- `src/components/Nav/Nav.tsx` + `Nav.module.css`
- `src/components/Hero/Hero.tsx` + `Hero.module.css`

---

### Nav Design Brief

**Inspiration:** Prada + Apple sticky navigation
**Mood:** Invisible until needed. Ultra-slim. Does not compete with the content below it.

**Layout:**
```
┌─────────────────────────────────────────────────────┐
│  AURUM         Rings  Necklaces  Bracelets  Earrings  [Search] [Bag 0] │
└─────────────────────────────────────────────────────┘
```

**Exact Styling Rules:**
- Position: `fixed`, top 0, full width, `z-index: var(--z-nav)`
- Height: `64px` on desktop, `52px` on mobile
- Background: starts fully transparent over the hero video. When user scrolls down 80px, background transitions to `rgba(250, 248, 245, 0.85)` with `backdrop-filter: blur(12px)` (glassmorphism effect)
- Logo "AURUM": `var(--font-display)`, `22px`, font-weight 300, letter-spacing 0.12em (wide tracking makes a short word feel like a logo)
- Nav links: `var(--font-body)`, `var(--text-small)`, uppercase, letter-spacing `var(--tracking-caps)`. On hover: gold underline `2px solid var(--text-accent)` slides in from left using `scaleX` transform
- Bag icon + Search icon: `20px` SVG icons, colour `var(--text-primary)`
- On dark hero: text is `var(--text-on-dark)`. After scroll threshold: text becomes `var(--text-primary)`. Transition using `--duration-base` and `--ease-in-out`

**Scroll Behaviour (JavaScript):**
```ts
// Inside Nav.tsx, use this useEffect
useEffect(() => {
  const handleScroll = () => {
    setScrolled(window.scrollY > 80)
  }
  window.addEventListener('scroll', handleScroll)
  return () => window.removeEventListener('scroll', handleScroll)
}, [])
```
When `scrolled` is true, apply a CSS class that adds the blur background and changes text colour.

**Hide/Show on Scroll:**
Nav hides when scrolling DOWN (past 300px). Slides back into view instantly when scrolling UP even 1px.
```ts
// Track scroll direction
let lastScrollY = 0
const handleScroll = () => {
  const currentScrollY = window.scrollY
  setHidden(currentScrollY > lastScrollY && currentScrollY > 300)
  lastScrollY = currentScrollY
}
```
Use CSS `transform: translateY(-100%)` with transition `var(--duration-base)` to hide/show.

**Mega Menu (Hover):**
When user hovers a nav link like "Rings", a full-width panel drops down showing sub-categories with small images. This can be built in Phase 2 — for now, just build the basic nav correctly.

---

### Hero Design Brief

**Inspiration:** Gucci + Burberry hero
**Mood:** Cinematic. Silent. The brand speaks in one sentence and one image.

**Layout:**
```
┌─────────────────────────────────────────────────────┐
│                                                     │
│         [Full screen video playing silently]        │
│         [Dark overlay: rgba(0,0,0, 0.35)]           │
│                                                     │
│                   NEW COLLECTION                    │   ← eyebrow label
│                                                     │
│          Crafted to Last a Lifetime.                │   ← main headline
│                                                     │
│                 [ EXPLORE NOW ]                     │   ← ghost button
│                                                     │
│                      ↓                              │   ← scroll indicator
└─────────────────────────────────────────────────────┘
```

**Exact Styling Rules:**
- Section height: `100vh`, `100svh` (safe viewport height for mobile browsers)
- Background: `<video>` element, `autoPlay`, `loop`, `muted`, `playsInline`, `object-fit: cover`, absolute position covering full section
- Video overlay: `<div>` absolutely positioned, `background: rgba(28, 26, 24, 0.40)`, same size as video, `z-index: var(--z-overlay)`
- All text content is centered (`text-align: center`, `display: flex; flex-direction: column; align-items: center; justify-content: center`)
- Eyebrow label "NEW COLLECTION": class `eyebrow` from globals, colour `var(--text-on-dark)`
- Main headline "Crafted to Last a Lifetime.": `var(--font-display)`, `var(--text-hero)`, font-weight 300, colour `var(--text-on-dark)`, letter-spacing `var(--tracking-display)`, line-height 0.95, `max-width: 900px`
- Ghost button "EXPLORE NOW": see button styles in Style Guide (btn-ghost)
- Gap between eyebrow and headline: `var(--space-3)` (24px)
- Gap between headline and button: `var(--space-5)` (48px)

**Scroll Indicator:**
A small animated down arrow or line that bounces gently. CSS keyframe animation:
```css
@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50%       { transform: translateY(8px); }
}
.scrollIndicator {
  animation: bounce 1.8s var(--ease-in-out) infinite;
  color: var(--text-on-dark);
  opacity: 0.6;
}
```
Respect `prefers-reduced-motion`:
```css
@media (prefers-reduced-motion: reduce) {
  .scrollIndicator { animation: none; }
}
```

**Page Load Animation (GSAP):**
On page load, the content animates in. Use `gsap.timeline()`:
```ts
// Inside Hero.tsx useEffect
const tl = gsap.timeline({ delay: 0.3 })
tl.fromTo(eyebrowRef.current, 
  { opacity: 0, y: 20 }, 
  { opacity: 1, y: 0, duration: 0.8, ease: 'power3.out' })
  .fromTo(headlineRef.current,
  { opacity: 0, y: 40 },
  { opacity: 1, y: 0, duration: 1, ease: 'power3.out' }, '-=0.4')
  .fromTo(buttonRef.current,
  { opacity: 0 },
  { opacity: 1, duration: 0.6 }, '-=0.3')
```

**Hero video for development:**
Use this free Pexels jewellery video (or similar):
`https://www.pexels.com/video/close-up-of-diamond-ring-5026580/`
Download and place at `public/videos/hero-loop.mp4`
Reference as `<video src="/videos/hero-loop.mp4">`

---

---

## Pratham — Category Grid

**Branch:** `git checkout -b section/category-grid`

**Files you own:**
- `src/components/CategoryGrid/CategoryGrid.tsx` + `CategoryGrid.module.css`

**Data import:**
```ts
import { CATEGORIES } from '@/data/products'
```

---

### Category Grid Design Brief

**Inspiration:** Maison Margiela + Bottega Veneta
**Mood:** Curated. Asymmetric. Like a gallery, not a shop.

**Layout — Desktop (4 categories: Rings, Necklaces, Bracelets, Earrings):**
```
┌───────────────────────────────────────────────────────────────┐
│     NEW COLLECTION                                            │   ← eyebrow
│     Discover Our World                                        │   ← section headline
│                                                               │
│  ┌──────────────────────┐  ┌──────┐  ┌──────────────────────┐│
│  │                      │  │      │  │                      ││
│  │        RINGS         │  │      │  │      BRACELETS       ││
│  │     (tall image)     │  │ NECK │  │     (tall image)     ││
│  │                      │  │ LACE │  │                      ││
│  │                      │  │      │  └──────────────────────┘│
│  └──────────────────────┘  │      │  ┌──────────────────────┐│
│                             │      │  │                      ││
│                             │      │  │      EARRINGS        ││
│                             │      │  │     (short image)    ││
│                             └──────┘  └──────────────────────┘│
└───────────────────────────────────────────────────────────────┘
```

This is an intentionally asymmetric broken grid. It is not a regular 4-column grid. Achieve it using CSS Grid with `grid-template-areas`.

**CSS Grid Structure:**
```css
.grid {
  display: grid;
  grid-template-columns: 2fr 1fr 2fr;   /* Left wide, middle narrow, right wide */
  grid-template-rows: auto auto;
  grid-template-areas:
    "rings    necklaces bracelets"
    "rings    necklaces earrings";
  gap: var(--grid-gap);
}

.rings     { grid-area: rings;     }
.necklaces { grid-area: necklaces; }
.bracelets { grid-area: bracelets; }
.earrings  { grid-area: earrings;  }
```

**Each category cell styling:**
- Background: `var(--bg-card)` — warm cream
- `overflow: hidden` — so zoom effect stays inside the cell
- `position: relative` — for the label overlay
- Minimum height: `400px` for short cells, `500px` for tall cells
- `border-radius: 0` — no rounded corners. Sharp edges feel more editorial.

**The image inside each cell:**
```css
.categoryImage {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform var(--duration-slow) var(--ease-out);
}

.categoryCell:hover .categoryImage {
  transform: scale(1.04);  /* The luxury zoom. Exactly 1.04. */
}
```

**The category label (overlaid on image):**
- Position: absolute, bottom left, inside the cell
- Background: `rgba(28, 26, 24, 0.50)` — semi-transparent dark strip
- Padding: `var(--space-3) var(--space-4)`
- Category name: `var(--font-display)`, `var(--text-heading)`, font-weight 300, colour `var(--text-on-dark)`
- "EXPLORE →" text below the name: `var(--font-body)`, `var(--text-micro)`, letter-spacing `var(--tracking-caps)`, uppercase, colour `var(--text-on-dark)`, opacity 0.7

**Section header (above the grid):**
- Eyebrow: "THE COLLECTION" — use `.eyebrow` class from globals
- Headline: "Discover Our World" — use `.headline` class from globals, `var(--text-display)` size
- Alignment: left-aligned (more editorial than centred)
- Margin below header before grid: `var(--space-6)` (64px)

**Animation on scroll:**
Each category cell staggers in from below. Use this in `CategoryGrid.tsx`:
```ts
useEffect(() => {
  gsap.fromTo('.categoryCell', 
    { opacity: 0, y: 60 },
    {
      opacity: 1, y: 0,
      duration: 1,
      ease: 'power3.out',
      stagger: 0.15,  // each cell starts 150ms after the previous
      scrollTrigger: {
        trigger: '.categoryGrid',
        start: 'top 75%',
        once: true,
      }
    }
  )
}, [])
```

**Mobile:** Stack all 4 cells into a single column. Each cell is `280px` tall.

**Stock images to use (search Unsplash):**
- Rings: search "gold ring minimalist white background"
- Necklaces: search "diamond pendant necklace editorial"
- Bracelets: search "gold bracelet jewellery"
- Earrings: search "drop earrings gold luxury"

---

---

## Rachna — Product Showcase

**Branch:** `git checkout -b section/product-showcase`

**Files you own:**
- `src/components/ProductShowcase/ProductShowcase.tsx` + `ProductShowcase.module.css`
- `src/components/ProductShowcase/ProductCard.tsx`

**Data import:**
```ts
import { PRODUCTS } from '@/data/products'
import type { Product } from '@/types'
```

---

### Product Showcase Design Brief

**Inspiration:** Tiffany & Co. + Aesop product grid
**Mood:** Immaculately clean. Each product has room to breathe. The card is not a card — it is a frame.

**Section Layout:**
```
┌──────────────────────────────────────────────────────┐
│                                                      │
│   OUR PIECES               [ Rings  Necklaces  All ]│   ← filter tabs
│   Fine Jewellery                                     │
│                                                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────┐│
│  │          │  │          │  │          │  │      ││
│  │  [image] │  │  [image] │  │  [image] │  │[img] ││
│  │          │  │          │  │          │  │      ││
│  │──────────│  │──────────│  │──────────│  │──────││
│  │Soleil Rng│  │Arc Pendnt│  │Meridian  │  │Veil  ││
│  │₹ 48,000  │  │₹ 62,000  │  │₹ 35,000  │  │28,500││
│  └──────────┘  └──────────┘  └──────────┘  └──────┘│
│                                                      │
└──────────────────────────────────────────────────────┘
```

**Product Grid CSS:**
```css
.grid {
  display: grid;
  grid-template-columns: var(--product-grid-columns);  /* From globals: auto 1-4 columns */
  gap: var(--grid-gap);
}
```

**Product Card — Default State (no hover):**
- Background: `var(--bg-card)` — warm cream (#f2ede6)
- Internal padding: `var(--space-card)`
- No border, no shadow. The card boundary is implied by the colour change.
- Image container: aspect ratio `3/4` (portrait — jewellery looks better tall)
  ```css
  .imageContainer {
    aspect-ratio: 3 / 4;
    overflow: hidden;
    background: var(--color-cream); /* slightly darker for image bg */
    margin-bottom: var(--space-3);
  }
  ```
- Product name below image: `var(--font-display)`, `18px`, font-weight 300, colour `var(--text-primary)`
- Price below name: `var(--font-body)`, `var(--text-small)`, colour `var(--text-accent)` (gold), `margin-top: var(--space-1)`
- Category label above name: `var(--text-micro)`, uppercase, letter-spacing `var(--tracking-caps)`, colour `var(--text-secondary)`, `margin-bottom: var(--space-1)`

**Product Card — Hover State:**
On hover, two things happen:
1. Image zooms: `transform: scale(1.04)` on the image (NOT the card)
2. "QUICK ADD" button slides up from the bottom of the card

```css
/* The card itself doesn't move. Only the image inside zooms. */
.productImage {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform var(--duration-slow) var(--ease-out);
}
.productCard:hover .productImage {
  transform: scale(1.04);
}

/* Quick Add button — hidden by default, slides in on hover */
.quickAdd {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: var(--space-2) var(--space-3);
  background: var(--color-ink);
  color: var(--color-parchment);
  font-family: var(--font-body);
  font-size: var(--text-micro);
  letter-spacing: var(--tracking-caps);
  text-transform: uppercase;
  text-align: center;
  transform: translateY(100%);   /* Hidden below the card */
  transition: transform var(--duration-base) var(--ease-luxury);
}
.productCard:hover .quickAdd {
  transform: translateY(0);      /* Slides into view */
}

/* The image container needs position relative for the button overlay */
.imageContainer {
  position: relative;
  overflow: hidden;  /* So button doesn't appear outside */
}
```

**Filter Tabs (Rings / Necklaces / Bracelets / All):**
Simple state management in React. `useState` tracks the active filter.
When filter changes, the product grid re-renders with filtered products.
Active tab gets a gold bottom border: `border-bottom: 2px solid var(--text-accent)`.

```tsx
const [activeFilter, setActiveFilter] = useState('All')
const filtered = activeFilter === 'All' 
  ? PRODUCTS 
  : PRODUCTS.filter(p => p.category === activeFilter)
```

Tab styling: no background, just text + underline. Tabs must feel like editorial navigation, not website tabs.

**Scroll Animation:**
Cards stagger in when section comes into view. Use `useScrollAnimation` hook on the grid container, or the GSAP stagger approach from Pratham as reference.

**Mobile:**
- 1 column on mobile (`< 640px`)
- 2 columns on tablet (`640px - 1024px`)
- 3-4 columns on desktop

---

---

## Akshay & Manan — Horizontal Narrative Carousel

**Branch:** `git checkout -b section/carousel`

**Files you own:**
- `src/components/Carousel/Carousel.tsx` + `Carousel.module.css`

---

### Carousel Design Brief

**Inspiration:** BMW + Porsche horizontal scroll
**Mood:** This is the emotional centrepiece. Full-bleed imagery. It should feel like flipping through a magazine campaign. The user should want to linger here.

**Layout:**
```
┌──────────────────────────────────────────────────────────────┐
│  ← DRAG TO EXPLORE                                          │   ← label
│                                                              │
│ ┌────────────────────┐ ┌───────────────────┐ ┌────────────┐ │
│ │                    │ │                   │ │            │ │
│ │   [Full slide 1]   │ │  [Full slide 2]   │ │ [Slide 3]  │ │
│ │   Campaign image   │ │  Campaign image   │ │            │ │
│ │                    │ │                   │ │            │ │
│ │  SOLEIL COLLECTION │ │  THE ARC SERIES   │ │            │ │
│ │  Fine Jewellery    │ │  New Arrivals     │ │            │ │
│ └────────────────────┘ └───────────────────┘ └────────────┘ │
│                                                              │
│   ● ○ ○ ○                              [1/4]                │   ← dots + counter
└──────────────────────────────────────────────────────────────┘
```

**Container:**
```css
.carouselSection {
  padding: var(--space-section) 0;
  overflow: hidden;  /* Critical: don't let slides bleed out of section */
  background: var(--bg-primary);
}

.track {
  display: flex;
  gap: 24px;
  cursor: grab;
  user-select: none;  /* Prevents text selection while dragging */
  overflow-x: scroll;
  scroll-snap-type: x mandatory;
  scroll-behavior: smooth;
  -webkit-overflow-scrolling: touch;  /* Smooth scroll on iOS */

  /* Hide scrollbar visually but keep functionality */
  scrollbar-width: none;
}
.track::-webkit-scrollbar { display: none; }
```

**Each Slide:**
```css
.slide {
  flex-shrink: 0;
  width: clamp(300px, 70vw, 900px);  /* Wide on desktop, nearly full on mobile */
  aspect-ratio: 16 / 9;
  scroll-snap-align: start;
  position: relative;
  overflow: hidden;
}

.slideImage {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform var(--duration-crawl) var(--ease-out);
}
.slide:hover .slideImage {
  transform: scale(1.03);
}

/* Dark overlay on each slide for text legibility */
.slideOverlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: var(--space-5) var(--space-5) var(--space-4);
  background: linear-gradient(to top, rgba(15,15,15,0.75), transparent);
}

.slideName {
  font-family: var(--font-display);
  font-size: var(--text-heading);
  font-weight: 300;
  color: var(--text-on-dark);
  letter-spacing: var(--tracking-display);
  margin-bottom: var(--space-1);
}

.slideSubtitle {
  font-family: var(--font-body);
  font-size: var(--text-micro);
  letter-spacing: var(--tracking-caps);
  text-transform: uppercase;
  color: var(--text-on-dark);
  opacity: 0.7;
}
```

**Drag to Scroll — Desktop:**
Implement click-and-drag scrolling so users can drag the slides with their mouse on desktop:

```tsx
// Inside Carousel.tsx
const trackRef = useRef<HTMLDivElement>(null)
const isDown = useRef(false)
const startX = useRef(0)
const scrollLeft = useRef(0)

const handleMouseDown = (e: React.MouseEvent) => {
  isDown.current = true
  startX.current = e.pageX - trackRef.current!.offsetLeft
  scrollLeft.current = trackRef.current!.scrollLeft
  trackRef.current!.style.cursor = 'grabbing'
}

const handleMouseMove = (e: React.MouseEvent) => {
  if (!isDown.current) return
  e.preventDefault()
  const x = e.pageX - trackRef.current!.offsetLeft
  const walk = (x - startX.current) * 2  // ×2 = faster drag feel
  trackRef.current!.scrollLeft = scrollLeft.current - walk
}

const handleMouseUp = () => {
  isDown.current = false
  trackRef.current!.style.cursor = 'grab'
}
```

Attach these to the `<div ref={trackRef}>` element:
```tsx
onMouseDown={handleMouseDown}
onMouseMove={handleMouseMove}
onMouseUp={handleMouseUp}
onMouseLeave={handleMouseUp}
```

**Progress Dots:**
Track current visible slide using `IntersectionObserver` or `onScroll` event.
Active dot: `background: var(--text-primary)`, width `24px`
Inactive dot: `background: var(--color-mist)`, width `8px`
Transition: `width var(--duration-base) var(--ease-luxury)` — the active dot grows, not changes colour only.

**Section header (above the track):**
- Left side: Eyebrow "THE AURUM EDIT" + headline "A Story in Gold"
- Right side: "← DRAG TO EXPLORE" label in `var(--text-micro)`, muted colour
- These sit in a `display: flex; justify-content: space-between` row above the track

**Slide images to use (search Pexels or Unsplash):**
- "luxury jewellery editorial dark background"
- "gold necklace model editorial"
- "diamond ring close up cinematic"
- "jewellery campaign shoot"

**Mobile:** The slide width becomes `85vw` automatically via the clamp. Touch swipe works natively via `scroll-snap-type`. No extra JS needed for mobile.

---

---

## Shared Rules — Read Before Each PR

Before you open your Pull Request, confirm:

- [ ] Every colour in my CSS uses a variable from `globals.css`
- [ ] Every font reference uses `var(--font-display)` or `var(--font-body)`
- [ ] Every spacing value uses a variable (e.g. `var(--space-section)`)
- [ ] Every animation uses `var(--duration-*)` and `var(--ease-*)`
- [ ] Image hover zoom is exactly `scale(1.04)`
- [ ] My section has `padding: var(--space-section) 0` top and bottom
- [ ] I have not imported `globals.css` in my component (it's already loaded via `main.tsx`)
- [ ] I have tested on mobile screen width (375px)
- [ ] No TypeScript errors (`npm run build` passes without errors)

---

## Communication

- Create issues in GitHub for bugs or questions tagged with your section name
- Post progress screenshots in the team chat
- If you need to change anything in `globals.css`, discuss with the full team first — a change there affects every section

---

## Final Page Rendering Order (for reference)

```
<Nav />              ← Saurabh & Vaibhav
<Hero />             ← Saurabh & Vaibhav
<CategoryGrid />     ← Pratham
<ProductShowcase />  ← Rachna
<Carousel />         ← Akshay & Manan
<Footer />           ← Saurabh & Vaibhav
```