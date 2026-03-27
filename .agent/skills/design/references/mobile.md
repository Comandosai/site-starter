# Mobile-First Design Reference

## Table of Contents
1. [Mobile-First Principles](#mobile-first-principles)
2. [Touch Targets & Fitts' Law](#touch-targets--fitts-law)
3. [Thumb Zone Mapping](#thumb-zone-mapping)
4. [Platform Divergence Matrix](#platform-divergence-matrix)
5. [Mobile Typography & Color](#mobile-typography--color)
6. [Mobile Performance](#mobile-performance)
7. [Mobile Security](#mobile-security)
8. [Mobile Checklist](#mobile-checklist)

---

## Mobile-First Principles

Mobile is NOT "shrink the desktop." It's the primary design target.

1. **Design for smallest screen first** — then expand for larger viewports
2. **Touch-first** — every interaction must work with a finger, not a cursor
3. **One hand** — primary actions reachable with one thumb
4. **Content priority** — show less, show better. Cut before you shrink.
5. **Network-aware** — assume 3G, optimize for slow connections
6. **Battery-conscious** — minimize animations, GPS, background processes on mobile

### Viewport Setup
```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
```
- Use `100dvh` not `100vh` (accounts for mobile browser chrome)
- Use `env(safe-area-inset-*)` for notch/island devices

---

## Touch Targets & Fitts' Law

### Minimum Sizes
| Element | Min Size | Min Spacing |
|---------|----------|-------------|
| Buttons, links | 44 × 44px | 8px between targets |
| Icon buttons | 48 × 48px | 8px between targets |
| Form inputs | 48px height | 12px between fields |
| List items (tappable) | 48px height | 0 (divider serves as spacing) |

### Implementation
```html
<!-- Even if the icon is 24px, the tap area must be 44px+ -->
<button class="p-3">  <!-- 24px icon + 12px*2 padding = 48px tap area -->
  <svg class="w-6 h-6">...</svg>
</button>
```

### Fitts' Law Applied
- **Larger targets are easier to hit** — make primary CTAs the biggest tappable element
- **Edge/corner targets are faster** — place key actions at screen edges (bottom nav, FABs)
- **Distance matters** — related actions should be close together, destructive actions far from frequent ones

---

## Thumb Zone Mapping

```
┌─────────────────────┐
│   HARD TO REACH     │  ← Navigation, settings, search
│                     │
├─────────────────────┤
│                     │
│   NATURAL REACH     │  ← Content, scrollable lists
│                     │
├─────────────────────┤
│                     │
│   EASY REACH        │  ← Primary CTA, bottom nav, FAB
│                     │
└─────────────────────┘
    👍 Thumb position
```

### Design Implications
- **Bottom navigation** > hamburger menu for frequent actions
- **Primary CTA** at bottom of screen, not top
- **FAB (Floating Action Button)** bottom-right for primary creation action
- **Swipe actions** (archive, delete) on list items — natural thumb motion
- **Pull to refresh** — natural downward thumb pull
- **Avoid**: fixed headers with important interactive elements

---

## Platform Divergence Matrix

| Pattern | iOS | Android | Unify? |
|---------|-----|---------|--------|
| Navigation | Tab bar (bottom) | Bottom nav / drawer | ✅ Unify on bottom |
| Back action | Swipe right edge | System back button | ❌ Keep platform default |
| Typography | SF Pro (system) | Roboto (system) | ✅ Use system font stack |
| Modals | Sheet from bottom | Dialog or bottom sheet | ✅ Bottom sheet both |
| Pull to refresh | Native bounce | Material indicator | ❌ Keep platform default |
| Switches | iOS-style toggle | Material switch | Depends on brand |
| Status bar | Light/dark content | Colored / transparent | ❌ Platform specific |
| Haptics | Taptic Engine | Vibration API | ✅ Both support it |

### System Font Stack
```css
font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
```

### When to Diverge
- Users expect platform-specific behavior (back gesture, status bar)
- Accessibility features tied to platform (Dynamic Type on iOS, TalkBack on Android)

### When to Unify
- Brand identity is more important than platform convention
- Cross-platform consistency for your product's identity
- Bottom navigation pattern works universally

---

## Mobile Typography & Color

### Typography
- **Minimum body text**: 16px (prevents iOS auto-zoom on inputs)
- **Line height**: 1.5-1.6 for body (more generous than desktop)
- **Heading scale**: Reduce desktop headings by 25-40% on mobile
- **Line length**: Max 35-40 characters on mobile (narrower than desktop's 65-75)
- **Dynamic Type (iOS)**: Support it if building native — use relative sizes

### Color for Mobile
- **OLED optimization**: True blacks save battery on OLED screens — but use `#0a0a0a` not `#000`
- **Outdoor readability**: Higher contrast than desktop (sunlight conditions)
- **Dark mode**: Should be automatic based on system preference
- **Accent colors**: Slightly more saturated on small screens to remain visible
- **Touch feedback**: Distinct tap highlight color (`-webkit-tap-highlight-color`)

---

## Mobile Performance

### Critical Rules
- **First Contentful Paint**: Target < 1.5s on 3G
- **Total page weight**: Target < 500KB on initial load
- **Images**: Always use responsive `srcset`, serve WebP/AVIF, lazy load below fold
- **Fonts**: Max 2 font files on mobile, prefer variable fonts

### React Native / Mobile App Specific
| Do | Don't |
|----|-------|
| `FlatList` for long lists | `ScrollView` for long lists |
| `React.memo` for list items | Re-render entire list on change |
| `useNativeDriver: true` for animations | JS-driven animation on main thread |
| Cache API responses | Fetch on every screen mount |
| `FlashList` for 500+ items | `FlatList` for very large lists |

### Reduce Motion on Low-End Devices
```js
// Detect low-end device
const isLowEnd = navigator.hardwareConcurrency <= 4 || navigator.deviceMemory <= 4;
if (isLowEnd) {
  // Disable parallax, reduce animation complexity
}
```

---

## Mobile Security

- **Token storage**: Use `SecureStore` (Expo) or Keychain (iOS) / Keystore (Android). Never `AsyncStorage` for tokens.
- **No hardcoded keys**: API keys must come from environment variables or secure config
- **SSL pinning**: Implement for sensitive API communications
- **Biometric auth**: Use `LocalAuthentication` for sensitive actions
- **Deep links**: Validate all deep link parameters — treat as untrusted input
- **Clipboard**: Never auto-copy sensitive data. Clear clipboard after paste of sensitive data.

---

## Mobile Checklist

### Before Every Screen
- [ ] All tap targets ≥ 44px
- [ ] Primary action in thumb-friendly zone (bottom)
- [ ] Text ≥ 16px body, adequate line-height
- [ ] Content readable without horizontal scrolling
- [ ] Safe area insets respected (notch/island)
- [ ] `100dvh` used instead of `100vh`

### Before Release
- [ ] Tested on real device (not just simulator)
- [ ] Tested on mid-range Android (not just latest iPhone)
- [ ] Tested on slow network (3G throttle)
- [ ] Dark mode works and respects system preference
- [ ] Keyboard doesn't cover input fields
- [ ] Pull-to-refresh works where expected
- [ ] Back navigation works correctly
- [ ] No horizontal overflow on any screen
