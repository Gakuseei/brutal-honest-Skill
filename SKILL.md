---
name: brutal-honest
description: Use when user wants honest code/UI/architecture review. Launches interactive wizard, then parallel SubAgent reviews with mandatory evidence. Tech-stack agnostic, 33 stacks supported.
allowed-tools: Read Glob Grep Bash Agent AskUserQuestion WebSearch
metadata:
  author: gakuseeii
  author_url: https://gitlab.com/Gakuseeii
  version: "3.0.0"
  last-updated: "2026-03-12"
---

# brutal-honest

Ruthless expert analysis with evidence. No guessing, no hallucinating, no ego.

## Iron Rules (non-negotiable, hardcoded, every phase)

1. **Read EVERY file before judging** — no exceptions, no assumptions
2. **file:line for EVERY finding** — no evidence = no finding
3. **Grep to verify** before claiming a pattern is missing
4. **If uncertain → ASK the user or RESEARCH** — never guess
5. **If something looks intentional → ASK, don't flag** — one question too many > one false finding
6. **NEVER invent findings** — hallucinated findings are worse than no findings
7. **ALL user interaction via AskUserQuestion** — never ask questions as chat text

## Process Flow

```
/brutal-honest → Wizard (Phase 1) → Research (Phase 2) → Follow-Up Questions (Phase 3) → SubAgent Review (Phase 4) → Output (Phase 5) → Action Wizard (Phase 6) → Fix Cycle (Phase 7) → Summary (Phase 8)
```

## Phase 1: Interactive Wizard

When the skill is invoked, present a 3-step wizard. ALL steps use `AskUserQuestion`. Never ask questions as plain chat text.

### Step 1 — What to Review

Use `AskUserQuestion`:

- **header:** "Review"
- **question:** "What should I review?"
- **multiSelect:** true
- **options:**
  - label: "Security", description: "OWASP, secrets, auth, input validation, AI security, dependency audit"
  - label: "Architecture & Code", description: "File structure, dependencies, DRY, error handling, types, testing coverage"
  - label: "Performance", description: "Core Web Vitals / frame budget, assets, caching, lazy loading, N+1 queries"
  - label: "UI/UX & Accessibility", description: "WCAG 2.2, mobile/responsive, visual hierarchy, anti-AI-slop patterns"

"Other" is always available via AskUserQuestion for custom focus areas.

### Step 2 — Review Settings

Use `AskUserQuestion` with 2 questions:

**Question 1:**
- **header:** "Scope"
- **question:** "What files should I review?"
- **multiSelect:** false
- **options:**
  - label: "git diff only (Recommended)", description: "Only changed files since last commit"
  - label: "Entire project", description: "All project files (max 30, prioritized by importance)"
  - label: "Specific files/folders", description: "You specify which files or directories"

**Question 2:**
- **header:** "Stack"
- **question:** "How should I detect your tech stack?"
- **multiSelect:** false
- **options:**
  - label: "Auto-detect (Recommended)", description: "Check config files (package.json, go.mod, etc.) automatically"
  - label: "Manual override", description: "You tell me which stack to review against"

### Step 3 — After the Review

Use `AskUserQuestion`:

- **header:** "Extras"
- **question:** "What do you want after the review?"
- **multiSelect:** true
- **options:**
  - label: "Fix Prompt", description: "Generate fix instructions grouped by severity"
  - label: "Feature Ideas", description: "Spawn a Feature Scout SubAgent that explores and suggests 3-5 implementable features"
  - label: "Verify Commits", description: "Run verification pipeline against recent git history"
  - label: "Review only (Recommended)", description: "Just the brutal review, no extras"

## Phase 2: Research + Discovery (automatic)

After the wizard completes, research runs automatically. No user input needed — the skill decides when web research is necessary.

### Step 1: Stack Detection

Use the Stack Detection table below to identify the project's tech stack. Read config files (package.json, go.mod, pyproject.toml, etc.) — do NOT guess from file extensions alone.

### Step 2: File Collection

Based on the scope selected in Step 2 of the wizard:
- **git diff:** Run `git diff --name-only` (or `git diff HEAD --name-only` for staged changes)
- **Entire project:** Glob for source files, prioritize: 1) Config files 2) Entry points 3) Core logic. Max 30 files.
- **Specific files:** Use the user's specified paths

### Step 3: Read All Files

**Read EVERY collected file using the Read tool.** This is mandatory. Do not skip files. Do not assume content from filenames. Actually read and understand the content before any review.

### Step 4: Automatic Web Research

When uncertain about anything, spawn a Web Search SubAgent via the Agent tool:
- **Framework versions:** Is the detected version current stable? Check "[framework] latest stable version 2026"
- **Known CVEs:** If Security review selected, search for vulnerabilities in detected dependencies
- **Best practices:** If Architecture review selected, search for current recommended patterns
- **Unknown patterns:** If you encounter code patterns you don't recognize, research them before judging

Research results are passed to all review agents as context in Phase 4.

### Step 5: Understand the Codebase

Before reviewing, build understanding:
- What patterns does this codebase follow?
- What's the architecture (monolith, microservices, modular)?
- What looks intentional vs accidental?
- What's disabled, commented out, or feature-flagged?

Flag anything suspicious for Phase 3 follow-up questions.

## Phase 3: Informed Follow-Up Questions

After reading the actual code, ask targeted questions based on REAL findings. Every question uses `AskUserQuestion` with recommended answers.

### When to Ask

Ask via `AskUserQuestion` when you find something that COULD be a deliberate decision:

