---
name: design-master
description: "Ultimate frontend design skill for creating beautiful, distinctive websites and applications with premium agency-level aesthetics. Eliminates AI-generated design clichés (generic fonts, neon glows, symmetric card grids) and enforces professional typography, color systems, layout composition, motion design, accessibility, and performance. Use when: (1) Building any frontend interface — websites, landing pages, dashboards, web apps, (2) Styling or designing UI components, (3) Improving or redesigning existing sites, (4) Adding animations or scroll effects, (5) Working with React, Next.js, Tailwind CSS, or any frontend stack, (6) Creating responsive, mobile-first interfaces. Covers: anti-AI-slop patterns, design tokens, Tailwind v4, dark mode, loading/error/empty states, Core Web Vitals optimization, mobile touch design, scroll-driven animations."
---

# Design Master

## 1. Design Philosophy

Every interface must look like a human designer made it, not an AI.

### Design Intensity (1-10)
Set the creative level based on the project:

| Level | Style | Use For |
|-------|-------|---------|
| 1-3 | Clean, corporate, conventional | Admin panels, internal tools, docs |
| 4-6 | Modern, polished, subtle personality | SaaS products, business sites, apps |
| 7-8 | Bold, editorial, distinctive | Landing pages, portfolios, brands |
| 9-10 | Experimental, art-directed, boundary-pushing | Creative agencies, art projects |

**Default: 6** (modern premium) unless specified otherwise.

### Decision Framework
Before writing any code, answer:
1. **Purpose** — What must the user accomplish?
2. **Tone** — What emotion should the design evoke? Pick a vibe archetype from `references/aesthetics.md`
3. **Constraints** — Device targets, accessibility needs, performance budget
4. **Differentiation** — What makes this NOT look like every other AI-generated site?

---

## 2. Anti-AI Aesthetic Rules

### Banned Patterns (NEVER do these)

**Visual:**
- Neon glows or glow effects on dark backgrounds
- Pure black `#000000` or `bg-black` backgrounds
- Purple-to-blue gradient hero sections
- Oversaturated accent colors (neon green, electric blue)
- Perfectly symmetric three-column card layouts with identical styling
- Full-screen generic stock photo heroes
- Glassmorphism on every element
- Rainbow or multi-color gradient text

**Typography:**
- Inter, Roboto, Arial, Helvetica, Open Sans, Lato as primary font
- Montserrat or Poppins used generically without intention
- Oversized hero text (`text-7xl`+) without visual justification
- Same font weight throughout (everything `font-normal`)

**Content:**
- "Lorem ipsum" or any placeholder latin text
- "John Doe", "Jane Smith", "Acme Corp", "Tech Solutions"
- Fake round metrics: "10,000+ users", "99.9% uptime", "500+ companies"
- AI cliché copy: "Elevate", "Seamless", "Next-Gen", "Revolutionize", "Unlock", "Supercharge"
- Emoji as feature icons (🚀 ✨ 💡 📊)
- `https://example.com` or placeholder image URLs

**Code:**
- `// ...` or `/* ... */` as placeholders for omitted code
- `// TODO` or `// rest follows same pattern`
- "for brevity" or "similar to above" instead of actual implementation
- Skeleton/descriptor comments replacing real code

### Required Patterns (ALWAYS do these)
- Choose a distinctive font pairing (see `references/aesthetics.md`)
- Use asymmetric layouts — break the grid intentionally
- Add real texture: noise overlays, grain, gradient mesh, layered transparencies
- Every interactive element has hover, active, and focus states
- Every design must look different from the last — vary fonts, layouts, palettes
- Write real, contextual content appropriate to the project
- Output complete, production-ready code — never truncate or abbreviate

---

## 3. Typography & Color

### Typography Hierarchy
- **Families**: Max 2-3. One display (distinctive), one body (readable).
- **Body**: Min 16px, line-height 1.5-1.6, max-width `65ch`
- **Headings**: Line-height 1.1-1.2, use font-weight variation (bold headings, medium labels, regular body)
- **Scale ratio**: At least 2:1 between hero heading and body text
- **Letter spacing**: Tight (`tracking-tight`) for large headings, normal for body

