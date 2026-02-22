# Checklists

---

## Universal Checklist (applies to ALL projects)

### Security
- [ ] **No secrets in code** — API keys, tokens, passwords in env vars / secret manager only
- [ ] **Input validation** — Validate at system boundaries (user input, external APIs, file uploads)
- [ ] **Injection prevention** — ORM / parameterized queries / sanitized output (SQL, XSS, command injection)
- [ ] **Dependency audit clean** — No known CVEs in dependencies
- [ ] **CSRF protection** — Anti-forgery tokens or SameSite cookies (web apps)
- [ ] **Auth best practice** — OAuth 2.1, OIDC, or framework-recommended auth patterns
- [ ] **AI security (if applicable)** — Prompt injection prevention, LLM output sanitization, AI tool permissions gated

### Performance
- [ ] **Appropriate for platform** — Web: Core Web Vitals (LCP <2s, INP <100ms, CLS <0.05) | Mobile: startup <2s | API: p95 <200ms
- [ ] **Asset optimization** — Images (modern formats), fonts (font-display: swap), bundle sizes appropriate for target
- [ ] **Caching strategy** — HTTP caching, application-level caching, CDN where appropriate
- [ ] **Lazy loading** — Below-fold content, heavy dependencies, routes/pages loaded on demand
- [ ] **No unnecessary computation** — Database queries optimized, N+1 queries eliminated, pagination for large datasets

### Architecture
- [ ] **Separation of concerns** — No God objects/functions, clear module boundaries
- [ ] **Clean dependency graph** — No circular dependencies, clear import direction
- [ ] **DRY without over-abstraction** — Repeated code extracted, but no premature abstraction
- [ ] **Error handling** — Graceful failures, user-facing error messages, structured logging
- [ ] **Configuration management** — Environment-specific config separated from code
- [ ] **Database patterns** — Connection pooling, migrations versioned, proper ORM usage (if applicable)

### Testing
- [ ] **Unit tests** — Core business logic covered with meaningful assertions
- [ ] **Integration tests** — Critical paths tested (API endpoints, database operations, auth flows)
- [ ] **E2E tests** — Key user flows covered (where applicable)
- [ ] **Test isolation** — Tests don't depend on external services or shared state
- [ ] **Coverage strategy** — Coverage thresholds defined for critical modules (not vanity 100%)
- [ ] **Mocking boundaries** — External services mocked at integration boundary, not deep internals
- [ ] **Test naming** — Test names describe behavior, not implementation ("should reject expired tokens" not "test1")

### CI/CD
- [ ] **Automated builds** — Build runs on every PR
- [ ] **Linting/formatting** — Enforced in CI, not just local
- [ ] **Tests run on PR** — Failing tests block merge
- [ ] **Deployment strategy** — Automated deploy with rollback capability

### Accessibility (web and mobile)
- [ ] **WCAG 2.2 compliance** — Focus rings, keyboard navigation, screen reader support
- [ ] **Semantic markup** — Correct elements for their purpose (button not div, nav not div, etc.)
- [ ] **ARIA labels** — Interactive elements accessible, landmarks defined
- [ ] **Color contrast** — 4.5:1 minimum for normal text, 3:1 for large text
- [ ] **Reduced motion** — Respect prefers-reduced-motion
- [ ] **Focus management** — Focus trap in modals, return focus on close

---

## Stack-Specific Checklists (applied ONLY when that stack is detected)

### React / Next.js
- [ ] **React Server Components** — `'use client'` only when needed, default to server
- [ ] **React 19.2 hooks** — useActionState, useOptimistic, useFormStatus, use() where applicable
- [ ] **Server Actions** — Mutations run server-side, not client-side API calls
- [ ] **Partial Pre-rendering (PPR)** — Enabled where beneficial (Next.js 15+/16+)
- [ ] **Error boundaries** — Each route has error.tsx + loading.tsx
- [ ] **State management** — TanStack Query for server state, not manual fetching
- [ ] **TypeScript strict** — strict: true, no implicit any

### Vue / Nuxt
- [ ] **Composition API** — Using `<script setup>` with Composition API, not Options API
- [ ] **Pinia stores** — Proper store patterns, no Vuex in new projects
- [ ] **Auto-imports** — Nuxt auto-imports configured, no manual imports for composables
- [ ] **defineModel** — Vue 3.5+ `defineModel` for v-model, not manual prop/emit
- [ ] **TypeScript** — Typed props with `defineProps<T>()`, typed emits