- **Disabled features** — "Feature X is disabled via early return at `file:line`. Intentional?"
  - Yes, I'm testing something (Recommended if code looks deliberate)
  - No, that's a bug — flag it
- **Unusual patterns** — "Found `dangerouslySkipPermissions: true` in `file:line`. On purpose?"
  - Yes, deliberate choice
  - No, should be removed
- **Outdated versions** — "[Framework] v[old] detected, current stable is v[new]. Flag as MAJOR?"
  - Yes, upgrade is important
  - No, staying on this version deliberately
- **Deactivated code** — "Code block at `file:line` is commented out / feature-flagged off. Intended?"
  - Yes, temporary for testing
  - No, forgot to clean up
- **Security concerns** — "Found [potential issue] in `file:line`. Deep security audit?"
  - Yes, check thoroughly
  - No, it's safe in this context

### Core Rules for Questions

1. **Only ask about things you ACTUALLY FOUND** — never hypothetical questions
2. **Maximum 4 questions per AskUserQuestion call** (tool limit) — batch related questions
3. **Include (Recommended) on the option you'd suggest** based on what the code looks like
4. **Answers inform the review** — intentional patterns are excluded from findings, confirmed bugs are flagged
5. **If nothing suspicious found → skip Phase 3 entirely** — don't ask questions for the sake of asking
6. **When in doubt: ASK** — one unnecessary question is infinitely better than one false finding that wastes the user's time

## Stack Detection (Step 0 — before analysis)

Detect the project's tech stack before applying any checklist. This determines which severity items and checklist sections are relevant.

| File Found | Stack | Checklist Focus |
|---|---|---|
| `package.json` with `"react"` | React/Next.js | React-specific + Universal |
| `package.json` with `"vue"` | Vue/Nuxt | Vue-specific + Universal |
| `package.json` with `"svelte"` | Svelte/SvelteKit | Svelte-specific + Universal |
| `package.json` with `"@angular"` | Angular | Angular-specific + Universal |
| `requirements.txt` / `pyproject.toml` with `django` | Python (Django) | Python (Django) + Universal |
| `requirements.txt` / `pyproject.toml` with `fastapi` | Python (FastAPI) | Python (FastAPI) + Universal |
| `requirements.txt` / `pyproject.toml` (no django/fastapi) | Python (generic) | Universal only |
| `go.mod` | Go | Go-specific + Universal |
| `Cargo.toml` | Rust | Rust-specific + Universal |
| `composer.json` | PHP/Laravel | PHP-specific + Universal |
| `pom.xml` / `build.gradle` | Java/Kotlin | JVM-specific + Universal |
| `*.csproj` / `*.sln` | C#/.NET | .NET-specific + Universal |
| `pubspec.yaml` | Flutter/Dart | Flutter-specific + Mobile + Universal |
| `astro.config.*` | Astro | Astro-specific + Universal |
| `package.json` with `"@remix-run"` | Remix | Remix-specific + Universal |
| `package.json` with `"solid-js"` | SolidJS | Solid-specific + Universal |
| `package.json` with `"hono"` | Hono | Hono-specific + Universal |
| `Gemfile` with `rails` | Ruby/Rails | Rails-specific + Universal |
| `mix.exs` | Elixir/Phoenix | Elixir-specific + Universal |
| `CMakeLists.txt` / `*.cpp` + `Makefile` | C/C++ | C/C++-specific + Universal |
| `Package.swift` / `*.xcodeproj` | Swift/iOS | Swift-specific + Mobile + Universal |
| `build.gradle.kts` with `kotlin` | Kotlin (native/multiplatform) | Kotlin-specific + Universal |
| `*.html` (standalone, no framework) | Vanilla HTML/JS/CSS | Web standards + Universal |
| `package.json` with `"react-native"` | React Native | Mobile + Universal |
| `Assets/` + `ProjectSettings/` + `*.unity` | Unity | Unity-specific + Game + Universal |
| `*.uproject` | Unreal Engine | Unreal-specific + Game + Universal |
| `project.godot` | Godot | Godot-specific + Game + Universal |
| `package.json` with `"phaser"` | Phaser | Phaser-specific + Game + Universal |
| `package.json` with `"three"` | Three.js | Three.js-specific + Game + Universal |
| `package.json` with `"pixi.js"` | PixiJS | Phaser/Web 2D-specific + Game + Universal |
| `package.json` with `"kaplay"` | Kaplay | Phaser/Web 2D-specific + Game + Universal |
| `Cargo.toml` with `bevy` | Bevy | Bevy-specific + Game + Universal |
| `conf.lua` + `main.lua` | Love2D | Love2D-specific + Game + Universal |
| `*.py` with `import pygame` | Pygame | Pygame-specific + Game + Universal |
| `*.html` with `<canvas>` + game loop (heuristic: `requestAnimationFrame` or `setInterval` with update/draw pattern) | Web Canvas Game | Game + Web standards + Universal |
| No recognizable config | Ask user | — |

**Important:** Only apply stack-specific items when that stack is detected. A Python API does not get React checklist items. A vanilla HTML game does not get Next.js items.

**Multi-stack projects:** If multiple config files are detected (e.g., `package.json` + `pyproject.toml` in a monorepo), apply ALL matching stack-specific checklists. Review each sub-project against its own stack.

## Input Handling

**Context Awareness:** Check for framework config files (package.json, pyproject.toml, go.mod, etc.) to detect versions. Flag outdated framework versions (2+ major versions behind current stable). (Exception: game engines — LTS versions are standard; only flag end-of-life or unsupported versions.) Check current stable version of the detected framework using available tools or training knowledge — flag anything 2+ major versions behind; if unable to verify, state the assumption.

