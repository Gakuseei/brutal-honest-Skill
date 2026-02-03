# Severity Guide

---

## 🔴 CRITICAL

Fix yesterday.

- **AI vulnerabilities** - Prompt injection (user input reaches LLM unsanitized), AI tools with DELETE permissions without confirmation, auth tokens in AI conversation logs
- **Security** - Hardcoded secrets, SQL injection, XSS, no input validation
- **Accessibility** - WCAG 2.2 violations = legal risk
- **Stability** - Crashes, infinite loops, memory leaks

## 🟠 MAJOR

Ship blocker.

- **Outdated** - React 18 in 2026, no Server Components where applicable
- **Architecture** - 2000+ line monoliths, copy-paste code
- **Type safety** - No strict mode, `any` everywhere
- **UX** - No loading states, broken mobile, no dark mode
- **AI Architecture** - Client-side LLM calls instead of server-side (exposes API keys)
- **Hydration Mismatches** - React hydration errors causing broken interactivity
- **Missing Error Handling** - No try/catch in server actions, silent failures

## 🟡 MEDIUM

Code review nightmare.

- **Missing standards** - No command palette, no keyboard shortcuts
- **State management** - Manual instead of TanStack Query
- **React 19+** - useState for forms instead of useActionState
- **Performance** - No image optimization, missing lazy loading
- **Missing React 19 Features** - Not using new hooks where they would simplify code
- **No React DevTools** - Profile not checked for unnecessary re-renders
- **Prop Drilling** - Context should be used at 3+ levels deep

## 🟢 MINOR

Nitpick, but fix it.

- **Inconsistent Quote Style** - Mixed " and ' in same file
- **Missing JSDoc** - Public functions without types/docs
- **Console in Production** - console.log left in code (especially AI logs)
- Inconsistent naming, div soup, inline styles

---

**Tone:** Direct. "This is 2024 code, we're in 2026."
