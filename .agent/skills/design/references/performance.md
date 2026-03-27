# Performance Optimization Reference

## Table of Contents
1. [Core Web Vitals Targets](#core-web-vitals-targets)
2. [Image Optimization](#image-optimization)
3. [JavaScript Bundle](#javascript-bundle)
4. [CSS Performance](#css-performance)
5. [Font Performance](#font-performance)
6. [Caching Strategy](#caching-strategy)
7. [Third-Party Scripts](#third-party-scripts)
8. [Performance Checklist](#performance-checklist)

---

## Core Web Vitals Targets

| Metric | Good | Needs Work | Poor |
|--------|------|------------|------|
| LCP (Largest Contentful Paint) | < 2.5s | 2.5-4.0s | > 4.0s |
| FID (First Input Delay) | < 100ms | 100-300ms | > 300ms |
| CLS (Cumulative Layout Shift) | < 0.1 | 0.1-0.25 | > 0.25 |
| TTFB (Time to First Byte) | < 600ms | 600-1800ms | > 1800ms |
| INP (Interaction to Next Paint) | < 200ms | 200-500ms | > 500ms |

**Methodology**: Measure → Identify bottleneck → Prioritize by impact → Implement fix → Verify improvement

**Tools**: Lighthouse (Chrome DevTools), WebPageTest, PageSpeed Insights, bundlephobia.com

---

## Image Optimization

### Format Priority
1. **AVIF** — best compression, growing support
2. **WebP** — good compression, wide support
3. **JPEG** — fallback for maximum compatibility

### Implementation
```jsx
// Next.js — use built-in Image component
import Image from 'next/image'
<Image
  src="/hero.jpg"
  alt="Descriptive alt text"
  width={1200}
  height={630}
  priority          // for above-the-fold images
  sizes="(max-width: 768px) 100vw, 50vw"
/>

// HTML — responsive with format fallback
<picture>
  <source srcset="hero.avif" type="image/avif" />
  <source srcset="hero.webp" type="image/webp" />
  <img src="hero.jpg" alt="..." loading="lazy" decoding="async" />
</picture>
```

### Rules
- **Above the fold**: `priority` or `fetchpriority="high"`, NO lazy loading
- **Below the fold**: `loading="lazy"` always
- **Sizing**: Always specify `width` and `height` to prevent CLS
- **Responsive**: Use `sizes` attribute to serve appropriate resolution
- **Quality**: 75-85% for photos, 90%+ for text-heavy images
- **Max dimensions**: Serve 2x display size maximum (e.g., 2400px for 1200px display)

---

## JavaScript Bundle

### Size Targets
| Metric | Target |
|--------|--------|
| Total JS (gzipped) | < 200KB |
| Initial bundle | < 100KB |
| Per-route chunk | < 50KB |

### Code Splitting
```jsx
// Route-based (automatic in Next.js App Router)
// No action needed — each page is its own bundle

// Component-based (heavy components)
const HeavyChart = dynamic(() => import('./Chart'), {
  loading: () => <ChartSkeleton />,
  ssr: false
})

// Library-based (import only what you need)
import { format } from 'date-fns'          // ✅ tree-shakeable
import moment from 'moment'                 // ❌ 300KB+ monolith
```

### Heavy Dependency Replacements
| Heavy | Light Alternative | Savings |
|-------|-------------------|---------|
| moment.js (300KB) | date-fns (tree-shake) | ~280KB |
| lodash (70KB) | lodash-es (tree-shake) or native | ~60KB |
| chart.js (200KB) | lightweight-charts or recharts | ~100KB |
| animate.css (80KB) | Tailwind animations + CSS | ~80KB |
| axios (14KB) | native fetch | ~14KB |

### Tree Shaking
- Use ES modules (`import/export`) not CommonJS (`require`)
- Mark package.json with `"sideEffects": false`
- Avoid barrel exports that import entire modules

---

## CSS Performance

- **Inline critical CSS**: Above-the-fold styles in `<head>` (Next.js does this automatically)
- **Defer non-critical**: Load below-fold styles asynchronously
- **Tailwind v4**: Auto-purges unused classes (Oxide engine) — no config needed
- **CSS containment**: Use `contain: layout` on independent sections to limit reflow scope
- **Avoid**: `@import` chains, deeply nested selectors (4+ levels), excessive `!important`
- **Animation perf**: Only animate `transform` and `opacity` (GPU-composited). Never animate `width`, `height`, `margin`, `padding`, `top/left`

---

## Font Performance

```html
<!-- Preload critical font -->
<link rel="preload" href="/fonts/display.woff2" as="font" type="font/woff2" crossorigin />

<!-- Font display strategy -->
<style>
  @font-face {
    font-family: 'Display';
    src: url('/fonts/display.woff2') format('woff2');
    font-display: swap;       /* show fallback immediately, swap when loaded */
    unicode-range: U+0000-00FF; /* subset to latin if applicable */
  }
</style>
```

### Rules
- **Format**: WOFF2 only (best compression, universal support)
- **Loading**: `font-display: swap` for body, `font-display: optional` for decorative
- **Subsetting**: Include only needed character sets (Latin, Cyrillic)
- **Variable fonts**: Prefer when using 3+ weights — one file, all weights
- **Limit**: Max 2-3 font families, max 4-5 total font files
- **Self-host**: Faster than Google Fonts CDN (eliminates DNS lookup + connection)

---

## Caching Strategy

### Static Assets (fonts, images, JS/CSS bundles)
```
Cache-Control: public, max-age=31536000, immutable
```
Files with content hash in name — cache forever.

### HTML Pages
```
Cache-Control: public, max-age=0, must-revalidate
```
Always check for updates.

### API Responses
```
Cache-Control: private, max-age=60, stale-while-revalidate=300
```
Short cache, serve stale while refreshing.

### CDN
- Static assets through CDN (Vercel, Cloudflare, AWS CloudFront)
- Enable Brotli compression
- Set `Vary: Accept-Encoding` header

---

## Third-Party Scripts

### Loading Strategy
```html
<!-- Critical (analytics): defer -->
<script src="analytics.js" defer></script>

<!-- Non-critical (chat widget): lazy load -->
<script>
  // Load after user interaction or 5s idle
  const loadChat = () => import('./chat-widget.js');
  window.addEventListener('scroll', loadChat, { once: true });
  setTimeout(loadChat, 5000);
</script>
```

### Rules
- Audit every third-party script with Lighthouse
- Defer everything that isn't needed for first paint
- Lazy load chat widgets, feedback tools, social embeds
- Set `loading="lazy"` on third-party iframes
- Consider `requestIdleCallback` for non-urgent scripts

---

## Performance Checklist

### Images
- [ ] Above-fold images have `priority` / `fetchpriority="high"`
- [ ] Below-fold images use `loading="lazy"`
- [ ] All images have explicit `width` and `height`
- [ ] Serving AVIF/WebP with fallbacks
- [ ] No images larger than 2x display size

### JavaScript
- [ ] Total bundle < 200KB gzipped
- [ ] Heavy components use dynamic imports
- [ ] No unused heavy dependencies
- [ ] Tree shaking working (ES modules)

### CSS
- [ ] Only animating `transform` and `opacity`
- [ ] No layout thrashing animations
- [ ] Tailwind purge working (check build output)

### Fonts
- [ ] Using WOFF2 format
- [ ] Critical font preloaded
- [ ] `font-display: swap` set
- [ ] Max 3 font families

### General
- [ ] LCP < 2.5s on mobile 3G
- [ ] CLS < 0.1 (no layout shifts)
- [ ] No render-blocking resources in `<head>`
- [ ] Compression enabled (Brotli preferred)
