# AURUM — Style Guide
## The Single Source of Truth for All Visual Decisions

**This file controls everything.**
If you are deciding a colour, a font size, a spacing value, an animation speed — the answer is in this file.
Never hardcode a value. Always use a CSS variable from `globals.css`.

---

## The Design Philosophy in One Sentence

> "Luxury is not decoration. It is the absence of noise."

Every decision we make follows this rule:
- If something can be removed without losing meaning — remove it.
- If a colour is not needed — use white or black.
- If an animation doesn't serve the content — cut it.

We are inspired by **Prada, Gucci, Bottega Veneta, and Aesop**.
Their websites feel expensive because they are disciplined, not because they are complex.

---

## The Global CSS File: `src/styles/globals.css`

Copy this file exactly. This is what every team member's CSS module will pull from.

```css
/* ============================================================
   AURUM — globals.css
   THE SINGLE SOURCE OF TRUTH FOR ALL DESIGN TOKENS
   
   HOW TO USE:
   In any .module.css file, write:
     color: var(--color-ink);
     font-family: var(--font-display);
     padding: var(--space-section);
   
   NEVER write:
     color: #1a1a1a;      ← hardcoded, impossible to change globally
     font-family: serif;  ← vague, inconsistent
   ============================================================ */


/* ============================================================
   1. FONT FACE DECLARATIONS
   ============================================================ */

@font-face {
  font-family: 'Canela';
  src: url('/fonts/Canela-Light.woff2') format('woff2');
  font-weight: 300;
  font-style: normal;
  font-display: swap;
}

/* If you don't have Canela, use this Google Fonts fallback: */
/* @import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@300;400&family=Inter:wght@300;400&display=swap'); */
/* Then change --font-display to 'Cormorant Garamond' below */

@font-face {
  font-family: 'NHaasGrotesk';
  src: url('/fonts/NHaasGrotesk-55.woff2') format('woff2');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}

/* If you don't have NHaasGrotesk, use Inter as fallback */


/* ============================================================
   2. COLOUR PALETTE
   The entire brand lives in these 6 colours.
   ============================================================ */

:root {
  /* --- Core Brand Colours --- */
  --color-ink:        #0f0f0f;   /* Near-black. Used for all body text and dark sections. */
  --color-parchment:  #faf8f5;   /* Warm off-white. Used as the main background. */
  --color-cream:      #f2ede6;   /* Slightly darker warm white. Used for product card backgrounds. */
  --color-gold:       #b8923a;   /* AURUM's single accent colour. Use sparingly — price tags, hover underlines, dividers. */
  --color-mist:       #8a8680;   /* Muted warm grey. Used for secondary text, captions, legal text. */
  --color-obsidian:   #1c1a18;   /* Dark section background (footer, dark hero overlay). Almost black but warm. */

  /* --- Semantic Aliases (use these in your components) ---
     These are the same colours above, given purpose-based names.
     If we rebrand, we change the value here ONCE and everything updates. */
  
  --bg-primary:       var(--color-parchment);  /* Main page background */
  --bg-dark:          var(--color-obsidian);   /* Footer, dark sections */
  --bg-card:          var(--color-cream);      /* Product card backgrounds */

  --text-primary:     var(--color-ink);        /* Headings, body text on light bg */
  --text-on-dark:     var(--color-parchment);  /* Text on dark/footer background */
  --text-secondary:   var(--color-mist);       /* Captions, subheadings, metadata */
  --text-accent:      var(--color-gold);       /* Price tags, hover states, decorative lines */

  --border-default:   rgba(15, 15, 15, 0.12);  /* Subtle dividers on light backgrounds */
  --border-on-dark:   rgba(250, 248, 245, 0.15); /* Subtle dividers on dark backgrounds */


/* ============================================================
   3. TYPOGRAPHY
   Two fonts only. Display serif for headlines. Sans for everything else.
   ============================================================ */

  --font-display: 'Canela', 'Cormorant Garamond', 'Georgia', serif;
  /* Use for: hero headline, section titles, category labels, product names */
  /* Always set font-weight: 300 with this font. Lighter = more luxurious. */

  --font-body: 'NHaasGrotesk', 'Inter', system-ui, sans-serif;
  /* Use for: nav links, price tags, button labels, body copy, captions */
  /* Always set font-weight: 400. Never bold. */


/* ============================================================
   4. TYPE SCALE
   Use these sizes. Do not invent your own.
   ============================================================ */

  /* Display — Hero headline only */
  --text-hero:    clamp(64px, 11vw, 160px);   /* Scales from mobile to desktop automatically */
  
  /* Large — Section titles, category names */
  --text-display: clamp(40px, 6vw, 96px);

  /* Medium — Product names, feature headings */
  --text-heading: clamp(24px, 3vw, 40px);

  /* Body — Descriptive text, captions */
  --text-body:    16px;

  /* Small — Price tags, labels, nav links, legal */
  --text-small:   13px;

  /* Micro — Tracking labels, metadata above headings ("NEW COLLECTION") */
  --text-micro:   11px;


/* ============================================================
   5. LETTER SPACING
   This is what makes typography feel luxury vs. cheap.
   ============================================================ */

  --tracking-display: -0.03em;   /* Tighten large headlines. Gucci / Prada style. */
  --tracking-body:     0.01em;   /* Slightly loose for readability */
  --tracking-caps:     0.15em;   /* Wide tracking for ALL CAPS labels e.g. "NEW COLLECTION" */


/* ============================================================
   6. SPACING SYSTEM
   All spacing is based on a 8px base unit.
   ============================================================ */

  --space-1:    8px;
  --space-2:    16px;
  --space-3:    24px;
  --space-4:    32px;
  --space-5:    48px;
  --space-6:    64px;
  --space-7:    96px;
  --space-8:    128px;

  /* Section padding — the whitespace that makes the site feel expensive */
  --space-section: clamp(80px, 12vw, 160px);
  /* Every section's top and bottom padding must use this value. */

  /* Card internal padding */
  --space-card: clamp(24px, 3vw, 48px);


/* ============================================================
   7. ANIMATION TOKENS
   Everyone must use the same speeds and easing curves.
   If animations feel different across sections, the site feels broken.
   ============================================================ */

  /* Duration */
  --duration-fast:    200ms;  /* Hover colour changes, tiny reveals */
  --duration-base:    500ms;  /* Standard transitions */
  --duration-slow:    900ms;  /* Scroll-triggered reveals */
  --duration-crawl:   1400ms; /* Page load animations, hero reveals */

  /* Easing */
  --ease-out:         cubic-bezier(0.25, 0.46, 0.45, 0.94); /* Smooth decelerate. Use for reveals. */
  --ease-in-out:      cubic-bezier(0.76, 0,    0.24, 1);    /* Elegant. Use for hover transitions. */
  --ease-luxury:      cubic-bezier(0.16, 1,    0.3,  1);    /* Overshoots slightly, then settles. Very premium feel. */


/* ============================================================
   8. LAYOUT
   ============================================================ */

  --max-width:        1440px;  /* Maximum width of the content area */
  --container-padding: clamp(24px, 5vw, 80px); /* Side padding on the container, scales with viewport */

  --grid-columns: 12;          /* Base 12-column grid */
  --grid-gap: clamp(16px, 2vw, 32px); /* Gutter between grid columns */

  /* Product grid */
  --product-grid-columns: repeat(auto-fill, minmax(280px, 1fr));
  /* This auto-creates 1 column on mobile, 2 on tablet, 3-4 on desktop */


/* ============================================================
   9. Z-INDEX LAYERS
   Never guess z-index. Use these named layers.
   ============================================================ */

  --z-base:     0;
  --z-raised:   10;   /* Product cards on hover */
  --z-overlay:  50;   /* Image overlays, tints */
  --z-nav:      100;  /* Sticky navigation */
  --z-cursor:   200;  /* Custom cursor, always on top */
}


/* ============================================================
   10. GLOBAL RESETS
   Applied automatically to every element on the site.
   ============================================================ */

*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  background-color: var(--bg-primary);
  color: var(--text-primary);
  font-family: var(--font-body);
  font-size: var(--text-body);
  line-height: 1.6;
  letter-spacing: var(--tracking-body);
  -webkit-font-smoothing: antialiased; /* Makes fonts look sharper on Mac */
  -moz-osx-font-smoothing: grayscale;
}

body {
  overflow-x: hidden; /* Prevent horizontal scroll from animations */
}

img, video {
  display: block;
  max-width: 100%;
}

a {
  color: inherit;
  text-decoration: none;
}

button {
  background: none;
  border: none;
  cursor: pointer;
  font-family: var(--font-body);
  font-size: var(--text-small);
  letter-spacing: var(--tracking-caps);
  text-transform: uppercase;
}


/* ============================================================
   11. UTILITY CLASSES
   Small reusable classes. Use these in your JSX directly.
   ============================================================ */

/* Container — wraps content and centers it */
.container {
  width: 100%;
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 var(--container-padding);
}

/* Section wrapper — provides consistent vertical breathing room */
.section {
  padding: var(--space-section) 0;
}

/* Eyebrow label — the small ALL CAPS text above section headings */
/* Example: "NEW COLLECTION" above "Rings & Pendants" */
.eyebrow {
  font-family: var(--font-body);
  font-size: var(--text-micro);
  font-weight: 400;
  letter-spacing: var(--tracking-caps);
  text-transform: uppercase;
  color: var(--text-secondary);
}

/* Display headline — the large serif heading */
.headline {
  font-family: var(--font-display);
  font-weight: 300;
  letter-spacing: var(--tracking-display);
  line-height: 0.95;
  color: var(--text-primary);
}

/* Gold divider line — used as decorative accent */
.divider-gold {
  width: 40px;
  height: 1px;
  background-color: var(--text-accent);
}

/* Visually hidden — for screen readers (accessibility) */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
  white-space: nowrap;
  border: 0;
}
```

