# Severity Guide

---

## CRITICAL

Fix yesterday.

- **AI vulnerabilities** — Prompt injection (user input reaches LLM unsanitized), AI tools with DELETE permissions without confirmation, auth tokens in AI conversation logs
- **Security** — Hardcoded secrets, SQL injection, XSS, CSRF without protection, no input validation at system boundaries
- **Accessibility** — WCAG 2.2 violations = legal risk
- **Stability** — Crashes, infinite loops, memory leaks, unhandled exceptions in critical paths
- **Data loss** — No backups, no transaction safety, destructive operations without confirmation

## MAJOR

Ship blocker.

- **Outdated framework** — Using a version 2+ major versions behind current stable release (check against latest stable, not a hardcoded version number)
- **Architecture** — 2000+ line files without clear section separation (unless architecturally intentional, e.g., single-file apps, Go's copy-over-dependency idiom), God objects/functions, circular dependencies
- **Type safety** — No strict mode (where language supports it), `any`/`object`/`interface{}` everywhere at boundaries
- **UX** — No loading states, broken mobile/responsive, no error feedback to users
- **AI Architecture** — Client-side LLM calls instead of server-side (exposes API keys)
- **Framework-specific rendering errors** — Hydration mismatches (SSR frameworks), template compilation errors, runtime binding failures
- **Missing error handling** — No try/catch in async operations, silent failures, unhandled promise rejections / panics / exceptions

## MEDIUM

Code review nightmare.

- **Not using framework idioms** — Not leveraging the framework's recommended patterns and latest stable features
- **Performance** — No image/asset optimization, missing lazy loading, no caching strategy
- **Missing tests** — No unit tests, no integration tests for critical paths
- **No CI/CD** — No automated builds, no linting on PR, manual deployments
- **State management** — Prop drilling / global mutation / spaghetti state where framework provides better patterns
- **Missing keyboard navigation** — No keyboard shortcuts for power users (where applicable)
- **No monitoring** — No error tracking, no performance monitoring, no logging strategy

## MINOR

Nitpick, but fix it.

- **Inconsistent code style** — Mixed formatting, naming conventions, quote styles within the same file
- **Missing documentation** — Public APIs / exported functions without types or docs
- **Debug code in production** — console.log, print(), debug flags, TODO comments left in shipped code
- **Non-semantic markup** — div soup, inline styles (web projects)
- **Inconsistent naming** — Mixed camelCase/snake_case, unclear variable names

---

**Tone:** Direct. "This framework version is 2+ major releases behind current stable. Update or justify."