### Color Architecture
Three-layer token system (details in `references/components.md`):
1. **Primitive** — raw OKLCH values
2. **Semantic** — `--color-text`, `--color-bg`, `--color-accent`
3. **Component** — `--button-bg`, `--card-border`

**Rules:**
- Use OKLCH color model for perceptually uniform palettes
- Dominant + accent strategy: one primary color, one accent, neutrals
- Contrast ratio 4.5:1 minimum for body text (WCAG AA)
- Dark mode: `oklch(0.14-0.18 ...)` range, never pure black. Reduce accent saturation 10-20%.

Curated palettes: see `references/aesthetics.md`

---

## 4. Layout & Composition

### Spatial Principles
- **Asymmetry over symmetry** — offset elements, unequal column ratios (40/60, 35/65)
- **Generous whitespace** — section padding `py-24` to `py-40`, not `py-4` to `py-8`
- **Grid-breaking** — at least one element per page should overlap, offset, or break the grid
- **Negative space is intentional** — empty space communicates hierarchy and premium feel
- **Content density varies** — alternate dense and sparse sections for rhythm

### Layout Archetypes
Choose one as the foundation. Details in `references/aesthetics.md`.

| Archetype | Description | Best For |
|-----------|-------------|----------|
| Asymmetrical Bento | Varied-size blocks in broken grid | Feature showcases, dashboards |
| Z-Axis Cascade | Overlapping layers creating depth | Product pages, portfolios |
| Editorial Split | Asymmetric text/visual halves | Landing sections, about pages |
| Scrolling Canvas | Full-width sequential screens | Storytelling, product launches |

### Responsive Strategy
- **Mobile-first**: Base classes for mobile, `sm:` / `md:` / `lg:` for expansion
- **Breakpoints**: Test at 375 / 768 / 1024 / 1440px
- **Container queries**: Use `@container` for component-level responsiveness (Tailwind v4)
- **`min-h-dvh`** not `min-h-screen` (accounts for mobile browser chrome)

---

## 5. Motion & Animation

### Philosophy
Motion is purposeful communication, not decoration. Every animation must answer: "What does this help the user understand?"

### Duration Rules
| Type | Duration | Easing |
|------|----------|--------|
| Micro-interaction (hover, toggle) | 150-200ms | `ease-out` |
| State transition (tab change, expand) | 200-300ms | `ease-out` or spring |
| Page transition (route change) | 300-500ms | `ease-in-out` |
| Entrance animation (scroll reveal) | 400-600ms | Spring or `ease-out-expo` |

### Performance Rules
- **Only animate** `transform` and `opacity` — never `width`, `height`, `margin`, `top/left`
- **`prefers-reduced-motion`**: Always respect. Disable animations when reduced motion is set.
- **Staggered reveals**: Max 5-6 elements, 60-80ms stagger delay between children
- **No infinite loops** unless explicitly requested (drain battery on mobile)

### Page Load Choreography
Load sequence: background instant → headline (0-200ms) → subtext (100-300ms) → CTA (200-400ms) → secondary elements stagger (300-600ms)

Advanced patterns: see `references/animations.md`

---

## 6. Accessibility (Non-Negotiable)

- **Contrast**: 4.5:1 body text, 3:1 large text and UI components
- **Focus**: Visible `focus-visible:ring-2` on all interactive elements
- **Alt text**: Meaningful on informational images, `alt=""` on decorative
- **Tab order**: Matches visual reading order
- **Touch targets**: Minimum 44×44px with 8px spacing
- **Motion**: Respect `prefers-reduced-motion` — no exceptions
- **Forms**: Every input has a visible `<label>` with `for` attribute
- **Color**: Never the sole indicator of state (add icon or text)
- **Semantic HTML**: Use `<nav>`, `<main>`, `<section>`, `<article>`, `<button>` — not `<div>` for everything

---

## 7. Implementation Stack

