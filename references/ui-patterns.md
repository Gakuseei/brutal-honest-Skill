# UI Patterns 2026 - Pro Level

## Layout Architecture

### Bento Grid (Asymmetric - Apple Style)
Not symmetrical. Visual hierarchy through size.
```css
.bento {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
}
.bento > *:nth-child(1) { grid-column: span 2; grid-row: span 2; } /* Hero */
.bento > *:nth-child(4) { grid-column: span 2; } /* Wide item */
```

### View Transitions API (2026 Standard)
```js
// Before navigation
document.startViewTransition(() => updateDOM());
```
```css
::view-transition-old(root) { animation: slide-out 0.3s; }
::view-transition-new(root) { animation: slide-in 0.3s; }
```

### Anchor Positioning (Popovers without JS)
```css
.anchor { anchor-name: --tooltip; }
.tooltip {
  position: absolute;
  position-anchor: --tooltip;
  inset-area: bottom;
}
```

### CSS 2026 Features
```css
/* CSS if() function — conditional values */
.card { padding: if(style(--compact: true), 0.5rem, 1.5rem); }

/* sibling-index() — positional styling without nth-child hacks */
.item { opacity: calc(1 - sibling-index() * 0.1); }

/* Scroll State Queries — style based on scroll position */
@container scroll-state(scrollable: top) {
  .header { box-shadow: none; }
}

/* corner-shape — squircle and other shapes */
.card { corner-shape: superellipse; border-radius: 1rem; }

/* Customizable <select> — native dropdown styling */
select, select::picker(select) {
  appearance: base-select;
}
select::picker(select) {
  background: var(--surface);
  border: 1px solid var(--border);
}
```

## Visual Hierarchy

### 60-30-10 Color Rule
- 60% Neutral (Background)
- 30% Primary (Cards, sections)
- 10% Accent (CTAs, highlights)
- **Check:** Never more than 3 main colors

### Typography Scale (Strict)
```css
h1: clamp(2.5rem, 5vw, 4rem)     /* Hero */
h2: clamp(1.5rem, 3vw, 2.5rem)   /* Section */
h3: 1.25rem                       /* Card Title */
body: 1rem                        /* Base */
small: 0.875rem                   /* Meta */
```
**Rule:** Max 6 sizes, prefer rem over px.

### Spacing System (4px Grid)
```css
--space-1: 0.25rem;  /* 4px */
--space-2: 0.5rem;   /* 8px */
--space-3: 1rem;     /* 16px */
--space-4: 1.5rem;   /* 24px */
--space-5: 2rem;     /* 32px */
--space-6: 3rem;     /* 48px */
```
**Check:** Every spacing should be on a consistent scale.

## Interaction Design

### Micro-Interactions (0.1s-0.2s)
```css
.button {
  transition: transform 0.15s cubic-bezier(0.4, 0, 0.2, 1),
              background 0.2s ease;
}
.button:hover { transform: translateY(-1px); }
.button:active { transform: scale(0.97); }
```
**Rule:** Never more than 300ms, never linear easing.

### Loading States (No Spinner Spam)
**Skeleton > Spinner > Blank**
```html
<!-- Good Skeleton: Content shape visible -->
<div class="skeleton" style="height: 1em; width: 60%;"></div>
```

### Focus States (WCAG 2.2 AAA)
```css
:focus-visible {
  outline: 3px solid currentColor;
  outline-offset: 3px;
  border-radius: 2px;
}
```
**Critical:** Without focus-visible it's an accessibility fail in 2026.

## AI-Native UI (applies to AI chat applications only)

### Chat Interface Architecture
```html
<div class="chat-container">
  <div class="message-list">
    <div class="message-group" data-user="human">
      <div class="message-content">...</div>
    </div>
    <div class="message-group" data-user="ai">
      <div class="ai-header"><!-- model, timestamp --></div>
      <div class="streaming-content"><!-- Streaming dots during generation --></div>
      <div class="ai-actions">
        <button class="copy-btn">Copy</button>
        <button class="regenerate-btn">Regenerate</button>
      </div>
    </div>
  </div>
  <div class="composer">
    <div class="attachment-dropzone"></div>
    <textarea autoresize></textarea>
    <button class="send-btn" disabled>Send</button>
  </div>
</div>
```

### Streaming States
- **Pending:** Pulsing dot (not Spinner)
- **Streaming:** Typing indicator (3 animated dots)
- **Complete:** Fade-in of Actions (Copy, etc.)

### AI Content Markers
```css
[data-content-author="ai"] {
  border-left: 3px solid var(--ai-accent);
  background: var(--ai-surface);
}
[data-content-author="user"] {
  border-left: 3px solid transparent;
}
```

## Mobile UI Patterns (applies to native/hybrid mobile apps)

### Touch Targets
- Minimum 44x44pt (iOS) / 48x48dp (Android) tap targets
- Adequate spacing between interactive elements (8dp minimum)

### Safe Areas
- Respect device safe areas (notch, home indicator, status bar)
- Content should never be clipped by hardware features

### Navigation
- Bottom navigation for top-level destinations (max 5 items)
- Back gesture / swipe-to-go-back support
- Tab bar stays visible during scroll

### Platform Conventions
- iOS: Large titles, SF Symbols, swipe actions, haptic feedback on meaningful actions
- Android: Material 3 tokens, FAB for primary action, edge-to-edge layout, predictive back gesture

### Performance
- 60fps scrolling — no jank during list rendering
- Pull-to-refresh for refreshable content
- Optimistic UI updates for user actions
- Skeleton screens during data loading (not spinners)

---

## Common Anti-Patterns (context-dependent)

**Always wrong:**
- Rainbow Gradients (>2 colors in a gradient)
- Box-Shadow on every static element (only on hover/active)
- Border-Radius 50% on rectangles
- Placeholder text as label (accessibility violation)

**Context-dependent — NOT always wrong:**
- **System fonts vs custom fonts:** System font stacks (`system-ui, -apple-system, sans-serif`) have zero FOIT/FOUT, zero CLS, zero network requests, and instant rendering. They are a valid choice for performance-critical apps, dashboards, and tools. Custom fonts (Inter, Geist, etc.) are better for brand-heavy marketing sites and design-forward apps. Neither is universally "correct."
- **Infinite scroll vs pagination:** Virtual/infinite scroll is standard and expected for social feeds, chat interfaces, activity logs, and real-time data streams. Pagination is better for searchable, filterable, bookmarkable content (e-commerce, documentation, search results). "Load More" buttons are a middle ground. Judge by use case, not by blanket rule.

## Checklist for UI Review

- [ ] 60-30-10 color rule followed?
- [ ] Typography scale consistent?
- [ ] All spacing on a consistent scale?
- [ ] View Transitions on navigation? (where supported)
- [ ] Focus states visible?
- [ ] Loading states better than spinners?
- [ ] Micro-animations < 200ms?
- [ ] Dark mode / system preference respected?
- [ ] No accessibility violations in interactive elements?
- [ ] Font strategy appropriate for the project type?

**Tone for UI failures:**
- "This looks like a template from 2018"
- "Color palette is giving migraine"
- "Spacing is random, not systematic"
- "Accessibility: Screen reader users will have a terrible experience"
