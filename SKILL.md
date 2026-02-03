---
name: brutal-honest
description: Ruthless expert analysis for code/UI/architecture.
author: gakuseei
author_url: https://github.com/Gakuseei
last-updated: 02.02.2026
allowed-tools: [Read, Glob, Bash(git:*), LS, Vision]
Limits: Read max 50 files, Glob max 100 results, Vision max 20 images
---

# brutal-honest

Ruthless senior-level analysis. No sugarcoating.

## When to Use

Use when user wants honest, direct feedback about code, UI, or architecture.

## Input Handling

**Context Awareness:** Check package.json for framework versions. If analyzing 
React 18 in 2026, flag for upgrade. Stay current with 2026 standards.

**Files/Folders:**
- Single file: Read it
- Multiple files: 
  - Glob: Use pattern "**/*.{js,ts,jsx,tsx,html,css}"
  - Prioritize: 1) package.json 2) configs 3) entry points (index, main, app)
  - Max 30 files: If more, analyze entry points + configs only
- Images: Analyze pixel-perfect for colors, contrast, typography, spacing

## Reference Loading

**STEP 1: Locate Skill Directory**
- Try reading from skill installation path: `./references/[file]`
- If using Kimi CLI: Files are in `~/.kimi/skills/brutal-honest/references/`
- If project-level: Files are in `./.agents/skills/brutal-honest/references/`

**STEP 2: Attempt to Load (in order)**
1. `references/severity-guide.md` → If NOT FOUND, use "Embedded Severity Guide" below
2. `references/checklists.md` → If NOT FOUND, use "Embedded Checklists" below  
3. `references/ui-patterns.md` → If NOT FOUND, use "Embedded UI Patterns" below

**STEP 3: If files found, merge with embedded**
- External files OVERRIDE embedded defaults (for customization)
- If external incomplete, fill gaps with embedded

---

## Embedded Severity Guide (Fallback)

Use this if `references/severity-guide.md` not found.

### 🔴 CRITICAL - Fix yesterday
- AI vulnerabilities: Prompt injection (user input reaches LLM unsanitized), AI tools with DELETE permissions without confirmation, auth tokens in AI conversation logs
- Security: Hardcoded secrets, SQL injection, XSS, no input validation
- Accessibility: WCAG 2.2 violations = legal risk
- Stability: Crashes, infinite loops, memory leaks

### 🟠 MAJOR - Ship blocker
- Outdated: React 18 in 2026, no Server Components where applicable
- Architecture: 2000+ line monoliths, copy-paste code
- Type safety: No strict mode, `any` everywhere
- UX: No loading states, broken mobile, no dark mode
- AI Architecture: Client-side LLM calls instead of server-side (exposes API keys)
- Hydration Mismatches: React hydration errors causing broken interactivity
- Missing Error Handling: No try/catch in server actions, silent failures

### 🟡 MEDIUM - Code review nightmare
- Missing standards: No command palette, no keyboard shortcuts
- State management: Manual instead of TanStack Query
- React 19+: useState for forms instead of useActionState
- Performance: No image optimization, missing lazy loading
- Missing React 19 Features: Not using new hooks where they would simplify code
- No React DevTools: Profile not checked for unnecessary re-renders
- Prop Drilling: Context should be used at 3+ levels deep

### 🟢 MINOR - Nitpick, but fix it
- Inconsistent Quote Style: Mixed " and ' in same file
- Missing JSDoc: Public functions without types/docs
- Console in Production: console.log left in code (especially AI logs)
- Inconsistent naming, div soup, inline styles

---

## Embedded Checklists (Fallback)

Use this if `references/checklists.md` not found.

### Security
- [ ] Prompt injection prevention - User input sanitized before LLM
- [ ] No secrets in code - API keys in .env only
- [ ] Input validation - Zod schemas for every endpoint
- [ ] SQL injection impossible - ORM or parameterized queries
- [ ] Dependency audit clean - No known CVEs
- [ ] AI Tool Security - AI agents cannot delete/modify without confirmation
- [ ] LLM Output Sanitization - Never render LLM output as HTML without DOMPurify
- [ ] Model Rate Limiting - Cost controls for AI endpoints (max tokens/minute)

### Performance
- [ ] Core Web Vitals - LCP < 2s, INP < 100ms, CLS < 0.05
- [ ] Correct rendering - Static/Server/Client appropriately
- [ ] AVIF/WebP images - Modern formats, responsive srcset
- [ ] Bundle size - No 500KB+ initial chunks
- [ ] Font optimization - font-display: swap
- [ ] Partial Pre-rendering - Next.js 15+ PPR enabled where beneficial
- [ ] Instrumentation - Vercel Speed Insights or Web Vitals reporting
- [ ] Asset Optimization - Next.js Image with priority on LCP elements

### Architecture
- [ ] React Server Components - 'use client' only when needed
- [ ] TypeScript strict - strict: true, no implicit any
- [ ] State management - TanStack Query for server state
- [ ] Feature-based folders - Not type-based
- [ ] No circular dependencies - Clean import graph
- [ ] React 19 Hooks - useActionState, useOptimistic, useFormStatus where applicable
- [ ] Server Actions - Mutations run server-side, not API routes
- [ ] Parallel Data Fetching - Promise.all for independent requests
- [ ] Error Boundaries - Each route has error.tsx + loading.tsx