### Svelte / SvelteKit
- [ ] **Runes** — Using `$state`, `$derived`, `$effect` (Svelte 5+), not legacy reactive declarations
- [ ] **Server load functions** — Data fetching in `+page.server.ts`, not client-side
- [ ] **Form actions** — Server-side form handling with progressive enhancement
- [ ] **TypeScript** — Typed with `lang="ts"`, proper `PageData` typing

### Angular
- [ ] **Signals** — Using Angular signals for reactivity, not zone-based change detection
- [ ] **Standalone components** — No NgModule-based architecture in new code
- [ ] **Control flow** — Using `@if`, `@for`, `@switch` (Angular 17+), not `*ngIf`, `*ngFor`
- [ ] **Zoneless** — Prepared for zoneless change detection (Angular 21+)
- [ ] **Typed forms** — Strongly typed reactive forms

### Python (Django / FastAPI / Flask)
- [ ] **Type hints** — Full type annotation (Python 3.13+ syntax)
- [ ] **Async patterns** — async/await where I/O bound (FastAPI, Django 5.x async views)
- [ ] **ORM usage** — Proper queryset patterns, select_related/prefetch_related, no N+1
- [ ] **Virtual environment** — Dependencies isolated, requirements pinned
- [ ] **Security middleware** — CORS, rate limiting, input validation (Pydantic / Django forms)

### Go
- [ ] **Error handling** — `errors.Is`/`errors.As`, wrapped errors, no silent `_ = err`
- [ ] **Goroutine management** — Context cancellation, no goroutine leaks, proper sync primitives
- [ ] **Interfaces** — Accept interfaces, return structs; small, focused interfaces
- [ ] **go vet / staticcheck** — Clean output, no warnings suppressed
- [ ] **Module structure** — Clean `go.mod`, no unnecessary dependencies

### Rust
- [ ] **Ownership patterns** — Minimal cloning, proper lifetime annotations
- [ ] **Error handling** — `Result`/`Option` used properly, `thiserror`/`anyhow` for error types
- [ ] **Unsafe audit** — All `unsafe` blocks justified and documented
- [ ] **Clippy clean** — No clippy warnings suppressed without justification
- [ ] **Cargo.toml** — Dependencies audited, features minimized

### PHP / Laravel
- [ ] **Eloquent patterns** — Proper relationships, eager loading, no N+1
- [ ] **Middleware** — Auth, rate limiting, CORS configured properly
- [ ] **Queue patterns** — Long-running tasks queued, not inline
- [ ] **Laravel 12 features** — Using latest framework idioms, not legacy patterns
- [ ] **Type declarations** — PHP 8.3+ type hints, strict_types declared

### Astro
- [ ] **Island architecture** — Interactive components wrapped in `client:*` directives, static by default
- [ ] **Content Collections** — Using type-safe content collections with Zod schemas, not raw file reads
- [ ] **Zero JS by default** — Pages ship zero client JS unless explicitly opted in with `client:load/visible/idle`
- [ ] **View Transitions** — Using Astro's built-in View Transitions for page navigation
- [ ] **Integration usage** — Using official integrations (@astrojs/react, @astrojs/tailwind) not manual configs

### Remix
- [ ] **Loaders and actions** — Data fetching in `loader()`, mutations in `action()`, not client-side fetches
- [ ] **Nested routes** — Route hierarchy reflects UI hierarchy, shared layouts via nesting
- [ ] **Progressive enhancement** — Forms work without JavaScript, `<Form>` component used
- [ ] **Error boundaries** — Each route has `ErrorBoundary` export for graceful error handling
- [ ] **Optimistic UI** — Using `useNavigation`/`useFetcher` for optimistic updates during mutations

### SolidJS
- [ ] **Fine-grained signals** — Using `createSignal`, `createMemo`, `createEffect` — not React-style re-renders
- [ ] **No virtual DOM assumptions** — Components run once; reactivity via signals, not re-execution
- [ ] **createResource** — Server data fetching with `createResource`, not manual fetch in effects
- [ ] **Show/For/Switch** — Using Solid's control flow components (`<Show>`, `<For>`, `<Switch>`) not ternaries
- [ ] **Store patterns** — Using `createStore` for nested reactive state, not nested signals

