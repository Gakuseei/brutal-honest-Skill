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
h3: 1.25rem                        /* Card Title */
body: 1rem                         /* Base */
small: 0.875rem                    /* Meta */
```
**Rule:** Max 6 sizes, never px, always rem.

### Spacing System (4px Grid)
```css
--space-1: 0.25rem;  /* 4px */
--space-2: 0.5rem;   /* 8px */
--space-3: 1rem;     /* 16px */
--space-4: 1.5rem;   /* 24px */
--space-5: 2rem;     /* 32px */
--space-6: 3rem;     /* 48px */
```
**Check:** Every spacing must be divisible by 4.

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
```tsx
// Good Skeleton: Content shape visible
<div className="skeleton" style={{ height: '1em', width: '60%' }} />
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

## AI-Native UI (2026 Standard)

### Chat Interface Architecture
```tsx
<ChatContainer>
  <MessageList>
    <MessageGroup user="human">
      <MessageContent />
    </MessageGroup>
    <MessageGroup user="ai">
      <AIHeader model="gpt-4" timestamp />
      <StreamingContent /> {/* Streaming dots during generation */}
      <AIActions>
        <CopyButton />
        <RegenerateButton />
        <VariantButton /> {/* A/B Variants */}
      </AIActions>
    </MessageGroup>
  </MessageList>
  <Composer>
    <AttachmentDropzone />
    <Textarea autoResize />
    <ModelSelector />
    <SendButton disabled={!input || isLoading} />
  </Composer>
</ChatContainer>
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

## Common Anti-Patterns (What must be destroyed)

❌ **Rainbow Gradients:** More than 2 colors in gradient = 2019
❌ **Box-Shadow everywhere:** Only on Hover/Active, never static
❌ **Border-Radius 50% on rectangles:** Always looks like shit
❌ **System Fonts:** Inter/Geist are 2026 standard
❌ **Placeholder text as label:** Accessibility disaster
❌ **Infinite Scroll:** Pagination or "Load More" is better

## Checklist for UI Review

- [ ] 60-30-10 color rule followed?
- [ ] Typography scale consistent?
- [ ] All spacing divisible by 4?
- [ ] View Transitions on navigation?
- [ ] Anchor Positioning for tooltips?
- [ ] Focus states visible?
- [ ] Loading States > Spinner?
- [ ] Micro-animations < 200ms?
- [ ] Dark Mode system preference?
- [ ] No rainbow gradients?

**Tone for UI failures:**
- "This looks like a Bootstrap template from 2018"
- "Color palette is giving migraine"
- "Spacing is random, not systematic"
- "Accessibility: Blind users will sue you"