**Files/Folders:**
- Single file: Read it
- Multiple files:
  - Glob: Use pattern appropriate to detected stack. Default: `**/*.{js,ts,jsx,tsx,vue,svelte,astro,html,css,py,go,rs,java,kt,php,rb,cs,dart,swift,ex,exs,cpp,hpp,c,h,erl,gd,tscn,tres,unity,uproject,lua}`
  - Prioritize: 1) Config files (package.json, pyproject.toml, go.mod, etc.) 2) Entry points 3) Core logic
  - Max 30 files: If more, analyze entry points + configs only
  - Monorepos: If >3 config files detected at different paths, treat as multi-project. Analyze workspace root config + one entry point per sub-project. Distribute the 30-file limit proportionally. When user specifies a sub-project, focus on that one.
- Images: Use Read tool to analyze screenshots/mockups for colors, contrast, typography, spacing, layout issues. Supports PNG, JPG, WebP. If no image provided, skip.

## Reference Loading

**STEP 1: Locate Skill Directory**
- Try reading from skill installation path: `./references/[file]`
- Check your AI tool's skill/plugin directory for the `references/` folder (e.g., `~/.claude/skills/brutal-honest/references/`, project-level `.agents/skills/brutal-honest/references/`, or equivalent)

**STEP 2: Attempt to Load (in order)**
1. `references/severity-guide.md` → If NOT FOUND, use "Embedded Severity Guide" below
2. `references/checklists.md` → If NOT FOUND, use "Embedded Checklists" below
3. `references/ui-patterns.md` → If NOT FOUND, use "Embedded UI Patterns" below

**STEP 3: If files found, external overrides embedded; fill gaps with embedded defaults**
- External files OVERRIDE embedded defaults (for customization)
- If external incomplete, fill gaps with embedded

---

## Embedded Severity Guide (Fallback)

> These embedded sections are fallbacks only. If you have the `references/` folder installed, the external files take priority and these are ignored.

Use this if `references/severity-guide.md` not found.

### CRITICAL - Fix yesterday
- **AI vulnerabilities** — Prompt injection (user input reaches LLM unsanitized), AI tools with DELETE permissions without confirmation, auth tokens in AI conversation logs
- **Security** — Hardcoded secrets, SQL injection, XSS, CSRF without protection, no input validation at system boundaries
- **Accessibility** — WCAG 2.2 violations = legal risk
- **Stability** — Crashes, infinite loops, memory leaks, unhandled exceptions in critical paths
- **Data loss** — No backups, no transaction safety, destructive operations without confirmation
- **Soft-lock** — Player cannot progress and cannot load a prior save (game projects)
- **Save corruption** — Save data can be lost or corrupted without detection/recovery (game projects)
- **Determinism failure** — Desyncs in multiplayer from non-deterministic simulation (multiplayer games)

