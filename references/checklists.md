# Checklists

---

## Security

- [ ] **Prompt injection prevention** - User input sanitized before LLM
- [ ] **No secrets in code** - API keys in .env only
- [ ] **Input validation** - Zod schemas for every endpoint
- [ ] **SQL injection impossible** - ORM or parameterized queries
- [ ] **Dependency audit clean** - No known CVEs
- [ ] **AI Tool Security** - AI agents cannot delete/modify without confirmation
- [ ] **LLM Output Sanitization** - Never render LLM output as HTML without DOMPurify
- [ ] **Model Rate Limiting** - Cost controls for AI endpoints (max tokens/minute)

## Performance

- [ ] **Core Web Vitals** - LCP < 2s, INP < 100ms, CLS < 0.05
- [ ] **Correct rendering** - Static/Server/Client appropriately
- [ ] **AVIF/WebP images** - Modern formats, responsive srcset
- [ ] **Bundle size** - No 500KB+ initial chunks
- [ ] **Font optimization** - font-display: swap
- [ ] **Partial Pre-rendering** - Next.js 15+ PPR enabled where beneficial
- [ ] **Instrumentation** - Vercel Speed Insights or Web Vitals reporting
- [ ] **Asset Optimization** - Next.js Image with priority on LCP elements

## Architecture

- [ ] **React Server Components** - 'use client' only when needed
- [ ] **TypeScript strict** - strict: true, no implicit any
- [ ] **State management** - TanStack Query for server state
- [ ] **Feature-based folders** - Not type-based
- [ ] **No circular dependencies** - Clean import graph
- [ ] **React 19 Hooks** - useActionState, useOptimistic, useFormStatus where applicable
- [ ] **Server Actions** - Mutations run server-side, not API routes
- [ ] **Parallel Data Fetching** - Promise.all for independent requests
- [ ] **Error Boundaries** - Each route has error.tsx + loading.tsx

## Backend

- [ ] **App Router patterns** - Route handlers in app/api
- [ ] **Rate limiting** - Especially AI endpoints
- [ ] **Auth best practice** - OAuth 2.1 or OIDC
- [ ] **Edge compatible** - No Node.js-only APIs in edge
- [ ] **Database pooling** - Connection reuse

## Accessibility

- [ ] **WCAG 2.2 compliance** - Focus rings, keyboard nav
- [ ] **ARIA labels** - Interactive elements accessible
- [ ] **Semantic HTML** - button not div onClick
- [ ] **Reduced Motion** - Respect prefers-reduced-motion
- [ ] **Color Contrast** - 4.5:1 minimum (use contrast checker)
- [ ] **Focus Management** - Focus trap in modals, return focus on close