---

## Font Fallback Plan

If the custom fonts (`Canela`, `NHaasGrotesk`) are not available yet, use this Google Fonts import at the top of `globals.css` and update the variables:

```css
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@300;400&family=Inter:wght@300;400&display=swap');

:root {
  --font-display: 'Cormorant Garamond', serif;
  --font-body:    'Inter', system-ui, sans-serif;
}
```

This is just for development. The final site uses the paid fonts.

---

## Colour Usage Rules — Read Carefully

These are the rules every team member must know before using colours.

| Situation | Variable to Use | Example |
|-----------|----------------|---------|
| Page background | `var(--bg-primary)` | Body background |
| Product card background | `var(--bg-card)` | The card behind a product image |
| Footer and dark sections | `var(--bg-dark)` | Footer background |
| All body text on light bg | `var(--text-primary)` | Product name, nav links |
| Text on dark backgrounds | `var(--text-on-dark)` | Footer links, text on hero video |
| Captions, subtext, metadata | `var(--text-secondary)` | "Free shipping on orders above ₹5,000" |
| Price tags, hover accents | `var(--text-accent)` | "₹ 48,000", underline on hover |
| Dividers between sections | `var(--border-default)` | `border-top: 1px solid var(--border-default)` |

### The Gold Rule for `--text-accent` (Gold)
Gold is AURUM's brand colour. It is used in exactly 3 places:
1. Product price tags
2. The underline that appears when you hover a navigation link
3. The thin decorative horizontal divider lines between sections

