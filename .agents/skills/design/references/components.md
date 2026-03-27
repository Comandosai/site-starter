# Component Patterns & Design Tokens Reference

## Table of Contents
1. [Design Token Architecture](#design-token-architecture)
2. [Tailwind v4 Token Implementation](#tailwind-v4-token-implementation)
3. [Component Variant System](#component-variant-system)
4. [Layout Pattern Templates](#layout-pattern-templates)
5. [Dark Mode Implementation](#dark-mode-implementation)
6. [Container Queries](#container-queries)
7. [Component Extraction Rules](#component-extraction-rules)

---

## Design Token Architecture

Three-layer token system. NEVER hard-code raw values in components.

### Layer 1: Primitive Tokens (Raw Values)
```css
/* Raw scale — never reference directly in components */
--gray-50: oklch(0.98 0.005 260);
--gray-100: oklch(0.95 0.005 260);
--gray-200: oklch(0.90 0.01 260);
--gray-900: oklch(0.20 0.02 260);
--gray-950: oklch(0.14 0.02 260);
--blue-500: oklch(0.62 0.19 260);
--radius-sm: 0.375rem;
--radius-md: 0.5rem;
--radius-lg: 0.75rem;
--radius-xl: 1rem;
--radius-2xl: 1.5rem;
```

### Layer 2: Semantic Tokens (Purpose)
```css
/* Reference these in components */
--color-bg: var(--gray-50);
--color-surface: white;
--color-surface-raised: white;
--color-text-primary: var(--gray-900);
--color-text-secondary: var(--gray-500);
--color-text-muted: var(--gray-400);
--color-border: var(--gray-200);
--color-accent: var(--blue-500);
--color-error: oklch(0.58 0.22 25);
--color-success: oklch(0.60 0.18 155);
--color-warning: oklch(0.70 0.17 75);
--radius-component: var(--radius-xl);
--radius-button: var(--radius-lg);
--radius-input: var(--radius-lg);
--radius-card: var(--radius-2xl);
```

### Layer 3: Component Tokens (Specific)
```css
--button-bg: var(--color-accent);
--button-text: white;
--button-radius: var(--radius-button);
--card-bg: var(--color-surface);
--card-border: var(--color-border);
--card-radius: var(--radius-card);
--card-shadow: 0 1px 3px oklch(0 0 0 / 0.08);
```

### Spacing Scale
| Token | Value | Use |
|-------|-------|-----|
| `space-1` | 4px | Tight inline spacing |
| `space-2` | 8px | Icon-text gap, compact padding |
| `space-3` | 12px | Input padding, small card padding |
| `space-4` | 16px | Standard component padding |
| `space-6` | 24px | Card padding, section gaps |
| `space-8` | 32px | Section padding |
| `space-12` | 48px | Section vertical spacing |
| `space-16` | 64px | Major section spacing |
| `space-24` | 96px | Hero/section vertical padding |

### Typography Scale
| Token | Size | Line Height | Use |
|-------|------|-------------|-----|
| `text-xs` | 12px | 1.5 | Captions, labels |
| `text-sm` | 14px | 1.5 | Secondary text, metadata |
| `text-base` | 16px | 1.6 | Body text |
| `text-lg` | 18px | 1.5 | Lead paragraphs |
| `text-xl` | 20px | 1.4 | Card titles |
| `text-2xl` | 24px | 1.3 | Section titles |
| `text-3xl` | 30px | 1.2 | Page titles |
| `text-4xl` | 36px | 1.15 | Hero headings |
| `text-5xl` | 48px | 1.1 | Display headings |
| `text-6xl` | 60px | 1.05 | Large display |

---

## Tailwind v4 Token Implementation

### CSS-First Configuration
```css
/* app.css — replaces tailwind.config.js */
@import "tailwindcss";

@theme {
  /* Colors */
  --color-bg: oklch(0.98 0.005 260);
  --color-surface: oklch(1 0 0);
  --color-text: oklch(0.20 0.02 260);
  --color-text-muted: oklch(0.55 0.02 260);
  --color-accent: oklch(0.62 0.19 260);
  --color-border: oklch(0.90 0.01 260);

  /* Spacing (extend defaults) */
  --spacing-18: 4.5rem;
  --spacing-88: 22rem;

  /* Radius */
  --radius-card: 1.5rem;
  --radius-button: 0.75rem;

  /* Fonts */
  --font-display: 'Space Grotesk', sans-serif;
  --font-body: 'DM Sans', sans-serif;
}
```

### Usage in Components
```html
<div class="bg-bg text-text rounded-card p-6">
  <h2 class="font-display text-2xl font-bold">Title</h2>
  <p class="font-body text-text-muted text-base mt-2">Description</p>
  <button class="bg-accent text-white rounded-button px-4 py-2.5 mt-4">
    Action
  </button>
</div>
```

---

## Component Variant System

### Button Variants
```jsx
const variants = {
  solid:   'bg-accent text-white hover:brightness-110',
  outline: 'border-2 border-accent text-accent hover:bg-accent/10',
  ghost:   'text-accent hover:bg-accent/10',
  link:    'text-accent underline-offset-4 hover:underline',
};

const sizes = {
  sm: 'px-3 py-1.5 text-sm rounded-lg',
  md: 'px-4 py-2.5 text-sm rounded-xl',
  lg: 'px-6 py-3 text-base rounded-xl',
};
```

### Card Variants
```jsx
const cardVariants = {
  elevated:  'bg-surface shadow-md rounded-card',
  outlined:  'bg-surface border border-border rounded-card',
  filled:    'bg-bg rounded-card',
  glass:     'bg-white/5 backdrop-blur-xl border border-white/10 rounded-card',
};
```

### Input States
```jsx
const inputStates = {
  default:  'border-border focus:border-accent focus:ring-2 focus:ring-accent/20',
  error:    'border-red-400 focus:border-red-500 focus:ring-2 focus:ring-red-500/20',
  success:  'border-green-400',
  disabled: 'bg-gray-50 text-gray-400 cursor-not-allowed',
};
```

---

## Layout Pattern Templates

### Page Shell
```html
<div class="min-h-dvh bg-bg text-text">
  <header class="sticky top-0 z-50 bg-surface/80 backdrop-blur-lg border-b border-border">
    <nav class="max-w-7xl mx-auto px-4 h-16 flex items-center justify-between">
      <!-- logo, nav links, actions -->
    </nav>
  </header>
  <main class="max-w-7xl mx-auto px-4 py-12">
    <!-- page content -->
  </main>
  <footer class="border-t border-border mt-24">
    <div class="max-w-7xl mx-auto px-4 py-12">
      <!-- footer content -->
    </div>
  </footer>
</div>
```

### Bento Grid
```html
<div class="grid grid-cols-4 gap-4 md:grid-cols-12">
  <div class="col-span-4 md:col-span-8 row-span-2 bg-surface rounded-card p-6">Feature</div>
  <div class="col-span-4 bg-surface rounded-card p-6">Stat 1</div>
  <div class="col-span-2 md:col-span-4 bg-surface rounded-card p-6">Stat 2</div>
  <div class="col-span-2 md:col-span-4 bg-surface rounded-card p-6">Stat 3</div>
  <div class="col-span-4 md:col-span-4 bg-surface rounded-card p-6">Detail</div>
</div>
```

### Sidebar Layout
```html
<div class="flex min-h-dvh">
  <aside class="w-64 shrink-0 border-r border-border p-4 hidden lg:block">
    <!-- navigation -->
  </aside>
  <main class="flex-1 p-6 overflow-auto">
    <!-- content -->
  </main>
</div>
```

---

## Dark Mode Implementation

### Tailwind v4 Dark Mode
```css
@theme {
  --color-bg: oklch(0.98 0.005 260);
  --color-surface: oklch(1 0 0);
  --color-text: oklch(0.20 0.02 260);
}

/* Dark overrides — use .dark class or @media */
.dark {
  --color-bg: oklch(0.14 0.01 260);
  --color-surface: oklch(0.18 0.015 260);
  --color-text: oklch(0.93 0.005 260);
}
```

### Color Mapping
| Semantic | Light | Dark |
|----------|-------|------|
| Background | `oklch(0.98 ...)` near-white | `oklch(0.14 ...)` near-black (not #000) |
| Surface | white | `oklch(0.18 ...)` raised dark |
| Surface raised | white + shadow | `oklch(0.22 ...)` brighter dark |
| Primary text | `oklch(0.20 ...)` near-black | `oklch(0.93 ...)` near-white |
| Secondary text | `oklch(0.55 ...)` mid-gray | `oklch(0.65 ...)` lighter gray |
| Border | `oklch(0.90 ...)` light gray | `oklch(0.25 ...)` subtle dark |
| Accent | Full saturation | Reduce saturation 10-20% |

### System Preference Detection
```js
// Respect OS setting
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
document.documentElement.classList.toggle('dark', prefersDark);
```

---

## Container Queries

Use container queries for component-level responsiveness (not just viewport).

### Tailwind v4 Syntax
```html
<div class="@container">
  <div class="flex flex-col @md:flex-row @lg:grid @lg:grid-cols-3 gap-4">
    <!-- adapts based on container width, not viewport -->
  </div>
</div>
```

### When to Use
| Use Container Queries | Use Viewport Breakpoints |
|----------------------|--------------------------|
| Card in variable-width grid | Full-page layout changes |
| Sidebar content | Navigation mode (hamburger vs full) |
| Reusable widget | Header/footer structure |
| Dashboard panel | Overall content width |

---

## Component Extraction Rules

### When to Extract
- **3+ repetitions** of the same pattern
- **Complex state management** (loading, error, variants)
- **Design system element** (button, card, input)
- **Independent behavior** (modal, dropdown, tooltip)

### When NOT to Extract
- Used only once or twice
- Simple markup (< 5 lines) without logic
- Would require many props to be flexible
- Premature — wait until pattern is stable

### Extraction Template
```tsx
interface ComponentProps {
  variant?: 'solid' | 'outline' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  children: React.ReactNode;
}

export function Component({ variant = 'solid', size = 'md', children }: ComponentProps) {
  return (
    <div className={cn(baseStyles, variants[variant], sizes[size])}>
      {children}
    </div>
  );
}
```
