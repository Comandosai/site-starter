# Animations & Scroll Effects Reference

## Table of Contents
1. [Scroll as Narrative](#scroll-as-narrative)
2. [Parallax Layer System](#parallax-layer-system)
3. [Tool Comparison](#tool-comparison)
4. [Sticky Sections](#sticky-sections)
5. [Spring Physics](#spring-physics)
6. [Text Reveal Patterns](#text-reveal-patterns)
7. [Page Load Choreography](#page-load-choreography)
8. [Mobile-Safe Animations](#mobile-safe-animations)
9. [Anti-Patterns](#anti-patterns)

---

## Scroll as Narrative

Structure scroll-driven pages as a 5-part story:

| Act | Purpose | Technique |
|-----|---------|-----------|
| **Hook** (0-10vh) | Grab attention immediately | Bold hero, auto-playing motion, scale-in headline |
| **Context** (10-40vh) | Set the scene, explain what/why | Fade-in sections, subtle parallax, text reveals |
| **Journey** (40-70vh) | Show the product/features/story | Sticky sections, horizontal scroll, bento reveals |
| **Climax** (70-90vh) | Most impressive visual moment | Full parallax scene, 3D transform, canvas animation |
| **Resolution** (90-100vh) | CTA, pricing, footer | Settle motion, clear call to action, reduce complexity |

**Key principle**: Motion intensity should build, peak, then resolve. Never start at maximum intensity.

---

## Parallax Layer System

Create depth with elements moving at different scroll speeds:

| Layer | Speed | Z-Index | Content |
|-------|-------|---------|---------|
| Background | 0.2x | z-0 | Gradient, texture, ambient shapes |
| Midground | 0.5x | z-10 | Supporting visuals, decorative elements |
| Foreground | 1.0x (scroll speed) | z-20 | Main content, text, cards |
| Floating | 1.2x | z-30 | Accent elements, small details |

### GSAP Implementation
```js
gsap.to('.background', {
  y: -100,
  ease: 'none',
  scrollTrigger: {
    trigger: '.section',
    start: 'top bottom',
    end: 'bottom top',
    scrub: true   // ties animation to scroll position
  }
});
```

### CSS-Only Parallax
```css
.parallax-container {
  perspective: 1px;
  overflow-y: auto;
  height: 100vh;
}
.parallax-slow {
  transform: translateZ(-1px) scale(2);  /* moves slower */
}
.parallax-normal {
  transform: translateZ(0);              /* normal speed */
}
```

---

## Tool Comparison

| Tool | Best For | Size | Learning Curve |
|------|----------|------|----------------|
| **GSAP + ScrollTrigger** | Complex timelines, scrub animations, pinning | ~30KB | Medium |
| **Framer Motion** | React components, layout animations, gestures | ~30KB | Low (React) |
| **Lenis** | Smooth scroll only (pairs with GSAP/CSS) | ~5KB | Low |
| **CSS scroll-timeline** | Simple scroll-linked animations, no JS needed | 0KB | Low |
| **Locomotive Scroll** | Smooth scroll + parallax (legacy projects) | ~15KB | Medium |

### Recommendations
- **React/Next.js project**: Framer Motion for component animations + GSAP ScrollTrigger for scroll scenes
- **Lightweight**: CSS scroll-timeline + Lenis for smooth scroll
- **Maximum control**: GSAP for everything

### CSS scroll-timeline (native, no JS)
```css
@keyframes fade-in {
  from { opacity: 0; transform: translateY(40px); }
  to { opacity: 1; transform: translateY(0); }
}
.reveal {
  animation: fade-in linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 100%;
}
```

---

## Sticky Sections

Pin a section while content scrolls within it.

### CSS Sticky (Simple)
```html
<section class="min-h-[200vh] relative">
  <div class="sticky top-0 h-screen flex items-center">
    <!-- content that stays pinned -->
  </div>
</section>
```
Parent height controls how long the section stays pinned.

### GSAP Pin (Advanced)
```js
ScrollTrigger.create({
  trigger: '.panel',
  pin: true,
  start: 'top top',
  end: '+=200%',    // stay pinned for 2x viewport height
  scrub: 1
});
```

### Horizontal Scroll Section
```js
const panels = gsap.utils.toArray('.panel');
gsap.to(panels, {
  xPercent: -100 * (panels.length - 1),
  ease: 'none',
  scrollTrigger: {
    trigger: '.container',
    pin: true,
    scrub: 1,
    end: () => '+=' + document.querySelector('.container').scrollWidth
  }
});
```

---

## Spring Physics

Natural motion using spring-based easing instead of linear/cubic-bezier.

### Framer Motion Springs
```jsx
<motion.div
  initial={{ opacity: 0, y: 40 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{
    type: 'spring',
    stiffness: 100,    // higher = snappier
    damping: 20,       // higher = less oscillation
    mass: 1            // higher = more inertia
  }}
/>
```

### Spring Presets
| Name | Stiffness | Damping | Feel |
|------|-----------|---------|------|
| Gentle | 80 | 20 | Soft, floating |
| Snappy | 200 | 25 | Quick, decisive |
| Bouncy | 150 | 12 | Playful, elastic |
| Heavy | 60 | 30 | Weighted, deliberate |

### Custom Cubic-Bezier Equivalents
When springs aren't available (pure CSS):
```css
--ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);      /* fast start, gentle end */
--ease-out-back: cubic-bezier(0.34, 1.56, 0.64, 1);   /* slight overshoot */
--ease-in-out-circ: cubic-bezier(0.85, 0, 0.15, 1);   /* smooth S-curve */
```

---

## Text Reveal Patterns

### Word-by-Word Fade
```jsx
// Split text into words, stagger their appearance
{words.map((word, i) => (
  <motion.span
    key={i}
    initial={{ opacity: 0, y: 20 }}
    whileInView={{ opacity: 1, y: 0 }}
    transition={{ delay: i * 0.05 }}
  >
    {word}{' '}
  </motion.span>
))}
```

### Line Clip Reveal
```css
.reveal-line {
  clip-path: inset(0 100% 0 0);
  animation: reveal 0.8s var(--ease-out-expo) forwards;
}
@keyframes reveal {
  to { clip-path: inset(0 0 0 0); }
}
```

### Typewriter Effect
```css
.typewriter {
  overflow: hidden;
  white-space: nowrap;
  border-right: 2px solid currentColor;
  animation:
    typing 2s steps(30) forwards,
    blink 0.7s step-end infinite;
}
```

---

## Page Load Choreography

One well-orchestrated page load is worth more than scattered micro-interactions.

### Staggered Entry Pattern
```jsx
const container = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.08,    // 80ms between each child
      delayChildren: 0.1        // 100ms initial delay
    }
  }
};
const item = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0, transition: { type: 'spring', stiffness: 100, damping: 20 } }
};

<motion.div variants={container} initial="hidden" animate="visible">
  <motion.h1 variants={item}>Headline</motion.h1>
  <motion.p variants={item}>Subtext</motion.p>
  <motion.button variants={item}>CTA</motion.button>
</motion.div>
```

### Load Sequence
1. Background/structure appears instantly (no animation)
2. Primary headline fades up (0-200ms)
3. Supporting text follows (100-300ms)
4. CTA button enters (200-400ms)
5. Secondary elements stagger in (300-600ms)
6. Decorative/ambient motion begins (500ms+)

---

## Mobile-Safe Animations

### Rules
- **Reduce parallax**: Use 0.5x layer speed max (vs 0.2x desktop)
- **Simplify**: Fewer animated elements, simpler transforms
- **Respect preferences**: Always check `prefers-reduced-motion`
- **Performance**: Test on mid-range Android device, not just iPhone
- **Touch scroll**: Avoid scroll hijacking — mobile users expect native scroll feel
- **Viewport height**: Use `100dvh` not `100vh` to account for mobile browser chrome

### prefers-reduced-motion
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

```jsx
// Framer Motion
<motion.div
  initial={{ opacity: 0 }}
  animate={{ opacity: 1 }}
  transition={{ duration: window.matchMedia('(prefers-reduced-motion)').matches ? 0 : 0.5 }}
/>
```

---

## Anti-Patterns

### Never Do
- **Scroll hijacking**: Taking over native scroll behavior (custom scroll speed, snap-to-section on every section)
- **Animation overload**: More than 3 animated elements visible simultaneously
- **Desktop-only**: Animations that only work with mouse hover and break on touch
- **Infinite loops**: Continuously running animations that drain battery
- **Layout animations**: Animating `width`, `height`, `margin`, `padding` (causes reflow)
- **Delay walls**: Making users wait 2+ seconds before content is interactive
- **Auto-playing video**: Full-screen background video without user consent on mobile

### Always Do
- Animate only `transform` and `opacity`
- Test with `prefers-reduced-motion` enabled
- Ensure content is readable without animations
- Keep total animation time for page load under 1 second
- Use `will-change: transform` sparingly and only on elements about to animate