If you want to use gold somewhere else, discuss with the team lead first. Overusing gold destroys the premium feel.

---

## Typography Rules

**Rule 1: Display font = headlines only.**
Use `var(--font-display)` only for the hero headline, section titles, product names, and category labels.
Never use it for body copy, nav links, or button labels.

**Rule 2: Weight 300 always for display font.**
Never use bold (`font-weight: 700`) with the display font. Light weight at large size is what creates the luxury serif look.

**Rule 3: ALL CAPS for labels only.**
The `text-transform: uppercase` treatment is for micro-labels only (eyebrow text, button labels).
Never make headings or body text uppercase.

**Rule 4: Size from the scale only.**
Never write `font-size: 72px`. Write `font-size: var(--text-display)`. This ensures the site scales correctly on all screen sizes.

---

## Animation Rules

Every team member must follow these animation rules so the site feels cohesive.

**Rule 1: On-scroll reveals must use `useScrollAnimation` hook.**
Do not write custom GSAP scroll animations. Use the shared hook from `src/hooks/useScrollAnimation.ts`.
This ensures every section reveals in the same style.

**Rule 2: Hover transitions must use the CSS variables.**
```css
/* CORRECT */
.card {
  transition: transform var(--duration-base) var(--ease-luxury);
}

/* WRONG */
.card {
  transition: transform 0.5s ease; /* does not match the rest of the site */
}
```

