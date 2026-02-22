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
// Same-document (Level 1)
document.startViewTransition(() => updateDOM());
```
```css
::view-transition-old(root) { animation: slide-out 0.3s; }
::view-transition-new(root) { animation: slide-in 0.3s; }

/* Cross-document (Level 2) — CSS-only, no JS needed (Interop 2026) */
@view-transition { navigation: auto; }
::view-transition-group(hero) { animation-duration: 0.4s; }
```

### Anchor Positioning (Popovers without JS)
Interop 2026 focus area.
```css
.anchor { anchor-name: --tooltip; }
.tooltip {
  position: absolute;
  position-anchor: --tooltip;
  position-area: bottom;
  position-try-fallbacks: flip-block, flip-inline;
}
```

---

## Anti-AI-Slop Checklist

AI-generated websites have a recognizable aesthetic. If a site triggers 3+ of these red flags, it looks like AI slop regardless of code quality.

### Red Flags (AI-generated tells)
- **Purple/indigo gradient** as primary color scheme (the #1 AI default)
- **Single sans-serif font** for everything — Inter, Roboto, or Poppins with no pairing
- **Three-column icon grid** layout for features/benefits (the "three cards" pattern)
- **Generic CTAs** — "Sign Up", "Learn More", "Get Started" with no specificity
- **Uniform rounded corners** on every element (same border-radius everywhere)
- **0.1 opacity box-shadows** on every card/surface (the "floating everything" look)
- **Beige + teal** or **purple + mint** color combos (AI favorites)
- **No semantic color system** — raw hex values scattered through CSS
- **Generic stock illustrations** — flat corporate / Undraw / Humaaans style
- **Copy that could describe any business** — "We help you achieve your goals" / "Empowering innovation"
- **Perfectly symmetrical layouts** — no visual tension, no hierarchy, everything the same weight
- **Gratuitous gradients on text** — gradient headings with no brand justification

### Professional Signals (what good sites do)
- **Intentional font pairing** — serif + sans-serif, or display + body (see Typography section)
- **Semantic color system** via CSS custom properties (see Color System section)
- **8px spacing grid** with consistent scale
- **Clear visual hierarchy** — one focal point per section, not everything competing
- **Benefit-driven CTA language** — "Start your free trial" not "Sign Up", "See pricing" not "Learn More"
- **Social proof near decision points** — logos, testimonials, trust badges placed strategically
- **Mobile-first responsive** — not just "it doesn't break on mobile"
- **Fast loading** — under 3s on 3G, optimized assets
- **Purposeful animation** — motion serves UX (feedback, orientation), not decoration
- **Each page section has one job** — hero sells, features prove, testimonials validate, CTA converts

### Landing Page Above-the-Fold Checklist
Every landing page hero needs exactly these 5 elements:
1. **Outcome-first headline** — what the user gets, not what the product does
2. **One supporting line** — clarifies or qualifies the headline
3. **One primary CTA** with a specific verb (not "Submit" or "Learn More")
4. **One trust cue** — logo bar, user count, review score, or "No credit card required"
5. **One compelling visual** — product screenshot, demo, or illustration that reinforces the headline

---

## CSS 2026 Features

### Production-Ready (use now — Baseline Widely Available or cross-browser)

```css
/* CSS Nesting — native, no preprocessor needed (Baseline Widely Available, 82%+) */
.card {
  background: var(--surface);
  & .title { font-weight: 700; }
  &:hover { box-shadow: 0 4px 12px oklch(0 0 0 / 0.1); }
}

/* @scope — scoped styles without BEM or CSS Modules (Baseline Newly Available) */
@scope (.card) to (.card-footer) {
  p { margin-block: 0.5rem; }
}

/* @starting-style — entry animations from display:none (Baseline Newly Available) */
dialog[open] {
  opacity: 1; transform: scale(1);
  transition: opacity 0.3s, transform 0.3s, display 0.3s allow-discrete;
  @starting-style { opacity: 0; transform: scale(0.95); }
}