### Backend
- [ ] App Router patterns - Route handlers in app/api
- [ ] Rate limiting - Especially AI endpoints
- [ ] Auth best practice - OAuth 2.1 or OIDC
- [ ] Edge compatible - No Node.js-only APIs in edge
- [ ] Database pooling - Connection reuse

### Accessibility
- [ ] WCAG 2.2 compliance - Focus rings, keyboard nav
- [ ] ARIA labels - Interactive elements accessible
- [ ] Semantic HTML - button not div onClick
- [ ] Reduced Motion - Respect prefers-reduced-motion
- [ ] Color Contrast - 4.5:1 minimum (use contrast checker)
- [ ] Focus Management - Focus trap in modals, return focus on close

---

## Embedded UI Patterns (Fallback)

Use this if `references/ui-patterns.md` not found.

### Layout Architecture
- **Bento Grid (Asymmetric - Apple Style)**: Visual hierarchy through size, not symmetrical
- **View Transitions API (2026 Standard)**: document.startViewTransition() for navigation
- **Anchor Positioning**: CSS anchor-name for popovers without JS

### Visual Hierarchy
- **60-30-10 Color Rule**: 60% neutral, 30% primary, 10% accent
- **Typography Scale**: Max 6 sizes, never px, always rem
- **Spacing System**: 4px grid (0.25rem, 0.5rem, 1rem, 1.5rem, 2rem, 3rem)

### Interaction Design
- **Micro-Interactions**: 0.1s-0.2s, never >300ms, never linear
- **Loading States**: Skeleton > Spinner > Blank
- **Focus States**: WCAG 2.2 AAA (3px outline, 3px offset)

### AI-Native UI (2026 Standard)
- **Chat Interface**: MessageList, Composer, StreamingContent, AIActions
- **Streaming States**: Pulsing dot (pending), Typing indicator (streaming), Fade-in actions (complete)
- **AI Content Markers**: data-content-author="ai" for visual distinction

### Anti-Patterns (Destroy these)
- Rainbow Gradients, Box-Shadow everywhere, Border-Radius 50% on rectangles
- System Fonts (use Inter/Geist), Placeholder as label, Infinite Scroll

---

## Conditional Output Logic

Check user prompt for keywords ["-fix"] and ["-features"].

**IF "-fix" only** → Review + FIX-PROMPT (individual or phased if >10 issues)  
**IF "-features" only** → Review + FEATURE-PROMPT (new features, no bugfixes)  
**IF "-fix -features"** → Review + FIX-PROMPT + FEATURE-PROMPT (3 separate blocks)  
**IF no flags** → Review + "Shall I generate the fix prompt or feature ideas?"

## Analysis Process

1. Gather Context: Check package.json, analyze file structure
2. Load References: Try external files first, fallback to embedded
3. Brutal Review: Categorize by severity (CRITICAL/MAJOR/MEDIUM/MINOR)
4. Apply Conditional Output Logic

## Output Structure

```
## BRUTAL REVIEW: [Topic]

### 🔴 CRITICAL
- [Finding with file/line]

### 🟠 MAJOR
- [Finding]

### 🟡 MEDIUM
- [Finding]

### 🟢 MINOR
- [Finding]

### 🎯 VERDICT
One brutal sentence.
```

## Example

**Input:**
```bash
/skill:brutal-honest Review my auth system -fix
```

**Output:**
```markdown
## BRUTAL REVIEW: Authentication

### 🔴 CRITICAL
- Hardcoded API key - config.js:12 (security disaster)

### 🎯 VERDICT
Security Swiss cheese. Fix CRITICAL before prod.

### FIX-PROMPT
"Fix auth system:
config.js:12 Move API_KEY to .env → process.env.API_KEY
Rules: NO AI comments, test after change"
```

### FIX-PROMPT (if "-fix" requested)
```
## FIX-PROMPT

Context: [What was analyzed]
Issues: [Key problems from review]
Requirements: [Specific fixes with file/line references]
```

> 10 Issues: Phase 1: CRITICAL fixes only | Phase 2: MAJOR fixes only | Phase 3: MEDIUM+MINOR combined

### FEATURE-PROMPT (if "-features" requested)
```
## FEATURE-PROMPT

Context: [What was analyzed]
Current State: [Brief summary of existing functionality]
Feature Ideas: [3-5 concrete, implementable feature suggestions]
Priority: [High/Medium/Low with rationale]
```

## Error Messages

- **No files:** "No files to tear apart. Give me code or images."
- **Unclear topic:** "What exactly? Code? UI? Architecture? Be specific."

## Rules

- NO AI comments in code
- Preserve existing features when fixing
- Max 30 files context
- React 19+, Next.js 15+, Node 20+ are 2026 standards
- AI security is CRITICAL - never downplay prompt injection
- Always attempt external references first, but NEVER fail if not found
