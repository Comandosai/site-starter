# Premium Aesthetics Reference

## Table of Contents
1. [Vibe Archetypes](#vibe-archetypes)
2. [Layout Archetypes](#layout-archetypes)
3. [Curated Font Pairings](#curated-font-pairings)
4. [Color Palette Templates](#color-palette-templates)
5. [Advanced Visual Techniques](#advanced-visual-techniques)
6. [Creative Interaction Patterns](#creative-interaction-patterns)

---

## Vibe Archetypes

Choose one archetype as the foundation. Mix elements only with intention.

### Ethereal Glass (Tech / SaaS / AI)
- **Palette**: Cool grays, translucent whites, single vivid accent (electric blue, mint, violet)
- **Fonts**: Satoshi + Inter Tight, or Space Grotesk + DM Sans
- **Surface**: Frosted glass panels (`backdrop-blur-xl bg-white/5`), subtle noise overlay
- **Motion**: Slow float animations, gentle parallax, smooth morphing
- **Spacing**: Expansive (`py-32` to `py-48`), breathable
- **Use when**: Developer tools, AI products, cloud platforms, fintech dashboards

### Editorial Luxury (Lifestyle / Fashion / Premium)
- **Palette**: Deep neutrals (charcoal, cream, stone), gold or copper accent
- **Fonts**: Playfair Display + Source Serif Pro, or Cormorant Garamond + Outfit
- **Surface**: Flat, photographic, high contrast between text and image
- **Motion**: Reveal-on-scroll, clip-path transitions, slow image zooms
- **Spacing**: Dramatic whitespace (`py-40`+), oversized typography
- **Use when**: Fashion brands, luxury goods, editorial publications, portfolio sites

### Soft Structuralism (Consumer / Health / Education)
- **Palette**: Warm neutrals with soft pastels (peach, sage, lavender), muted accent
- **Fonts**: General Sans + Cabinet Grotesk, or Outfit + Plus Jakarta Sans
- **Surface**: Rounded corners (`rounded-2xl` to `rounded-3xl`), soft shadows, layered cards
- **Motion**: Bouncy spring animations, playful hover effects, staggered reveals
- **Spacing**: Comfortable (`py-24` to `py-32`), card-based
- **Use when**: Health apps, education platforms, consumer SaaS, community products

### Brutalist Digital (Creative / Agency / Experimental)
- **Palette**: High contrast (near-black + near-white), one neon accent used sparingly
- **Fonts**: Clash Display + Syne, or Space Mono + Instrument Sans
- **Surface**: Hard edges, visible grid, raw borders, no rounded corners
- **Motion**: Glitch effects, abrupt transitions, cursor-following elements
- **Spacing**: Dense and intentional, asymmetric gaps
- **Use when**: Creative agencies, art portfolios, experimental projects, music

---

## Layout Archetypes

### Asymmetrical Bento Grid
Content blocks of varying sizes arranged in a broken grid. No two adjacent blocks share the same dimensions.
```
| col-span-2  | col-span-1 |
| col-span-1  | col-span-3 |
| col-span-2  | col-span-2 |
```
- Use `grid-cols-4` or `grid-cols-12` base
- Mix `col-span-1` through `col-span-3` with varying `row-span`
- At least one block breaks the pattern (oversized or offset)
- On mobile: stack to single column, preserve visual hierarchy

### Z-Axis Cascade (Overlapping Layers)
Elements overlap to create depth. Cards, images, or sections partially cover each other.
- Use negative margins (`-mt-16`, `-ml-8`) or absolute positioning
- Layer with `z-10`, `z-20`, `z-30` scale
- Add subtle shadows to reinforce depth (`shadow-2xl` on front elements)
- Parallax scroll enhances the depth illusion

### Editorial Split
Page divided into two asymmetric halves — text-heavy on one side, visual on the other.
- Ratios: 40/60 or 35/65 (never 50/50)
- Text side: large headline, body, CTA stacked vertically
- Visual side: full-bleed image, video, or illustration
- On scroll: text and visual animate independently
- On mobile: visual moves above text, maintains hierarchy

### Scrolling Canvas
Full-width sections that act as individual "screens" stitched together vertically.
- Each section: `min-h-screen` with centered content
- Section transitions: color shift, parallax, or reveal animation
- Navigation: sticky dot-nav or progress bar
- Content density varies section-to-section

---

## Curated Font Pairings

### Luxury / Editorial
| Display | Body | Vibe |
|---------|------|------|
| Playfair Display | Source Serif Pro | Classic luxury |
| Cormorant Garamond | Outfit | Modern editorial |
| Fraunces | DM Sans | Warm premium |
| Libre Caslon Display | Karla | Clean editorial |

### Tech / Modern
| Display | Body | Vibe |
|---------|------|------|
| Space Grotesk | DM Sans | Clean tech |
| Satoshi | Inter Tight | Minimal SaaS |
| Clash Display | General Sans | Bold tech |
| Sora | Plus Jakarta Sans | Friendly tech |

### Creative / Experimental
| Display | Body | Vibe |
|---------|------|------|
| Clash Display | Syne | Brutalist |
| Cabinet Grotesk | Space Mono | Creative agency |
| Instrument Serif | Instrument Sans | Editorial-modern |
| Unbounded | Geist | Futuristic |

### Warm / Consumer
| Display | Body | Vibe |
|---------|------|------|
| General Sans | Cabinet Grotesk | Approachable |
| Outfit | Plus Jakarta Sans | Friendly |
| Bricolage Grotesque | Geist | Personality |
| Nunito | Work Sans | Soft consumer |

### Banned Fonts (Never Use as Primary)
Inter, Roboto, Arial, Helvetica, Open Sans, Lato, Montserrat, Poppins (when used generically), Nunito Sans (when used alone)

These fonts signal "default AI output." Use distinctive fonts from the pairings above.

---

## Color Palette Templates

### Cool Tech
- Background: `oklch(0.16 0.01 260)` (deep navy)
- Surface: `oklch(0.20 0.015 260)` (raised panel)
- Text: `oklch(0.95 0.01 260)` (near-white)
- Accent: `oklch(0.72 0.19 195)` (electric cyan)
- Muted: `oklch(0.55 0.03 260)` (secondary text)

### Warm Luxury
- Background: `oklch(0.97 0.01 80)` (warm cream)
- Surface: `oklch(1.0 0 0)` (white cards)
- Text: `oklch(0.20 0.02 50)` (rich charcoal)
- Accent: `oklch(0.55 0.12 55)` (burnished gold)
- Muted: `oklch(0.60 0.02 80)` (stone gray)

### Organic / Health
- Background: `oklch(0.97 0.015 145)` (soft sage tint)
- Surface: `oklch(1.0 0 0)` (white)
- Text: `oklch(0.25 0.03 160)` (deep forest)
- Accent: `oklch(0.65 0.16 155)` (vibrant green)
- Muted: `oklch(0.62 0.04 160)` (dusty sage)

### Bold Creative
- Background: `oklch(0.13 0.005 0)` (near-black)
- Surface: `oklch(0.18 0.005 0)` (dark gray)
- Text: `oklch(0.98 0 0)` (white)
- Accent: `oklch(0.70 0.22 25)` (vivid orange-red)
- Muted: `oklch(0.50 0.02 0)` (mid gray)

### Soft Consumer
- Background: `oklch(0.97 0.02 85)` (warm off-white)
- Surface: `oklch(1.0 0 0)` (white)
- Text: `oklch(0.22 0.02 260)` (dark blue-gray)
- Accent: `oklch(0.62 0.18 310)` (soft violet)
- Muted: `oklch(0.65 0.03 260)` (cool gray)

### Dark Mode Principles
- Never use pure `#000000` — use `oklch(0.14-0.18 ...)` range
- Reduce accent saturation by 10-20% vs light mode
- Surface layers: base < card < elevated (3-step depth)
- Text: primary `oklch(0.93+)`, secondary `oklch(0.65-0.75)`

---

## Advanced Visual Techniques

### Double-Bezel Component
Nested containers creating layered depth:
```html
<div class="rounded-3xl bg-zinc-100 p-2">        <!-- outer bezel -->
  <div class="rounded-2xl bg-white p-6 shadow-sm"> <!-- inner content -->
    ...
  </div>
</div>
```

### Glassmorphism (Done Right)
Not the AI-cliché version. Subtle, purposeful:
```css
background: oklch(1 0 0 / 0.05);
backdrop-filter: blur(20px) saturate(1.2);
border: 1px solid oklch(1 0 0 / 0.1);
```
- Use sparingly — one glass element per viewport, not everything
- Only on fixed/sticky elements for performance
- Always ensure text contrast with fallback background

### Noise & Grain Overlay
Adds analog texture, prevents "flat digital" feel:
```css
.grain::after {
  content: '';
  position: fixed;
  inset: 0;
  opacity: 0.03;
  background-image: url("data:image/svg+xml,..."); /* tiny noise pattern */
  pointer-events: none;
  z-index: 9999;
}
```

### Gradient Mesh Background
Organic, flowing color blobs instead of linear gradients:
```html
<div class="absolute inset-0 overflow-hidden">
  <div class="absolute -top-1/2 -left-1/4 w-[600px] h-[600px] rounded-full bg-violet-500/20 blur-[120px]" />
  <div class="absolute -bottom-1/4 -right-1/4 w-[500px] h-[500px] rounded-full bg-cyan-500/15 blur-[100px]" />
</div>
```

---

## Creative Interaction Patterns

### Magnetic Button
Button content shifts toward cursor on hover:
- Track mouse position relative to button center
- Apply `transform: translate(dx * 0.3, dy * 0.3)` to inner content
- Spring back on mouse leave
- Subtle scale increase: `hover:scale-105`

### Bento Card Spotlight
Mouse-following gradient highlight on cards:
- Track cursor position over card
- Apply radial gradient at cursor position: `radial-gradient(circle at ${x}px ${y}px, oklch(1 0 0 / 0.06), transparent 60%)`
- Smooth transition with `pointer-events: none` on overlay

### Text Masking
Text filled with image or gradient:
```css
background: linear-gradient(135deg, var(--accent), var(--accent-alt));
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
```

### Scroll-Triggered Color Shift
Background color transitions as user scrolls between sections:
- Each section defines its own color scheme
- Intersection Observer detects active section
- CSS custom properties transition: `transition: --bg 0.6s ease`
- Creates seamless thematic progression down the page
