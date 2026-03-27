# Redesign Methodology Reference

## Table of Contents
1. [Scan-Diagnose-Fix Workflow](#scan-diagnose-fix-workflow)
2. [Design Audit Checklist](#design-audit-checklist)
3. [Diagnosis Scoring](#diagnosis-scoring)
4. [Implementation Priority](#implementation-priority)
5. [Stack-Agnostic Approach](#stack-agnostic-approach)

---

## Scan-Diagnose-Fix Workflow

### Phase 1: Scan
Read the entire codebase or provided code. Identify:
- Tech stack (framework, CSS approach, component library)
- File structure and component organization
- Current fonts, colors, spacing values
- Existing animations or transitions
- Third-party dependencies

### Phase 2: Diagnose
Audit against the checklist below. Score each category. Identify the weakest areas.

### Phase 3: Fix
Apply targeted improvements in priority order. Preserve the existing stack. Never suggest a full rewrite unless explicitly requested.

---

## Design Audit Checklist

### Typography (Critical)
| Issue | Sign of AI/Generic | Fix |
|-------|-------------------|-----|
| Browser default fonts | No `font-family` set, or using Inter/Roboto/Arial | Replace with distinctive font pairing |
| Flat hierarchy | All headings similar size | Establish clear scale (3:1 ratio hero to body) |
| Poor line height | Too tight or too loose | Body: 1.5-1.6, headings: 1.1-1.2 |
| No font weight variation | Everything `font-normal` | Use bold for headings, medium for labels, regular for body |
| Long line length | Text stretches full width | Max `max-w-prose` (65ch) for reading content |
| Oversized H1 | `text-7xl` or larger hero text | Cap at `text-5xl`/`text-6xl`, use weight for emphasis |

### Color (Critical)
| Issue | Sign | Fix |
|-------|------|-----|
| Pure black background | `#000000` or `bg-black` | Use `bg-zinc-950` or `oklch(0.14 ...)` |
| Oversaturated accent | Neon blue/green/purple | Reduce chroma, use muted tones |
| Purple-to-blue gradient | Classic AI gradient hero | Replace with brand-specific color story |
| No color system | Random hex values everywhere | Establish semantic tokens (see components.md) |
| Missing dark mode | Only light theme | Add dark variant with proper mapping |
| Low contrast text | Light gray on white | Ensure 4.5:1 minimum for body text |

### Layout (High)
| Issue | Sign | Fix |
|-------|------|-----|
| Three equal columns | Identical cards in a row | Asymmetric bento grid or varied sizing |
| Perfect symmetry | Everything centered and balanced | Introduce asymmetry, offset elements |
| `height: 100vh` everywhere | Every section exactly one screen | Vary section heights, use `min-h` if needed |
| No whitespace variation | Uniform `p-4` or `p-6` everywhere | Increase section spacing (`py-24`+), vary rhythm |
| Content max-width too wide | Text stretching to 1400px+ | `max-w-4xl` for text, `max-w-7xl` for full layouts |

### Interactivity (High)
| Issue | Sign | Fix |
|-------|------|-----|
| No hover states | Buttons/links have no hover feedback | Add `hover:` transitions on all clickable elements |
| Missing active feedback | No press/click visual response | Add `active:scale-[0.98]` on buttons |
| Instant transitions | State changes happen abruptly | Add `transition-all duration-200 ease-out` |
| No focus rings | Tab navigation shows nothing | Add `focus-visible:ring-2 focus-visible:ring-accent/50` |
| Missing cursor | Clickable elements use default cursor | Add `cursor-pointer` on all interactive elements |

### Content (Medium)
| Issue | Sign | Fix |
|-------|------|-----|
| Placeholder names | "John Doe", "Jane Smith", "Acme Corp" | Use contextually appropriate, diverse names |
| Fake metrics | "10,000+ users", "99.9% uptime", round numbers | Use realistic, specific numbers or remove |
| AI cliché copy | "Elevate", "Seamless", "Next-Gen", "Revolutionize" | Write specific, concrete benefit statements |
| Lorem ipsum | Any placeholder latin text | Write real content or realistic placeholder |
| Stock photo heroes | Generic Unsplash hero images | Use illustrations, gradients, or product screenshots |

### Component Patterns (Medium)
| Issue | Sign | Fix |
|-------|------|-----|
| Generic cards | Box + icon + title + description, repeated identically | Vary card sizes, content, visual treatment |
| Emoji as icons | 🚀 ✨ 💡 used as feature icons | Use SVG icon library (Lucide, Heroicons) |
| Pricing table cliché | 3 columns, "Basic/Pro/Enterprise" | Redesign with unique layout, highlight recommended |
| Missing states | No loading, error, or empty states | Add all states (see ux-states.md) |

---

## Diagnosis Scoring

Rate each category:

| Score | Meaning | Action |
|-------|---------|--------|
| 1 — Critical | Looks obviously AI-generated or broken | Fix immediately |
| 2 — Poor | Functional but generic, no design thought | High priority fix |
| 3 — Acceptable | Works, some personality, minor issues | Improve when time allows |
| 4 — Good | Solid design, few issues | Polish only |
| 5 — Excellent | Distinctive, professional quality | Maintain |

Focus effort on categories scoring 1-2.

---

## Implementation Priority

Fix in this order for maximum visual impact with minimum effort:

1. **Font replacement** — Single highest-impact change. Swap to distinctive font pairing.
2. **Color refinement** — Establish proper palette, fix contrast, remove neon/oversaturation.
3. **Spacing rhythm** — Increase section spacing, add whitespace variation.
4. **Interactive states** — Add hover, active, focus on all interactive elements.
5. **Layout restructure** — Break symmetry, introduce bento/asymmetric grids.
6. **Component upgrade** — Replace generic cards, add missing states.
7. **Typography polish** — Fine-tune sizes, weights, line heights, letter spacing.
8. **Animation layer** — Add entrance animations, scroll reveals, micro-interactions.

### Quick Wins (< 5 minutes each)
- Change font family (1 line in CSS)
- Add `transition-colors duration-200` to all buttons
- Set `cursor-pointer` on clickable elements
- Replace `bg-black` with `bg-zinc-950`
- Add `hover:brightness-110` or `hover:bg-accent/90` to primary buttons
- Set `max-w-prose` on long text blocks

---

## Stack-Agnostic Approach

### Rules
- **Never suggest framework migration** unless explicitly asked
- **Work within existing CSS approach** (Tailwind, CSS modules, styled-components)
- **Preserve all existing functionality** — visual changes only
- **Check dependency versions** before using features (e.g., Tailwind v3 vs v4 syntax)
- **Test after each change** — ensure nothing breaks

### Adaptation by Stack
| Stack | How to Apply |
|-------|-------------|
| Tailwind | Direct utility class changes |
| CSS Modules | Update `.module.css` files with new values |
| styled-components | Update template literals with new values |
| Plain CSS | Update CSS custom properties and rules |
| Bootstrap | Override via custom CSS, replace components gradually |
