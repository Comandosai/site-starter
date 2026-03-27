# UX States Reference

## Table of Contents
1. [Loading State Decision Tree](#loading-state-decision-tree)
2. [Skeleton vs Spinner](#skeleton-vs-spinner)
3. [Error Handling Hierarchy](#error-handling-hierarchy)
4. [Empty States](#empty-states)
5. [Button & Form States](#button--form-states)
6. [Optimistic Updates](#optimistic-updates)
7. [State Checklist](#state-checklist)

---

## Loading State Decision Tree

Apply this logic for every data-dependent component:

```
Is there an error?
  → YES → Show error state (see Error Handling)
  → NO ↓

Is data loading AND no cached/previous data exists?
  → YES → Show skeleton or spinner (see below)
  → NO ↓

Is data loaded AND items array is empty?
  → YES → Show empty state (see Empty States)
  → NO ↓

Data exists → Render normally
```

**Golden rule**: Show loading indicator ONLY when there is no data to display. If previous data exists, show stale data with a subtle refresh indicator (not a full skeleton).

---

## Skeleton vs Spinner

### Use Skeleton When:
- Content shape is known (lists, cards, profiles, articles)
- Initial page load or first data fetch
- Multiple content blocks loading simultaneously
- Layout stability matters (prevents CLS)

### Use Spinner When:
- Content shape is unknown (search results, dynamic modals)
- Inline operations (button click, form submit)
- Short operations (< 1 second expected)
- Single small element loading

### Skeleton Implementation
```jsx
// Skeleton that matches actual content shape
<div class="animate-pulse space-y-4">
  <div class="h-48 bg-zinc-200 rounded-2xl" />        {/* image */}
  <div class="h-5 bg-zinc-200 rounded w-3/4" />        {/* title */}
  <div class="h-4 bg-zinc-200 rounded w-1/2" />        {/* subtitle */}
  <div class="space-y-2">
    <div class="h-3 bg-zinc-200 rounded" />             {/* text line */}
    <div class="h-3 bg-zinc-200 rounded w-5/6" />       {/* text line */}
  </div>
</div>
```

- Match skeleton shapes to actual content dimensions
- Use `rounded` matching actual component corners
- Pulse animation: `animate-pulse` or custom shimmer gradient
- Dark mode: `bg-zinc-800` instead of `bg-zinc-200`

### Spinner Rules
- Size relative to context: 16px inline, 24px button, 40px page-level
- Use `currentColor` so it inherits text color
- Never show spinner for less than 300ms (use delayed appearance)
- Accessibility: `role="status"` + `aria-label="Loading"`

---

## Error Handling Hierarchy

From least disruptive to most. Choose the minimum level needed.

### Level 1: Inline Error
For field-level or component-level failures.
```jsx
<p class="text-sm text-red-500 mt-1">Email address is invalid</p>
```
- Place directly below the affected element
- Red text, small size, icon optional
- Use for: form validation, single field errors

### Level 2: Toast Notification
For non-critical operations that failed.
```jsx
<div class="fixed bottom-4 right-4 bg-red-50 border border-red-200 rounded-xl p-4 shadow-lg">
  <p class="text-red-800 text-sm font-medium">Failed to save changes</p>
  <button class="text-red-600 text-sm underline">Retry</button>
</div>
```
- Auto-dismiss after 5-8 seconds (with pause on hover)
- Always include retry action if applicable
- Use for: save failures, API timeouts, background sync errors

### Level 3: Banner
For section-level failures or degraded functionality.
```jsx
<div class="bg-amber-50 border-b border-amber-200 px-4 py-3">
  <p class="text-amber-800 text-sm">Unable to load recent activity. <button>Retry</button></p>
</div>
```
- Placed at top of affected section
- Amber for warnings, red for errors
- Use for: partial page load failures, feature degradation

### Level 4: Full Error Screen
For page-level failures where nothing can render.
```jsx
<div class="min-h-[60vh] flex items-center justify-center">
  <div class="text-center space-y-4">
    <div class="text-6xl"><!-- error illustration --></div>
    <h2 class="text-xl font-semibold">Something went wrong</h2>
    <p class="text-zinc-500 max-w-md">We couldn't load this page. Please try refreshing.</p>
    <button class="px-6 py-2.5 bg-zinc-900 text-white rounded-xl">Refresh page</button>
  </div>
</div>
```
- Centered vertically, clear message, prominent action
- Never show raw error messages or stack traces to users
- Use for: 404, 500, complete API failure, auth errors

**Critical rule**: Never swallow errors silently. Every async operation must have an `onError` handler that shows user-facing feedback.

---

## Empty States

Every collection, list, table, or feed MUST have a designed empty state.

### Required Elements
1. **Visual** — illustration, icon, or subtle graphic (not emoji)
2. **Message** — what this section will contain when populated
3. **Action** — primary CTA to populate (create first item, import, invite)

### Contextual Empty States
Different empty messages based on context:
- **First-time**: "Create your first project to get started" + Create button
- **Filter result**: "No results match your filters" + Clear filters button
- **Search result**: "No results for '{query}'" + suggestion or Clear button
- **Deleted all**: "All items archived" + View archive link

### Implementation
```jsx
<div class="flex flex-col items-center justify-center py-16 px-4">
  <div class="w-16 h-16 rounded-2xl bg-zinc-100 flex items-center justify-center mb-4">
    <svg class="w-8 h-8 text-zinc-400"><!-- contextual icon --></svg>
  </div>
  <h3 class="text-lg font-semibold text-zinc-900 mb-1">No projects yet</h3>
  <p class="text-sm text-zinc-500 text-center max-w-sm mb-6">
    Create your first project to start organizing your work.
  </p>
  <button class="px-4 py-2 bg-zinc-900 text-white text-sm rounded-xl hover:bg-zinc-800 transition-colors">
    Create project
  </button>
</div>
```

---

## Button & Form States

### Button States
Every button must handle these states:

| State | Visual | Behavior |
|-------|--------|----------|
| Default | Normal styling | Clickable |
| Hover | Subtle shift (darken, scale 1.02, shadow) | Cursor pointer |
| Active/Pressed | Slight scale down (0.98) | Visual feedback |
| Loading | Spinner replaces or sits beside label | Disabled, no double-submit |
| Disabled | Reduced opacity (0.5), muted colors | `cursor-not-allowed`, no events |

### Loading Button Pattern
```jsx
<button disabled={isLoading} class="relative ...">
  {isLoading && <Spinner class="absolute left-4 w-4 h-4" />}
  <span class={isLoading ? "opacity-0" : ""}>{label}</span>
</button>
```
- Keep button width stable during loading (prevent layout shift)
- Disable immediately on click to prevent double-submission
- Re-enable on success or error

### Form Submission Flow
```
User clicks submit
  → Validate all fields client-side
    → Errors? Show inline errors, focus first error field
    → Valid? ↓
  → Set button to loading state
  → Disable all form inputs
  → Send request
    → Success? Show success feedback (toast or redirect)
    → Error? Re-enable form, show error toast, keep user's input
```

---

## Optimistic Updates

Update UI immediately, then sync with server.

### When to Use
- Low-failure-rate operations (likes, toggles, reordering)
- Operations where delay feels sluggish
- When server response doesn't change what was sent

### When NOT to Use
- Payment or financial operations
- Destructive actions (delete)
- Operations requiring server validation (unique checks)

### Pattern
```
User action → Update UI immediately → Send request in background
  → Success? Do nothing (UI already correct)
  → Error? Revert UI to previous state + show error toast
```

---

## State Checklist

Before completing ANY UI component, verify:

- [ ] Loading state: skeleton or spinner shown when data is fetching
- [ ] Error state: user-facing message with retry action
- [ ] Empty state: designed empty view with contextual CTA
- [ ] Button hover: every clickable element has hover feedback
- [ ] Button loading: async buttons show loading and prevent double-click
- [ ] Form validation: inline errors on invalid fields
- [ ] Focus state: visible focus ring on all interactive elements
- [ ] Disabled state: reduced opacity, `cursor-not-allowed`
- [ ] Transition: state changes animate smoothly (150-300ms)