**Rule 3: The hover zoom on images is always the same.**
Images that zoom on hover must use exactly this:
```css
.image {
  transition: transform var(--duration-slow) var(--ease-out);
}
.image:hover {
  transform: scale(1.04); /* Not 1.05, not 1.1. Exactly 1.04. */
}
```

**Rule 4: Maximum 3 animations visible at once.**
Never trigger animations in sections that are off-screen. Use `once: true` in ScrollTrigger so animations fire once and stop.

---

## Spacing Rules

**Every section on the page must use `var(--space-section)` for its top and bottom padding.**
This is what creates the consistent "breathing room" that makes the site feel luxurious.

```css
/* In every section's .module.css */
.section {
  padding: var(--space-section) 0;
}
```

Inside a section, use the numbered spacing variables (`--space-1` through `--space-8`).
```css
.cardTitle {
  margin-bottom: var(--space-2); /* 16px */
}
.sectionHeader {
  margin-bottom: var(--space-6); /* 64px */
}
```

---

## How to Use Variables in a CSS Module

```css
/* src/components/Hero/Hero.module.css */

.heroSection {
  width: 100%;
  height: 100vh;
  background-color: var(--bg-dark);           /* From globals.css */
  padding: var(--space-section) 0;            /* From globals.css */
}

.heroHeadline {
  font-family: var(--font-display);           /* From globals.css */
  font-size: var(--text-hero);                /* From globals.css */
  font-weight: 300;
  letter-spacing: var(--tracking-display);    /* From globals.css */
  color: var(--text-on-dark);                 /* From globals.css */
  line-height: 0.95;
}
```

---

## Button Styles

There are 3 button types used across the site. Use only these 3.

**Ghost Button** — used in the hero section
```css
/* Outline only. No background. Used on dark backgrounds. */
.btn-ghost {
  display: inline-block;
  padding: 14px 36px;
  border: 1px solid var(--text-on-dark);
  color: var(--text-on-dark);
  font-family: var(--font-body);
  font-size: var(--text-micro);
  letter-spacing: var(--tracking-caps);
  text-transform: uppercase;
  transition: background-color var(--duration-base) var(--ease-in-out),
              color          var(--duration-base) var(--ease-in-out);
}
.btn-ghost:hover {
  background-color: var(--text-on-dark);
  color: var(--bg-dark);
}
```

**Text Button** — used inside product cards and nav
```css
/* No box. Just underlined text. The most restrained CTA. */
.btn-text {
  font-family: var(--font-body);
  font-size: var(--text-micro);
  letter-spacing: var(--tracking-caps);
  text-transform: uppercase;
  color: var(--text-primary);
  border-bottom: 1px solid var(--text-accent); /* Gold underline */
  padding-bottom: 2px;
  transition: color var(--duration-fast) var(--ease-out);
}
.btn-text:hover {
  color: var(--text-accent);
}
```

**Solid Button** — used for Add to Cart
```css
/* Dark solid button. Used when you need a clear primary action. */
.btn-solid {
  display: inline-block;
  padding: 14px 36px;
  background-color: var(--text-primary);
  color: var(--bg-primary);
  font-family: var(--font-body);
  font-size: var(--text-micro);
  letter-spacing: var(--tracking-caps);
  text-transform: uppercase;
  transition: background-color var(--duration-base) var(--ease-luxury),
              color          var(--duration-base) var(--ease-luxury);
}
.btn-solid:hover {
  background-color: var(--color-gold);
  color: var(--color-obsidian);
}
```

---

## What NOT to Do — Common Mistakes

| ❌ Wrong | ✅ Correct |
|---------|----------|
| `color: #1a1a1a` | `color: var(--text-primary)` |
| `font-size: 72px` | `font-size: var(--text-display)` |
| `transition: all 0.3s ease` | `transition: transform var(--duration-base) var(--ease-luxury)` |
| `padding: 80px 0` | `padding: var(--space-section) 0` |
| `font-weight: bold` on display font | `font-weight: 300` |
| `font-family: serif` | `font-family: var(--font-display)` |
| `transform: scale(1.1)` on image hover | `transform: scale(1.04)` |
| Using gold colour for decoration | Gold for: price, nav hover, dividers only |
| `z-index: 9999` | `z-index: var(--z-nav)` |