### Hono
- [ ] **Web Standards** — Using standard `Request`/`Response`, not framework-specific abstractions
- [ ] **Middleware chains** — Auth, CORS, logging as composable middleware, not inline in handlers
- [ ] **Multi-runtime** — Tested on target runtimes (Cloudflare Workers, Deno, Bun, Node.js)
- [ ] **Validator middleware** — Using `hono/validator` with Zod for request validation, not manual parsing
- [ ] **Type-safe routes** — Using RPC mode or typed route parameters for client-server type safety

### Ruby / Rails
- [ ] **Active Record patterns** — Proper scopes, validations, callbacks; no raw SQL without justification
- [ ] **N+1 prevention** — Using `includes`/`eager_load`/`preload`, verified with Bullet gem
- [ ] **Hotwire / Turbo** — Turbo Frames and Turbo Streams for dynamic UI, not heavy JS frameworks
- [ ] **Rails conventions** — Following Rails 8 conventions (Solid Queue, Solid Cache, Kamal deploy)
- [ ] **Security** — Strong parameters, CSRF protection, Content Security Policy headers configured

### Elixir / Phoenix
- [ ] **OTP patterns** — GenServer, Supervisor trees used correctly; processes not used as global mutable state
- [ ] **LiveView** — Using LiveView for real-time UI, proper handle_event/handle_info separation
- [ ] **Fault tolerance** — "Let it crash" philosophy: supervisors restart failed processes, not defensive try/catch everywhere
- [ ] **Contexts** — Business logic in Phoenix Contexts (bounded contexts), not in controllers or LiveView
- [ ] **Ecto patterns** — Changesets for validation, preloads for associations, no N+1 queries

### C / C++
- [ ] **Memory safety** — RAII for resource management, smart pointers (`unique_ptr`, `shared_ptr`), no raw `new`/`delete`
- [ ] **Modern C++ (20/23)** — Using ranges, concepts, `std::format`, `std::expected`, coroutines where beneficial
- [ ] **No undefined behavior** — No dangling pointers, buffer overflows, use-after-free; sanitizers (ASan, UBSan) clean
- [ ] **Build system** — CMake with proper targets, no global include/link; dependencies via vcpkg/Conan
- [ ] **Static analysis** — clang-tidy clean, no suppressed warnings without justification

### Swift
- [ ] **Structured concurrency** — Using `async`/`await`, `TaskGroup`, actors for concurrency; no raw GCD unless necessary
- [ ] **Protocol-oriented design** — Protocols over class inheritance, extensions for shared behavior
- [ ] **SwiftUI lifecycle** — Using `@Observable` (not `ObservableObject`), `@State`, `@Environment` correctly
- [ ] **Error handling** — Typed throws (Swift 6), `Result` type for async boundaries, no force unwraps in production
- [ ] **Memory management** — `[weak self]` in closures, no retain cycles, Instruments leak-free

### Kotlin
- [ ] **Coroutines** — Structured concurrency with `CoroutineScope`, proper `Dispatchers`, no `GlobalScope`
- [ ] **Null safety** — No `!!` (force unwrap) in production code, `?.let {}` / `?:` for null handling
- [ ] **Multiplatform** — Shared code in `commonMain`, platform-specific in `expect`/`actual` (if KMP project)
- [ ] **Sealed classes** — Using sealed classes/interfaces for exhaustive `when` expressions in state/event modeling
- [ ] **Flow** — Using `StateFlow`/`SharedFlow` for reactive streams, not LiveData in new code

### Vanilla HTML / JS / CSS
- [ ] **Progressive enhancement** — Works without JS where possible
- [ ] **Semantic HTML** — Correct elements, proper heading hierarchy, landmark regions
- [ ] **No-build patterns** — ES modules, CSS custom properties, no unnecessary toolchain
- [ ] **Web Standards** — Using modern APIs (Fetch, Web Components, CSS nesting, etc.)
- [ ] **Accessibility** — ARIA where semantic HTML isn't sufficient, focus management

### Mobile (React Native / Flutter / SwiftUI)
- [ ] **Platform conventions** — Following iOS HIG / Material Design guidelines
- [ ] **Navigation patterns** — Using framework's navigation (React Navigation, GoRouter, NavigationStack)
- [ ] **State management** — Appropriate for framework (Riverpod/Bloc for Flutter, Zustand for RN)
- [ ] **Performance** — No jank, 60fps scrolling, optimized list rendering
- [ ] **Accessibility** — VoiceOver/TalkBack support, semantic labels
