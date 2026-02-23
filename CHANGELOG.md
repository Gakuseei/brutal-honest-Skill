# Changelog

### 2.7.2 (2026-02-23)
- **Made Check 4 (Devil's Advocate) mandatory** — removed all "best-effort", "context budget", and skip language. All four checks must run on every `-check` invocation, no exceptions
- **Routed Flutter/Swift detection to Mobile checklist** — detection table now says `Flutter-specific + Mobile + Universal` and `Swift-specific + Mobile + Universal`, matching the Mobile checklist that names them
- **Restored internal link validation in Check 1** — lost when merging old Check 4 (Structural Validation) into Check 1 in v2.7.0; Check 1 now explicitly verifies internal links and table structure for markdown/docs
- **Unified version-check phrasing** — Input Handling (line 68) and Rules (line 577) now use identical language including the "flag anything 2+ major versions behind" clause
- **Fixed CHANGELOG v2.7.0 column count** — was still wrong ("to 5" → "to 6" → now correctly "to 7" with full breakdown)

### 2.7.1 (2026-02-23)
- **Added Three.js/Web 3D sync point** — was the only detected stack with a dedicated checklist but no sync point entry
- **Fixed CHANGELOG column count claim** — was "7 columns to 5", corrected to "7 columns to 7" (added # row number, reduced checks from 5 to 4)
- **Fixed README body text** — "31+" updated to "32+", React Native added to Native category in summary table
- **Aligned Mobile checklist label** — embedded now says "Mobile (React Native / Flutter / SwiftUI)" matching references/checklists.md
- **Fixed stale version-check in Rules section** — now matches softened language from Input Handling ("if unable to verify, state the assumption")

### 2.7.0 (2026-02-23)
- **Synced embedded fallbacks with reference files** — embedded Security checklist now has 7 items (was 5), Testing has 7 (was 3), Accessibility has 6 (was 4), Architecture has 6 (was 4), Performance has 5 (was 3), CI/CD has 4 (was 3). Paste-SKILL.md users now get full coverage instead of a weaker subset
- **Added 16 missing sync points** — Sync Points table now covers all 30+ detected stacks (was 14). Added: Astro, Remix, SolidJS, Hono, Ruby/Rails, Elixir/Phoenix, C/C++, Swift, Kotlin, Java/Kotlin JVM, C#/.NET, Flutter/Dart, React Native, Phaser/PixiJS/Kaplay, Bevy, Pygame/Love2D
- **Simplified verification pipeline from 5 checks to 4** — merged Structural Validation into Check 1 (verify syntax when reading files), made Devil's Advocate best-effort with explicit budget guidance. Table format changed from 7 columns to 7 (added # row number, reduced checks from 5 to 4; was Item + Check 1-5 + Severity, now # + Item + Check 1-4 + Severity)
- **Added priority order for checks** — Check 1 for all items first (catches most issues), then Check 2+3 for passed items, then Check 4. (Note: Check 4 was originally best-effort; made mandatory in v2.7.2)
- **Moved changelog to CHANGELOG.md** — saves ~90 lines of token overhead per skill invocation
- **Removed AI Model Compatibility section from SKILL.md** — already in README, was wasting ~10 lines of prompt context
- **Added React Native detection** — `package.json` with `"react-native"` now detected as Mobile stack
- **Added React Native / Mobile embedded checklist** — platform conventions, navigation, state management, 60fps, VoiceOver/TalkBack
- **Softened version-check language** — now says "check using available tools or training knowledge; if unable to verify, state the assumption" instead of implying internet access
- Fixed README badge count (was "31+" for 30 actual stacks)
- Updated CONTRIBUTING.md with embedded-sync and changelog reminders

### 2.6.0 (2026-02-23)
- **Breaking: Replaced 4-check keyword-grep pipeline with skeptical 5-check pipeline** — every item starts UNVERIFIED, keyword match != proof
- Check 1 (Existence + Correctness): Must Read actual file content, not just Grep hit counts. New status `PRESENT_BUT_WRONG` for keyword-matches-but-implementation-doesn't-match
- Check 2 (Cross-Reference Consistency): Traces ALL references across files, counts table rows (N detection entries -> N checklist sections). Statuses: `CONSISTENT` / `INCONSISTENT` / `ORPHANED`
- Check 3 (Regression + Contradiction): Uses `git diff` for renames, searches for contradictions (two locations defining same thing differently). Statuses: `CLEAN` / `STALE` / `CONTRADICTION`
- Check 4 (Structural Validation): Code syntax + run tests if they exist; docs/markdown: validate internal links, table consistency. Statuses: `VALID` / `BROKEN` / `SKIPPED`
- Check 5 (Devil's Advocate): Only runs on Check 1 VERIFIED items. For each: "What assumption does this rely on? What would break it?" Searches for the counterexample. Statuses: `SURVIVED` / `CHALLENGED`
- Anti-confirmation-bias statuses: Replaced FOUND/COMPLETE/CLEAN/PASS with statuses that encode failure modes, not just presence
- Severity override: `PRESENT_BUT_WRONG` is always MAJOR minimum — worse than MISSING, because it creates false confidence
- Weighted verdicts: Any CRITICAL = 0% regardless of other scores. Verdict leads with worst severity, not percentage
- Verification output redesign: "Worst finding:" at the very top, per-phase 7-column table (Item + Check 1-5 + Severity), mandatory NOT TESTED section
- CHECK-FIX-PROMPT now shows: which check failed, what the status was, concrete mismatch, specific fix, and "re-run `-check` to confirm"

### 2.5.0 (2026-02-23)
- Split Python detection into 3 rows: Python (Django), Python (FastAPI), Python (generic) — eliminates dangling sync-point references
- Renamed Pygame/Love2D/Raylib checklist to Pygame/Love2D — Raylib had no detection logic
- Added monorepo handling guidance: >3 config files triggers multi-project analysis with proportional file limits
- Documented Vision tool usage for screenshot/mockup analysis (PNG, JPG, WebP)
- Added game engine version exception to Input Handling (LTS versions are standard, only flag end-of-life)
- Clarified plan-item extraction: mixed prose/bullet commits now extract bullets only (>80 char prose lines ignored)
- Clarified git merge-base fallback chain: tries main, then master, then default branch
- Improved Reference Loading wording: "external overrides embedded; fill gaps with embedded defaults"
- Added image error message to Error Messages section
- Added "Why This?" differentiation section to README
- Added "What's New" line to README
- Added .gitignore and CONTRIBUTING.md for repo hygiene

### 2.4.0 (2026-02-23)
- Added `-check` flag: Post-mortem verification of implementation against git commit history
- Phase detection via 5 commit subject patterns (Phase N, Step N, #N, numbered, [Phase N]) with 3-level fallback chain
- Plan-item extraction from commit bodies (bullet points and non-empty lines)
- 4-check verification pipeline: Existence (Grep/Glob), Integration (stack-specific sync points), Regression (broken refs, orphans), Runtime (syntax/compile)
- Sync-point table for 14 stacks defining where new features must be wired in
- Hybrid output format: per-phase tally table + severity-classified findings + percentage verdict
- `-check -fix` combination: generates targeted fix prompts for failed verification items
- Added 3 check-specific error messages (no git repo, no phase commits, empty bodies)
- Added `Grep` to allowed-tools for codebase searching
- Expanded `Bash` access from `git:*` only to full (needed for runtime checks: node --check, go vet, cargo check, etc.)

### 2.3.0 (2026-02-22)
- Added game development support: 11 game engine/framework detection entries (Unity, Unreal, Godot, Phaser, Three.js, PixiJS, Kaplay, Bevy, Love2D, Pygame, Web Canvas)
- Added 9 game-engine-specific checklists (5 items each): Unity, Unreal Engine, Godot, Phaser/Web 2D, Three.js/Web 3D, Bevy, Pygame/Love2D/Raylib, Web Canvas Game
- Added game-specific severity items: CRITICAL (soft-lock, save corruption, determinism failure), MAJOR (no object pooling, wrong update loop, no frame budget), MEDIUM (no input rebinding, no game accessibility, no spatial partitioning), MINOR (audio mixing issues)
- Added Game UI Patterns section: HUD design, menu architecture, tutorial/onboarding, resolution independence, game accessibility (2026 baseline), frame budget model, game audio
- Added context guards to suppress web-only false positives on game projects (CWV, lazy loading, anti-AI-slop font/gradient rules, CI/CD as MEDIUM, global state)
- Added "Game-specific -- NOT anti-patterns" block: purple/neon gradients, single font family, global singletons, large files, no lazy loading
- Extended Glob pattern with game file extensions (.gd, .tscn, .tres, .unity, .uproject, .lua)
- Updated framework version guard for game engines (LTS versions are standard, flag only end-of-life)

### 2.2.0 (2026-02-22)
- Added Anti-AI-Slop Checklist: 12 red flags and 10 professional signals for identifying AI-generated-looking websites
- Added Landing Page Patterns: hero section design, above-the-fold 5-element checklist, CTA design, social proof placement, section rhythm, Z/F reading patterns
- Rewrote CSS 2026 Features into Production-Ready (11 features) vs Emerging (8 features) with browser support tiers
- Added 10+ production-ready CSS features: CSS nesting, @scope, @starting-style, light-dark(), color-mix(), relative color syntax, container queries, Popover API, scroll-driven animations, cross-document View Transitions
- Added semantic color system guidance: oklch, CSS custom properties, light-dark(), color-mix() for states
- Added font pairing strategies: serif+sans, display+body, variable fonts, anti-patterns (single font = AI slop)
- Fixed anchor positioning: `inset-area` -> `position-area`, `position-try-options` -> `position-try-fallbacks`
- Fixed Core Web Vitals thresholds to match Google's actual values (LCP <=2.5s, INP <=200ms, CLS <=0.1)
- Updated spacing system from 4px to 8px grid (Apple HIG / Material Design standard)
- Updated Django reference from 5.x to 6.x (async views, background tasks, CSP)
- Added 5th Svelte checklist item: $props() rune for component props
- Added View Transitions Level 2 (cross-document via CSS @view-transition)
- Added dead trends to anti-patterns: carousel heroes, parallax overload, auto-playing video, generic stock illustrations
- Added AI-generated aesthetic as MEDIUM severity item
- Added Interop 2026 annotations for anchor positioning, scroll-driven animations, View Transitions

### 2.1.0 (2026-02-22)
- Added 10 new stack-specific checklists: Astro, Remix, SolidJS, Hono, Ruby/Rails, Elixir/Phoenix, C/C++, Swift, Kotlin
- Detection table expanded from 16 to 22 stacks
- Each new stack has 5 concrete checklist items with framework-specific patterns and anti-patterns
- Glob pattern extended for .ex, .exs, .cpp, .hpp, .c, .h, .erl

### 2.0.0 (2026-02-22)
- Rewrote entire skill to be tech-stack agnostic (was React/Next.js-only)
- Added Stack Detection (Step 0) with 16 supported stacks
- Added multi-stack project support for monorepos
- Split checklists into Universal + Stack-Specific sections
- Added stack-specific checklists: Vue, Svelte, Angular, Python, Go, Rust, PHP, Vanilla HTML, Mobile
- Added Testing and CI/CD universal checklists
- Added CSS 2026 features (if(), sibling-index(), Scroll State Queries, corner-shape, customizable select)
- Fixed "System Fonts" and "Infinite Scroll" anti-patterns to be context-dependent
- Replaced JSX/TSX examples with framework-agnostic HTML
- Removed vendor-specific paths (Kimi CLI hardcoding)
- Removed hardcoded version numbers -- now checks "2+ major versions behind current stable"
- Added AI Model Compatibility section
- Added Mobile UI Patterns section
- Deleted duplicate `brutal-honest-Skill-1.0.0/` subfolder
- Fixed file limit contradiction (unified to 30)
