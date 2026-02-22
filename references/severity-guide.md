# Severity Guide

---

## CRITICAL

Fix yesterday.

- **AI vulnerabilities** — Prompt injection (user input reaches LLM unsanitized), AI tools with DELETE permissions without confirmation, auth tokens in AI conversation logs
- **Security** — Hardcoded secrets, SQL injection, XSS, CSRF without protection, no input validation at system boundaries
- **Accessibility** — WCAG 2.2 violations = legal risk
- **Stability** — Crashes, infinite loops, memory leaks, unhandled exceptions in critical paths
- **Data loss** — No backups, no transaction safety, destructive operations without confirmation
- **Soft-lock** — Player cannot progress and cannot load a prior save (game projects)
- **Save corruption** — Save data can be lost or corrupted without detection/recovery (game projects)
- **Determinism failure** — Desyncs in multiplayer from non-deterministic simulation (multiplayer games)

## MAJOR

Ship blocker.

- **Outdated framework** — Using a version 2+ major versions behind current stable release (check against latest stable, not a hardcoded version number; for game engines: locking to an LTS version is standard; flag only if engine version is end-of-life or unsupported)
- **Architecture** — 2000+ line files without clear section separation (unless architecturally intentional, e.g., single-file apps, Go's copy-over-dependency idiom), God objects/functions, circular dependencies
- **Type safety** — No strict mode (where language supports it), `any`/`object`/`interface{}` everywhere at boundaries
- **UX** — No loading states, broken mobile/responsive, no error feedback to users
- **AI Architecture** — Client-side LLM calls instead of server-side (exposes API keys)
- **Framework-specific rendering errors** — Hydration mismatches (SSR frameworks), template compilation errors, runtime binding failures
- **Missing error handling** — No try/catch in async operations, silent failures, unhandled promise rejections / panics / exceptions
- **No object pooling** — Frequently spawned objects (bullets, particles, enemies) allocated/destroyed per frame causing GC pauses (game projects)
- **Wrong update loop** — Physics in render loop or rendering in physics loop, causing frame-rate-dependent behavior (game projects)
- **No frame budget discipline** — No profiling, no performance targets, frame time exceeds 16.6ms in core gameplay (game projects)
- **No worker offloading** — Heavy simulation running on main thread blocking rendering; use Web Worker for game logic, transferable ArrayBuffers for data (web game projects with large entity counts)

## MEDIUM

Code review nightmare.

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

## MINOR

Nitpick, but fix it.

- **Inconsistent code style** — Mixed formatting, naming conventions, quote styles within the same file
- **Missing documentation** — Public APIs / exported functions without types or docs
- **Debug code in production** — console.log, print(), debug flags, TODO comments left in shipped code
- **Non-semantic markup** — div soup, inline styles (web projects)
- **Inconsistent naming** — Mixed camelCase/snake_case, unclear variable names
- **Audio mixing issues** — No separate volume controls for music/SFX/dialogue, or sound clipping from too many simultaneous voices (game projects)

---

**Tone:** Direct. "This framework version is 2+ major releases behind current stable. Update or justify."