/* light-dark() — one-line dark mode values (all browsers since May 2024) */
:root { color-scheme: light dark; }
body {
  background: light-dark(#fff, #1a1a2e);
  color: light-dark(#1a1a2e, #e0e0e0);
}

/* color-mix() — color manipulation without Sass (Baseline Widely Available) */
.button:hover {
  background: color-mix(in oklch, var(--accent), black 15%);
}

/* Relative color syntax — derive colors from a base (Baseline cross-browser) */
.card {
  --hover-bg: oklch(from var(--brand) calc(l + 0.1) c h);
}

/* Container queries (size) — component-level responsive (Baseline Widely Available, 95%+) */
.card-container { container-type: inline-size; }
@container (min-width: 400px) {
  .card { flex-direction: row; }
}

/* Popover API — native popovers with light-dismiss, no JS (Baseline Widely Available) */
/* <button popovertarget="menu">Open</button> */
/* <div popover id="menu">...</div> */
[popover] {
  &:popover-open { opacity: 1; transform: translateY(0); }
  @starting-style { opacity: 0; transform: translateY(-8px); }
}

/* Scroll-driven animations — CSS-only scroll effects (Interop 2026) */
.progress-bar {
  animation: grow-width linear;
  animation-timeline: scroll();
}
@keyframes grow-width { from { width: 0; } to { width: 100%; } }
```

### Emerging (progressive enhancement only — limited browser support)

```css
/* CSS if() — conditional values (Chrome Canary only, behind flag) */
.card { padding: if(style(--compact: true), 0.5rem, 1.5rem); }

/* sibling-index() / sibling-count() — positional styling (no Firefox yet) */
.item { opacity: calc(1 - sibling-index() * 0.1); }

/* corner-shape — squircle borders (Chrome Canary only) */
.card { corner-shape: superellipse; border-radius: 1rem; }

/* Customizable <select> — native dropdown styling (limited support) */
select, select::picker(select) { appearance: base-select; }

/* Container style queries — style-based container conditions (partial support) */
@container style(--theme: dark) { .card { background: #1a1a2e; } }

/* field-sizing: content — auto-sizing inputs/textareas (Chromium only, progressive enhancement) */
textarea { field-sizing: content; min-height: 3lh; max-height: 10lh; }

/* text-box-trim — trim typographic whitespace (Chrome + Safari) */
h1 { text-box-trim: both; text-box-edge: cap alphabetic; }

/* contrast-color() — automatic accessible contrast (spec draft, no shipping browser) */
.badge { background: var(--brand); color: contrast-color(var(--brand)); }
```

**Rule:** Production-Ready features should be used in production code. Emerging features need `@supports` checks or are Chromium-only progressive enhancements. Never depend on Emerging features for core functionality.

---

## Landing Page Patterns

### Hero Section
- **Bold typography as dominant element** — the headline does the heavy lifting, not a background image
- **Single focused CTA** — one primary action, optionally one secondary (ghost/text link)
- **One trust cue below CTA** — "No credit card required", "Used by 10,000+ teams", or logo bar
- **Whitespace** — hero needs breathing room; cramped heroes feel desperate

### Section Rhythm
**"One job per section" rule:**
1. **Hero** — sells the outcome (what the user gets)
2. **Social proof** — validates with logos, stats, or a quote
3. **Features/Benefits** — proves the hero's promise (how it works)
4. **Deeper proof** — testimonials with photos, case studies, or live demos
5. **CTA repeat** — re-state the offer after building the case
6. **FAQ / Objections** — handle remaining doubts
7. **Final CTA** — last conversion opportunity

### CTA Design
- **Complementary color contrast** — CTA should visually pop against its section background
- **Benefit-driven language** — "Start building for free" not "Sign Up"
- **Breathing room** — minimum 24px padding around CTA buttons
- **Repeat after persuasion blocks** — CTA appears after features, after testimonials, at footer

### Social Proof Placement
- **Logo bar** directly below hero (instant credibility)
- **Testimonials with photos** near CTAs (faces build trust)
- **Trust badges** (security, awards, certifications) near forms and checkout
- **Specific numbers** over vague claims — "12,847 teams" not "Thousands of users"

### Reading Patterns
- **Z-pattern** for landing pages — eye moves: headline → nav → visual → CTA
- **F-pattern** for content-heavy pages — eye scans: heading → first line → scan left column

---

## Visual Hierarchy

### Semantic Color System
Don't scatter raw hex values. Build a system with CSS custom properties:
```css
:root {
  color-scheme: light dark;

  /* Semantic tokens — these are what you use in components */
  --text-primary: light-dark(oklch(0.2 0 0), oklch(0.93 0 0));
  --text-secondary: light-dark(oklch(0.4 0 0), oklch(0.7 0 0));
  --surface: light-dark(#fff, oklch(0.15 0.01 260));
  --surface-raised: light-dark(oklch(0.97 0 0), oklch(0.2 0.01 260));
  --border: light-dark(oklch(0.85 0 0), oklch(0.3 0 0));
  --accent: oklch(0.65 0.2 250);
  --accent-hover: color-mix(in oklch, var(--accent), black 15%);
  --accent-text: oklch(from var(--accent) calc(l - 0.35) c h);
}
```
- Use **oklch** color space for perceptual uniformity (equal lightness steps look equal)
- Use `light-dark()` for dark mode without duplicating every custom property
- Use `color-mix()` for hover/pressed/disabled states — derive, don't hardcode
- Use relative color syntax for tints/shades: `oklch(from var(--accent) calc(l + 0.15) c h)`

### 60-30-10 Color Rule
- 60% Neutral (background, surfaces)
- 30% Primary (cards, sections, secondary elements)
- 10% Accent (CTAs, highlights, active states)
- **Check:** Never more than 3 main colors
- **Anti-pattern:** Raw hex values with no system = maintenance nightmare and AI slop tell

### Typography Scale (Strict)
```css
h1: clamp(2.5rem, 5vw, 4rem)     /* Hero */
h2: clamp(1.5rem, 3vw, 2.5rem)   /* Section */
h3: 1.25rem                       /* Card Title */
body: 1rem                        /* Base */
small: 0.875rem                   /* Meta */
```
**Rule:** Max 6 sizes, prefer rem over px.

### Font Pairing (Critical for Professional Look)
The #1 differentiator between professional and amateur design:
- **Serif headline + sans-serif body** — classic, editorial feel (e.g., Playfair Display + Source Sans 3)
- **Display headline + body text** — bold personality (e.g., Space Grotesk + Inter)
- **Monospace accent + sans-serif body** — technical/dev feel (e.g., JetBrains Mono + Geist Sans)
- **Variable fonts** — use weight and width axes for hierarchy instead of loading multiple files
- **Weight variation** — use 3+ weights (regular, medium, bold) to create hierarchy beyond size
- **Letter-spacing differentiation** — tighter for large headings (`-0.02em`), normal for body, wider for small caps/labels (`0.05em`)
- **Anti-pattern:** Inter or Roboto as sole font with one weight = instant AI slop tell. If using a single family, vary weight, size, AND letter-spacing aggressively.

### Spacing System (8px Grid — Industry Standard)
Apple HIG and Google Material Design both use 8px as the base unit:
```css
--space-1: 0.25rem;  /* 4px  — fine detail: icon gaps, inline padding */
--space-2: 0.5rem;   /* 8px  — tight: form elements, compact lists */
--space-3: 1rem;     /* 16px — base: paragraph spacing, card padding */
--space-4: 1.5rem;   /* 24px — comfortable: section sub-spacing */
--space-5: 2rem;     /* 32px — spacious: between content groups */
--space-6: 3rem;     /* 48px — generous: section spacing */
--space-7: 4rem;     /* 64px — dramatic: hero padding, page margins */
--space-8: 6rem;     /* 96px — section dividers on large screens */
```
**Primary grid: 8px** (multiples of 0.5rem). Use 4px only for fine-granularity detail (icon-to-text gaps, border offsets).
**Check:** Every spacing value should be on this scale. Random values like 13px or 22px are a code smell.

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

## Game UI Patterns (applies to games only)

### HUD Design
- Health bars, minimaps, ammo counters, cooldown indicators — size + color for importance hierarchy
- Progressive disclosure: show info only when relevant (ammo only during combat, minimap on demand)
- Diegetic UI preferred for immersion (in-world displays) over flat overlays where appropriate
- Screen-space UI vs world-space UI: health bars above enemies use world-space; main HUD uses screen-space

### Menu Architecture
- Pause menus, inventory (grid/tab/radial), skill trees, crafting UIs, dialogue
- Controller-navigable with visible focus indicators (no mouse assumption)
- Button prompts auto-switch based on last input device (keyboard icons ↔ gamepad icons)

### Tutorial / Onboarding
- Contextual tooltips with progressive disclosure of mechanics
- First-time-user experience: one mechanic at a time, let players skip/replay
- Visual highlight overlays for interactive elements

### Resolution Independence
- UI anchoring/stretching for aspect ratios (16:9, 21:9, 4:3, mobile)
- DPI awareness, safe zones for mobile notch/status bar
- Integer scaling for pixel art games

### Game Accessibility (2026 Baseline)
- The Big Three: subtitles (customizable), remappable controls, high contrast mode
- Colorblind modes: deuteranopia, protanopia, tritanopia
- Difficulty as accessibility (not shaming)
- Reduced motion toggle (respect prefers-reduced-motion on web, in-app toggle for native)
- One-handed control schemes, auto-aim/aim assist (configurable)

### Game Performance (frame budget model, replaces CWV for games)
- 60fps = 16.6ms frame budget, 30fps = 33.3ms, 120fps (VR) = 8.3ms
- Object pooling: reuse bullets/particles/enemies instead of alloc/dealloc
- Draw call batching, texture atlases, frustum culling, LOD
- GC avoidance in hot loops: pre-allocate, ring buffers, no string concat in update
- Spatial partitioning for collision: quadtree (2D), octree (3D), spatial hash
- Worker thread offloading: move simulation/physics to Web Worker (or equivalent background thread), render on main thread only
- Zero-copy data transfer: use transferable ArrayBuffers / SharedArrayBuffer for cross-thread item data (eliminates serialization)
- TypedArrays for hot data: Int32Array/Float32Array for counters, item buffers, spatial indices — avoids object overhead and GC
- Adaptive quality: monitor frame times, auto-degrade rendering (particle counts, LOD thresholds, draw detail) when budget exceeded
- Offscreen canvas caching: pre-render static tiles/sprites to offscreen canvases, blit instead of redraw each frame
- Swap-and-pop removal: O(1) unordered array deletion by swapping with last element — no splice in hot loops
- Dirty tracking: only reset/update changed tiles via dirty list instead of clearing entire arrays each frame

### Game Audio
- Separate volume: music, SFX, dialogue
- Sound pooling with max concurrent voices
- Spatial audio falloff for 3D games
- Music crossfading/layering for dynamic scores

---

## Common Anti-Patterns (context-dependent)

**Always wrong:**
- Rainbow Gradients (>2 colors in a gradient)
- Box-Shadow on every static element (only on hover/active)
- Border-Radius 50% on rectangles
- Placeholder text as label (accessibility violation)

**Dead trends (flag these):**
- **Carousel/slider heroes** — users rarely click past slide 1; use a single strong hero instead
- **Parallax overload** — causes motion sickness, kills mobile performance, often used as a crutch for weak content
- **Auto-playing video with sound** — instant bounce; if using video, mute + play button
- **Generic stock illustrations** — Undraw/Humaaans/Storyset saturation; if everyone uses the same illustration library, nobody stands out
- **Full-screen overlay popups on entry** — user hasn't even read the page yet; show after engagement

**Context-dependent — NOT always wrong:**
- **System fonts vs custom fonts:** System font stacks (`system-ui, -apple-system, sans-serif`) have zero FOIT/FOUT, zero CLS, zero network requests, and instant rendering. They are a valid choice for performance-critical apps, dashboards, and tools. Custom fonts (Inter, Geist, etc.) are better for brand-heavy marketing sites and design-forward apps. Neither is universally "correct."
- **Infinite scroll vs pagination:** Virtual/infinite scroll is standard and expected for social feeds, chat interfaces, activity logs, and real-time data streams. Pagination is better for searchable, filterable, bookmarkable content (e-commerce, documentation, search results). "Load More" buttons are a middle ground. Judge by use case, not by blanket rule.

**Game-specific — NOT anti-patterns:**
- **Purple/neon gradients** — valid game aesthetic (stylistic choice, not AI slop)
- **Single font family** — thematic cohesion in games (one font across HUD is normal)
- **Global singletons** — GameManager/AudioManager/InputManager = standard game architecture
- **Large files** — game simulation files are naturally large (single-file games, monolithic update loops)
- **No lazy loading** — games preload assets behind loading screens (this is correct behavior)

## Checklist for UI Review

- [ ] Anti-AI-slop check — does the design have a distinct identity or could any AI have generated it?
- [ ] 60-30-10 color rule followed with semantic color system?
- [ ] Typography: intentional font pairing with weight variation?
- [ ] All spacing on 8px grid?
- [ ] View Transitions on navigation? (where supported)
- [ ] Focus states visible?
- [ ] Loading states better than spinners?
- [ ] Micro-animations < 200ms?
- [ ] Dark mode / system preference respected?
- [ ] No accessibility violations in interactive elements?
- [ ] Font strategy appropriate for the project type?
- [ ] Landing page hero has all 5 elements? (if applicable)
- [ ] CSS features used are production-ready, not experimental?

**Tone for UI failures:**
- "This looks like it was generated by an AI in 5 minutes — purple gradient, Inter font, three-column cards"
- "Color palette is giving migraine"
- "Spacing is random, not systematic"
- "Accessibility: Screen reader users will have a terrible experience"
- "No font pairing — single weight Inter across the entire site is the AI slop starter pack"