### Primary: React / Next.js + Tailwind v4
```css
/* Tailwind v4: CSS-first configuration */
@import "tailwindcss";

@theme {
  --color-bg: oklch(0.98 0.005 260);
  --color-surface: oklch(1 0 0);
  --color-text: oklch(0.20 0.02 260);
  --color-accent: oklch(0.62 0.19 260);
  --font-display: 'Space Grotesk', sans-serif;
  --font-body: 'DM Sans', sans-serif;
}
```

### Key Rules
- **Icons**: SVG only (Lucide, Heroicons). Never emoji. Never guess brand logos — use text placeholder.
- **Images**: Next.js `<Image>` with `priority` for above-fold, `loading="lazy"` for below
- **Fonts**: Self-hosted WOFF2, `font-display: swap`, max 2-3 families
- **Components**: Use design tokens, not hard-coded values (see `references/components.md`)

### Other Stacks
Design principles are stack-agnostic. For Vue/Svelte/plain HTML:
- Same color/typography/spacing rules apply
- Replace Tailwind utilities with equivalent CSS if needed
- Component patterns translate directly

---

## 8. Output Standards

### Complete Output Enforcement
Treat every task as production-critical. A partial output is a broken output.

- Deliver full, executable code — every component, every style, every state
- If scope exceeds token limits, split into files and implement each fully. Use clean breakpoints.
- Never use descriptions or comments as substitutes for real code

### Pre-Delivery Checklist
Before considering ANY output complete, verify:

- [ ] No banned fonts (Inter, Roboto, Arial, Helvetica, Open Sans, Lato)
- [ ] No emoji icons — SVG icon library used
- [ ] No placeholder content (Lorem ipsum, John Doe, fake metrics)
- [ ] Hover state on every clickable element (with `transition-colors duration-200`)
- [ ] `cursor-pointer` on all interactive elements
- [ ] Focus-visible ring on all interactive elements
- [ ] Responsive at 375 / 768 / 1024 / 1440px
- [ ] `min-h-dvh` used (not `min-h-screen`)
- [ ] Dark mode handled (or explicitly noted as light-only)
- [ ] All images have `alt` text
- [ ] No horizontal scroll on any viewport
- [ ] Loading, error, empty states for data-dependent components

---

## 9. Reference Guide

Read these files when the specific domain is relevant to the task:

| Reference | Read When |
|-----------|-----------|
| [aesthetics.md](references/aesthetics.md) | Choosing visual direction, fonts, color palettes, vibe archetypes, layout archetypes, advanced visual techniques |
| [mobile.md](references/mobile.md) | Building mobile app or mobile-responsive site, touch interactions, platform-specific design |
| [animations.md](references/animations.md) | Adding scroll animations, parallax, page transitions, spring physics, text reveals |
| [performance.md](references/performance.md) | Optimizing load times, Core Web Vitals, image/font/bundle optimization |
| [components.md](references/components.md) | Building component systems, design tokens, Tailwind v4 config, dark mode, container queries |
| [redesign.md](references/redesign.md) | Improving or auditing an existing website or application |
| [ux-states.md](references/ux-states.md) | Implementing loading, error, empty states, form patterns, button states |

---

## 10. Workflow

### Building a New Site or Page
1. Clarify design intensity (1-10) and pick a vibe archetype → read `references/aesthetics.md`
2. Select font pairing and color palette
3. Choose layout archetype
4. Implement with full output standards
5. Run pre-delivery checklist

### Redesigning an Existing Site
1. Read `references/redesign.md`
2. Audit: scan → diagnose → score each category
3. Fix in priority order: fonts → colors → spacing → interactions → layout → components → animation

### Building Components
1. Read `references/components.md` + `references/ux-states.md`
2. Define design tokens first
3. Build with variant system (solid/outline/ghost)
4. Handle all states: default, hover, active, focus, loading, disabled, error

### Adding Animations
1. Read `references/animations.md`
2. Choose tool (Framer Motion for React, GSAP for complex scroll, CSS for simple)
3. Implement with performance rules (transform + opacity only)
4. Test `prefers-reduced-motion`

### Mobile-First Design
1. Read `references/mobile.md`
2. Design for 375px width first
3. Check thumb zones, touch targets, safe areas
4. Expand to larger breakpoints with `sm:` / `md:` / `lg:`