### MAJOR - Ship blocker
- **Outdated framework** — Using a version 2+ major versions behind current stable release (check against latest stable, not a hardcoded version number; for game engines: locking to an LTS version is standard; flag only if engine version is end-of-life or unsupported)
- **Architecture** — 2000+ line files without clear section separation (unless architecturally intentional, e.g., single-file apps, embeddable widgets, Go's copy-over-dependency idiom), God objects/functions, circular dependencies
- **Type safety** — No strict mode (where language supports it), `any`/`object`/`interface{}` everywhere at boundaries
- **UX** — No loading states, broken mobile/responsive, no error feedback to users
- **AI Architecture** — Client-side LLM calls instead of server-side (exposes API keys)
- **Framework-specific rendering errors** — Hydration mismatches (SSR frameworks), template compilation errors, runtime binding failures
- **Missing error handling** — No try/catch in async operations, silent failures, unhandled promise rejections / panics / exceptions
- **No object pooling** — Frequently spawned objects (bullets, particles, enemies) allocated/destroyed per frame causing GC pauses (game projects)
- **Wrong update loop** — Physics in render loop or rendering in physics loop, causing frame-rate-dependent behavior (game projects)
- **No frame budget discipline** — No profiling, no performance targets, frame time exceeds 16.6ms in core gameplay (game projects)
- **No worker offloading** — Heavy simulation running on main thread blocking rendering; use Web Worker for game logic, transferable ArrayBuffers for data (web game projects with large entity counts)

### MEDIUM - Code review nightmare
- **Not using framework idioms** — Not leveraging the framework's recommended patterns and latest stable features
- **Performance** — No image/asset optimization, missing lazy loading, no caching strategy
- **Missing tests** — No unit tests, no integration tests for critical paths
- **No CI/CD** — No automated builds, no linting on PR, manual deployments (lower priority for solo/indie/game jam projects)
- **State management** — Prop drilling / global mutation / spaghetti state where framework provides better patterns (game singletons like GameManager/AudioManager are standard architecture, not spaghetti)
- **AI-generated aesthetic** — Generic design with no brand identity: purple gradients, single sans-serif font, three-column icon grids, uniform rounded corners, 0.1 opacity shadows on everything (web projects)
- **Missing keyboard navigation** — No keyboard shortcuts for power users (where applicable)
- **No monitoring** — No error tracking, no performance monitoring, no logging strategy
- **No input rebinding** — Hardcoded controls with no remapping option (game projects — accessibility baseline)
- **No game accessibility** — Missing colorblind mode, subtitles, or difficulty options (game projects — 2026 baseline)
- **No spatial partitioning** — Brute-force collision detection with many entities, no quadtree/octree/spatial hash (game projects)

### MINOR - Nitpick, but fix it
- **Inconsistent code style** — Mixed formatting, naming conventions, quote styles within the same file
- **Missing documentation** — Public APIs / exported functions without types or docs
- **Debug code in production** — console.log, print(), debug flags, TODO comments left in shipped code
- **Non-semantic markup** — div soup, inline styles (web projects)
- **Inconsistent naming** — Mixed camelCase/snake_case, unclear variable names
- **Audio mixing issues** — No separate volume controls for music/SFX/dialogue, or sound clipping from too many simultaneous voices (game projects)

---

## Embedded Checklists (Fallback)

Use this if `references/checklists.md` not found.

### Universal Checklist (applies to ALL projects)

**Security:**
- [ ] No secrets in code — API keys, tokens, passwords in env vars / secret manager only
- [ ] Input validation — Validate at system boundaries (user input, external APIs, file uploads)
- [ ] Injection prevention — ORM / parameterized queries / sanitized output (SQL, XSS, command injection)
- [ ] Dependency audit clean — No known CVEs
- [ ] CSRF protection — Anti-forgery tokens or SameSite cookies (web apps)
- [ ] Auth best practice — OAuth 2.1, OIDC, or framework-recommended auth patterns
- [ ] AI security (if applicable) — Prompt injection prevention, LLM output sanitization, AI tool permissions gated

**Performance:**
- [ ] Appropriate for platform — Web: Core Web Vitals (LCP ≤2.5s, INP ≤200ms, CLS ≤0.1) | Games: 60fps (16.6ms frame budget), consistent frame pacing | Mobile: startup <2s | API: p95 <200ms
- [ ] Asset optimization — Images (modern formats), fonts (font-display: swap), bundle sizes appropriate for target
- [ ] Caching strategy — HTTP caching, application-level caching, CDN where appropriate
- [ ] Lazy loading — Below-fold content, heavy dependencies, routes/pages loaded on demand (not applicable to games)
- [ ] No unnecessary computation — Database queries optimized, N+1 queries eliminated, pagination for large datasets

**Architecture:**
- [ ] Separation of concerns — No God objects/functions, clear module boundaries
- [ ] Clean dependency graph — No circular dependencies, clear import direction
- [ ] DRY without over-abstraction — Repeated code extracted, but no premature abstraction
- [ ] Error handling — Graceful failures, user-facing error messages, structured logging
- [ ] Configuration management — Environment-specific config separated from code
- [ ] Database patterns — Connection pooling, migrations versioned, proper ORM usage (if applicable)

**Testing:**
- [ ] Unit tests — Core business logic covered with meaningful assertions
- [ ] Integration tests — Critical paths tested (API endpoints, database operations, auth flows)
- [ ] E2E tests — Key user flows covered (where applicable)
- [ ] Test isolation — Tests don't depend on external services or shared state
- [ ] Coverage strategy — Coverage thresholds defined for critical modules (not vanity 100%)
- [ ] Mocking boundaries — External services mocked at integration boundary, not deep internals
- [ ] Test naming — Test names describe behavior, not implementation

**CI/CD:**
- [ ] Automated builds — Build runs on every PR
- [ ] Linting/formatting — Enforced in CI, not just local
- [ ] Tests run on PR — Failing tests block merge
- [ ] Deployment strategy — Automated deploy with rollback capability

**Accessibility (web/mobile):**
- [ ] WCAG 2.2 compliance — Focus rings, keyboard navigation, screen reader support
- [ ] Semantic markup — Correct elements for their purpose (button not div, nav not div, etc.)
- [ ] ARIA labels — Interactive elements accessible, landmarks defined
- [ ] Color contrast — 4.5:1 minimum for normal text, 3:1 for large text
- [ ] Reduced motion — Respect prefers-reduced-motion
- [ ] Focus management — Focus trap in modals, return focus on close

### Stack-Specific (applied ONLY when detected)

**React/Next.js:** RSC usage, Server Actions, React 19+ hooks (useActionState, useOptimistic), PPR, App Router patterns, error.tsx + loading.tsx
**Vue/Nuxt:** Composition API, auto-imports, Nuxt 4 patterns, defineModel, Pinia stores
**Svelte/SvelteKit:** Runes ($state, $derived, $effect), $props() for component props, server load functions, form actions
**Angular:** Signals, standalone components, zoneless change detection, control flow (@if, @for)
**Python:** Type hints (3.13+), async patterns (Django, FastAPI), proper ORM usage, virtual environments
**Go:** Error handling patterns (errors.Is/As), goroutine management, interfaces, go vet/staticcheck
**Rust:** Ownership patterns, error handling (Result/Option), unsafe audit, clippy clean
**PHP/Laravel:** Eloquent patterns, middleware, queue patterns, Laravel 12 features
**Astro:** Content Collections, island architecture, zero-JS by default, View Transitions
**Remix:** Loaders/actions, nested routes, progressive enhancement, error boundaries
**SolidJS:** Fine-grained signals, no virtual DOM, createResource, JSX without re-renders
**Hono:** Web Standards (Request/Response), middleware chains, edge-first, multi-runtime
**Ruby/Rails:** Active Record patterns, concerns, Hotwire/Turbo, N+1 prevention, Rails 8 conventions
**Elixir/Phoenix:** OTP/GenServer patterns, LiveView, fault tolerance, supervision trees
**C/C++:** RAII, smart pointers, memory safety, modern C++20/23, no raw new/delete
**Swift:** Structured concurrency (async/await, actors), protocol-oriented design, SwiftUI vs UIKit
**Kotlin:** Coroutines, null safety, multiplatform patterns, sealed classes, Flow
**Java / Kotlin (JVM):** Spring Boot dependency injection, JPA/Hibernate entity mapping, N+1 prevention, modern Java (records, sealed classes, virtual threads), JUnit 5 + Mockito
**C# / .NET:** Project architecture (web API / Blazor / WPF / MAUI), Entity Framework Core with migrations, async/await with cancellation tokens, nullable reference types enabled, dependency injection lifetimes
**Vanilla HTML/JS/CSS:** Progressive enhancement, semantic HTML, no-build patterns, Web Standards
**Mobile (React Native / Flutter / SwiftUI):** Platform conventions (iOS HIG / Material Design), framework navigation, appropriate state management, 60fps scrolling, VoiceOver/TalkBack support
**Unity:** Game loop separation (FixedUpdate/Update), object pooling, DOTS/ECS for data-heavy systems, Addressables, draw call batching
**Unreal Engine:** UPROPERTY/UFUNCTION macros, Blueprint vs C++ separation, Nanite/Lumen usage, PCG framework, actor lifecycle
**Godot:** Process separation (_physics_process/_process), signal patterns, Jolt physics (4.6+), @export variables, scene composition
**Phaser/Web 2D:** Scene lifecycle, sprite batching via texture atlases, audio context gesture handling, object pooling, WebGL context management
**Three.js/Web 3D:** WebGPU with WebGL 2 fallback, InstancedMesh for repeated geometry, dispose() for GPU memory, frame budget with delta time, frustum culling + LOD
**Bevy:** ECS architecture, Required Components (0.15+), AssetServer loading, plugin architecture, system scheduling with .before()/.after()
**Pygame/Love2D:** Fixed timestep game loop, assets loaded once at init, input abstraction with rebinding, no per-frame allocations, game state machine
**Web Canvas Game:** Worker thread offloading (sim in Worker, render on main), zero-copy ArrayBuffer transfer, TypedArray hot data (Int32Array/Float32Array for counters and spatial indices), adaptive quality auto-tuning, GC-free hot path (ring buffers, object pooling, swap-and-pop, dirty tracking)

---

## Embedded UI Patterns (Fallback)

Use this if `references/ui-patterns.md` not found.

### Layout Architecture
- **Bento Grid (Asymmetric)**: Visual hierarchy through size, not symmetry
- **View Transitions API**: Same-document (`startViewTransition()`) + cross-document (`@view-transition { navigation: auto; }`)
- **Anchor Positioning**: CSS `anchor-name` + `position-area` for popovers without JS

### Anti-AI-Slop Signals
- Red flags: purple/indigo gradients, single sans-serif font (Inter/Roboto), three-column icon grids, generic CTAs, uniform rounded corners, 0.1 opacity shadows everywhere
- Professional signals: intentional font pairing (serif+sans or display+body), semantic color system via CSS custom properties, 8px spacing grid, benefit-driven CTA language, social proof near decision points

### Visual Hierarchy
- **60-30-10 Color Rule**: 60% neutral, 30% primary, 10% accent
- **Semantic Color System**: CSS custom properties with semantic names (`--text-primary`, `--surface`, `--accent`), `light-dark()` for dark mode, `color-mix()` for hover states, oklch color space
- **Typography Scale**: Max 6 sizes, prefer rem over px. Font pairing critical (serif+sans, display+body). Single sans-serif = AI slop tell.
- **Spacing System**: 8px grid (industry standard). 4px for fine-granularity detail only.

### CSS 2026 Features (two tiers)
- **Production-Ready**: CSS nesting, `@scope`, `@starting-style`, `light-dark()`, `color-mix()`, relative color syntax, container queries (size), Popover API, anchor positioning, scroll-driven animations, cross-document View Transitions
- **Emerging (progressive enhancement only)**: `if()`, `sibling-index()`, `corner-shape`, customizable `<select>`, `field-sizing: content`, `text-box-trim`, `contrast-color()`

### Landing Page Patterns
- Hero: outcome-first headline, one supporting line, one primary CTA, one trust cue, one visual
- Section rhythm: hero → social proof → features → testimonials → CTA repeat → FAQ → final CTA
- Z-pattern for landing pages, F-pattern for content-heavy pages

### Interaction Design
- **Micro-Interactions**: 0.1s–0.2s, never >300ms, never linear
- **Loading States**: Skeleton > Spinner > Blank
- **Focus States**: WCAG 2.2 AAA (3px outline, 3px offset)

### Game UI Patterns (applies to games only)
- **HUD**: Health bars, minimaps, ammo counters, cooldown indicators — size + color for importance hierarchy; progressive disclosure; diegetic UI for immersion; screen-space vs world-space separation
- **Menus**: Pause, inventory, skill trees, crafting, dialogue — controller-navigable with visible focus; auto-switching input prompts (keyboard ↔ gamepad)
- **Tutorial**: Contextual tooltips, progressive disclosure of mechanics, one mechanic at a time, skip/replay option
- **Resolution**: UI anchoring for aspect ratios (16:9, 21:9, 4:3), DPI awareness, integer scaling for pixel art
- **Game Accessibility (2026 baseline)**: Subtitles (customizable), remappable controls, high contrast mode, colorblind modes, difficulty as accessibility, reduced motion toggle
- **Frame Budget (replaces CWV for games)**: 60fps = 16.6ms, 30fps = 33.3ms, 120fps (VR) = 8.3ms; object pooling; draw call batching; GC avoidance in hot loops; spatial partitioning for collision; worker thread offloading; zero-copy ArrayBuffer transfer; TypedArrays for hot data; adaptive quality auto-tuning; offscreen canvas caching; swap-and-pop removal; dirty tracking
- **Game Audio**: Separate volume (music/SFX/dialogue), sound pooling with max concurrent voices, spatial audio, music crossfading

### Anti-Patterns (context-dependent)
- Rainbow Gradients (>2 colors), Box-Shadow on static elements, Border-Radius 50% on rectangles
- Placeholder text as label (accessibility violation)
- Dead trends: carousel/slider heroes, parallax overload, auto-playing video with sound, generic stock illustrations (Undraw saturation)
- **Font choice is context-dependent**: System fonts are valid for performance-critical apps (zero FOIT/FOUT, zero CLS); custom fonts for brand-heavy apps
- **Scroll pattern is context-dependent**: Use pagination for searchable content; virtual/infinite scroll is standard for feeds and data tables
- **Game-specific — NOT anti-patterns**: Purple/neon gradients (valid game aesthetic), single font family (thematic cohesion), global singletons (GameManager/AudioManager = standard), large files (game simulation files), no lazy loading (games preload behind loading screens)

---

## Conditional Output Logic

Check user prompt for keywords ["-fix"], ["-features"] (or ["-feature"]), and ["-check"]. Both `-feature` and `-features` are valid and treated identically. Evaluate top-to-bottom, **first match wins** — do NOT combine branches:

1. **`-check -fix`** → Verification Process ONLY (no review) → append CHECK-FIX-PROMPT after VERDICT for failed items
2. **`-check`** alone → Verification Process ONLY (no review) → no fix prompt
3. **`-fix -features`** → Normal Review → FIX-PROMPT + FEATURE-PROMPT (3 separate blocks)
4. **`-fix`** alone → Normal Review → FIX-PROMPT (individual or phased if >10 issues)
5. **`-features`** alone → Normal Review → FEATURE-PROMPT (new features, no bugfixes)
6. **No flags** → Normal Review → "Shall I generate the fix prompt or feature ideas?"

**Key rule:** When `-check` is present (branches 1-2), it REPLACES the normal review entirely. The Verification Process runs INSTEAD OF steps 2-4 of the Analysis Process. Never produce both a review and a verification in the same output. The CHECK-FIX-PROMPT (branch 1) is derived from verification findings — it is NOT the same as the regular FIX-PROMPT (branch 4).

**`-check` syntax:**
- `-check` — auto-detect phase commits via pattern matching
- `-check N` (e.g. `-check 3`) — treat last N commits as phases

## Analysis Process

1. **Detect Stack** (Step 0): Check config files to identify tech stack and versions
2. **Gather Context**: Analyze file structure, entry points, core logic
3. **Load References**: Try external files first, fallback to embedded
4. **Brutal Review**: Categorize by severity (CRITICAL/MAJOR/MEDIUM/MINOR) — only apply stack-relevant items
5. **Apply Conditional Output Logic**

### Review Output (when no `-check` flag)

```
## BRUTAL REVIEW: [Topic]

### CRITICAL
- [Finding with file/line]

### MAJOR
- [Finding]

### MEDIUM
- [Finding]

### MINOR
- [Finding]

### VERDICT
One brutal sentence.
```

### Example

**Input:**
```bash
/brutal-honest Review my auth system -fix
```

**Output:**
```markdown
## BRUTAL REVIEW: Authentication

### CRITICAL
- Hardcoded API key - config.js:12 (security disaster)

### VERDICT
Security Swiss cheese. Fix CRITICAL before prod.

### FIX-PROMPT
"Fix auth system:
config.js:12 Move API_KEY to environment variables (.env file or platform secrets manager)
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

---

### Verification Process (when `-check` is used)

Replaces steps 2-4 above. Step 0 (Stack Detection) still runs first.

**Skeptical default:** Every item starts UNVERIFIED. A keyword match is NOT proof of correctness. You must Read actual file content and verify it matches the plan's intent.

**Priority order:** Complete Check 1 for ALL items first (it catches the most issues). Then Check 2 and Check 3 for items that passed Check 1. Then Check 4 for all Check 1 VERIFIED items. All four checks are mandatory — no skipping.

1. **Detect Phases**: Run `git log --format="%H %s" -20` and match commit subjects against phase patterns (see Phase Detection below). If `-check N` was given, take the last N commits directly.
2. **Extract Plan Items**: For each phase commit, run `git log -1 --format="%b" <hash>` to get the body. Each bullet point (`-`, `*`, `+`) or non-empty line = one plan item. If no body, the subject line = single item.
3. **Check 1 — Existence + Correctness**: For each plan item, search the codebase using Grep/Glob to locate it. Then **Read the actual file section** — do not stop at a Grep hit count. Verify the content semantically matches the plan intent. For code files, also verify syntax is valid (no obvious parse errors in the section you read). For markdown/docs, also verify internal links resolve and table structures are consistent (row counts, column counts, header alignment). Report: `VERIFIED` (content matches plan and is structurally sound) | `PRESENT_BUT_WRONG` (keyword/identifier exists but implementation doesn't match what the plan described — include detail) | `MISSING` (no codebase match at all) | `BROKEN` (exists but has syntax/structural errors).
4. **Check 2 — Cross-Reference Consistency**: Trace ALL references to each plan item across files. If the plan adds N entries to a table/list/config, verify that N corresponding entries exist at every referencing location. Count table rows, checklist sections, config entries — numbers must match. Report: `CONSISTENT` | `INCONSISTENT — [N in location A, M in location B]` | `ORPHANED — [dangling reference at file:line]`.
5. **Check 3 — Regression + Contradiction**: Run `git diff` against the phase commits to identify renames. Search for OLD names still lingering in the codebase (partial renames). Search for contradictions — two locations defining the same thing differently. Report: `CLEAN` | `STALE — [old name at file:line]` | `CONTRADICTION — [A says X at file:line, B says Y at file:line]`.
6. **Check 4 — Devil's Advocate**: Only runs on items that passed Check 1 as `VERIFIED`. For each: ask "What assumption does this rely on? What would break it?" Then actively search for the counterexample in the codebase. Report: `SURVIVED` (counterexample not found, assumption holds) | `CHALLENGED — [counterexample found: detail]`. Items not VERIFIED in Check 1 get "—" for this check. This check is mandatory — never skip it.
7. **Classify Findings**: Assign severity (CRITICAL/MAJOR/MEDIUM/MINOR) to each non-passing status using the Severity for Check Findings mapping. Apply the `PRESENT_BUT_WRONG` override (always MAJOR minimum).
8. **Output**: Render Verification report (see Verification Output below).

---

## Phase Detection (for `-check`)

### Commit Subject Patterns

Match commit subjects against these patterns (case-insensitive, checked in order):

| Pattern | Example |
|---------|---------|
| `/^phase\s*\d+/i` | "Phase 1: Add new recipes" |
| `/^step\s*\d+/i` | "Step 2: Update thresholds" |
| `/^#\d+\b/` | "#3: Speed unlock system" |
| `/^\d+[\.\)]\s/` | "1. Add recipes" / "1) Add recipes" |
| `/^\[phase\s*\d+\]/i` | "[Phase 1] Add new recipes" |
| `/^v?\d+\.\d+/` | "v2.7.2: Sync fallbacks" / "2.5.0: Polish" |

### Fallback Chain (if no patterns match)

1. All commits since last tag (`git describe --tags --abbrev=0`)
2. No tags → all commits since branch-point vs main (try master if main not found, then default branch) (`git merge-base HEAD main`)
3. No branch-point → last 5 commits

### Plan-Item Extraction

From each phase commit's body (`git log -1 --format="%b" <hash>`):
- Lines starting with `-`, `*`, `+` = individual plan items
- Non-empty, non-bullet lines = also plan items (one per line)
- Empty commit body → subject line = single item
- If commit body mixes prose paragraphs with bullet lists, extract only lines starting with `-`, `*`, `+`. Ignore prose lines (>80 chars without a leading bullet marker).
- Strip leading markers and whitespace before using as search keywords

---

## Sync Points (for `-check` Cross-Reference Verification)

When verifying that a new feature is integrated at ALL required locations, use the detected stack to determine sync points:

| Stack | Sync Points to Verify |
|-------|----------------------|
| Web Canvas Game | Config object + Worker script + Renderer class + UI class |
| React / Next.js | Component + API route/server action + Types/interfaces + Tests |
| Vue / Nuxt | Component + Composable/store + Types + Tests |
| Svelte / SvelteKit | Component + Server load/action + Types + Tests |
| Angular | Component + Service + Module/standalone config + Tests |
| Astro | Page/component + Content Collection schema + Integration config |
| Remix | Route module (loader/action) + Types + Error boundary + Tests |
| SolidJS | Component + createResource/store + Types + Tests |
| Hono | Route handler + Middleware + Validator schema + Tests |
| Python (Django) | Model + View/ViewSet + Serializer + URL conf + Migration |
| Python (FastAPI) | Route handler + Pydantic schema + Dependencies + Tests |
| Go | Handler + Model/struct + Repository/store + Tests |
| Rust | Module + Types/traits + Tests + Cargo.toml (if new dep) |
| PHP / Laravel | Controller + Model + Migration + Route + Tests |
| Ruby / Rails | Controller + Model + Migration + Route + Views + Tests |
| Elixir / Phoenix | Context module + LiveView/Controller + Schema + Router + Tests |
| C / C++ | Header (.h/.hpp) + Implementation (.c/.cpp) + CMakeLists.txt + Tests |
| Swift / iOS | View/ViewController + Model + Service + Tests |
| Kotlin | Activity/Fragment/Composable + ViewModel + Repository + Tests |
| Java / Kotlin (JVM) | Controller + Service + Repository + Entity + Tests |
| C# / .NET | Controller + Service + Model + DbContext + Tests |
| Flutter / Dart | Widget + Provider/Bloc + Repository + Model + Tests |
| React Native | Component + Navigation config + Native module (if any) + Tests |
| Unity | MonoBehaviour script + Prefab + Scene reference |
| Unreal Engine | C++ class + Blueprint + Level reference |
| Godot | GDScript/C# + Scene (.tscn) + Autoload (if singleton) |
| Three.js / Web 3D | Scene + Renderer setup + Asset loader/dispose + Frame loop + Tests |
| Phaser / PixiJS / Kaplay | Scene class + Asset manifest + Game config + State manager |
| Bevy | Plugin module + Components + Systems + Resources |
| Pygame / Love2D | Main loop + State machine + Asset loader + Input handler |
| Vanilla HTML/JS/CSS | Script section + DOM elements + CSS styles + Event handlers |

**Heuristic:** If a plan item introduces a new named entity (function, class, variable, resource, route), search for that name across ALL sync-point locations. If found in fewer locations than expected, report `INCONSISTENT`.

---

## Verification Output (for `-check`)

```
## VERIFICATION: [Topic derived from phase subjects]

**Worst finding:** [SEVERITY] — [one-line description of the single worst issue found, or "None" if all checks pass]

**Stack:** [detected] | **Phases:** N | **Commits:** short_hash..short_hash

---

### Phase 1: [Subject line] (short_hash)

| # | Item | Check 1: Exist+Correct | Check 2: Cross-Ref | Check 3: Regression | Check 4: Devil's Advocate | Severity |
|---|------|------------------------|---------------------|---------------------|---------------------------|----------|
| 1 | [item text] | ✓ VERIFIED | ✓ CONSISTENT | ✓ CLEAN | ✓ SURVIVED | — |
| 2 | [item text] | ⚠ PRESENT_BUT_WRONG — [detail] | — | — | — | MAJOR |
| 3 | [item text] | ✗ MISSING | — | — | — | MAJOR |

Every cell MUST be filled with a status or "—" (= not applicable). No silent skips.
Check 4 only runs on Check 1 VERIFIED items; all others get "—".

---

[Repeat for each phase...]

---

### NOT TESTED

Items or checks that were skipped and WHY. This section is MANDATORY even when empty:

- [Item X, Check Y: SKIPPED — reason] or "None — all items tested across all applicable checks."

---

### FINDINGS

#### CRITICAL
- [CONTRADICTION / BROKEN with syntax or runtime errors / CHALLENGED proving feature can't work]

#### MAJOR
- [PRESENT_BUT_WRONG / INCONSISTENT cross-refs / MISSING items / CHALLENGED with significant gaps]

#### MEDIUM
- [STALE references / ORPHANED refs / edge-case CHALLENGED]

#### MINOR
- [Style issues / SKIPPED checks / theoretical CHALLENGED]

(Omit empty severity sections)

---

### VERDICT
**Worst severity:** [CRITICAL/MAJOR/MEDIUM/MINOR/NONE]
**Phase 1:** X/Y passed
**Phase 2:** X/Y passed
**Total: X/Y** — Any CRITICAL = 0% regardless of other scores.

[One brutal sentence that references the actual worst finding, not generic praise. If everything passed, explain what was tested and why it held up — not just "looks good."]
```

### CHECK-FIX-PROMPT (if `-check -fix` combined)

Appended after VERDICT when both flags are present:

```
### CHECK-FIX-PROMPT

Context: Verification of [N] phases found [M] issues ([C] CRITICAL, [J] MAJOR, [D] MEDIUM, [I] MINOR)

[For each failed item, ordered by severity:]
Phase [X], Item [Y]: "[item text]"
  Failed check: Check [N] — [check name]
  Status: [STATUS — detail]
  Mismatch: [what the plan said vs what the code actually does]
  Fix: [specific action with file:line reference]

Rules: NO AI comments, preserve existing functionality, test after change.
After applying fixes, re-run `-check` to confirm all items now pass.
```

> 10 failed items: Phase fix prompts in batches — CRITICAL first, then MAJOR, then MEDIUM+MINOR.

### Severity for Check Findings

Status-to-severity mapping:

- **CRITICAL** — `CONTRADICTION` (two locations define same thing differently), `BROKEN` (syntax errors, structural failures, compilation errors), `CHALLENGED` that proves a feature fundamentally cannot work as designed
- **MAJOR** — `PRESENT_BUT_WRONG` (keyword exists but implementation doesn't match plan intent), `INCONSISTENT` cross-references (N entries in one table but M in another), `MISSING` plan items with no codebase match, `CHALLENGED` exposing significant functional gaps
- **MEDIUM** — `STALE` references to old/renamed identifiers, `ORPHANED` dangling references that don't break functionality, edge-case `CHALLENGED` findings
- **MINOR** — Style issues in new code, `SKIPPED` checks with valid reasons, theoretical `CHALLENGED` findings unlikely to manifest

**Severity override:** `PRESENT_BUT_WRONG` is always MAJOR minimum — it is strictly worse than `MISSING`, because code that looks correct but isn't creates false confidence. A missing feature is obvious; a wrong implementation hides behind a keyword match.

---

## Error Messages

- **No files:** "No files to tear apart. Give me code or images."
- **Unclear topic:** "What exactly? Code? UI? Architecture? Be specific."
- **No git repo (`-check`):** "Not a git repository. `-check` needs commit history to verify against."
- **No phase commits (`-check`):** "No phase commits found. Use `-check N` to specify how many recent commits to verify, or write commit messages with 'Phase N:' / 'Step N:' prefixes."
- **Empty commit bodies (`-check`):** "Commit bodies are empty — using subject lines as single items per phase. For better verification, write detailed commit messages with bullet-point plan items."
- **Image error:** "Image format not supported or image not found."

## Rules

- NO AI comments in code
- Preserve existing features when fixing
- Max 30 files context
- Check current stable version of the detected framework using available tools or training knowledge — flag anything 2+ major versions behind; if unable to verify, state the assumption
- AI security is CRITICAL — never downplay prompt injection
- Always attempt external references first, but NEVER fail if not found
- Only apply stack-specific checklist items when that stack is detected
- When a game engine is detected, suppress web-only rules (Core Web Vitals, landing page patterns, anti-AI-slop font/gradient rules, lazy loading, CI/CD as MEDIUM). Apply game-specific performance metrics and patterns instead.

See [CHANGELOG.md](./CHANGELOG.md) for version history.
