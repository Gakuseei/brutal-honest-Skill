---
name: brutal-honest
description: Ruthless expert analysis for code/UI/architecture. Tech-stack agnostic.
author: gakuseei
author_url: https://github.com/Gakuseei
version: 2.0.0
last-updated: 2026-02-22
allowed-tools: [Read, Glob, Bash(git:*), LS, Vision]  # Parsed by Kimi CLI skill loader; ignored by other AI tools
Limits: Read max 30 files, Glob max 100 results, Vision max 20 images
---

# brutal-honest

Ruthless senior-level analysis. No sugarcoating. Works with any tech stack.

## AI Model Compatibility

This skill is AI-model agnostic. It works with any LLM that supports skill/prompt loading:
- Claude (Opus, Sonnet, Haiku) via Claude Code skills
- GPT (4.x, 5.x, Codex) via custom instructions or system prompts
- Gemini (2.x, 3.x Pro) via gems or system instructions
- Kimi (K2.5+) via Kimi CLI skills
- DeepSeek, Grok, or any other model — paste SKILL.md as system context

No vendor-specific paths or APIs are assumed.

## When to Use

Use when user wants honest, direct feedback about code, UI, or architecture.

## Stack Detection (Step 0 — before analysis)

Detect the project's tech stack before applying any checklist. This determines which severity items and checklist sections are relevant.

| File Found | Stack | Checklist Focus |
|---|---|---|
| `package.json` with `"react"` | React/Next.js | React-specific + Universal |
| `package.json` with `"vue"` | Vue/Nuxt | Vue-specific + Universal |
| `package.json` with `"svelte"` | Svelte/SvelteKit | Svelte-specific + Universal |
| `package.json` with `"@angular"` | Angular | Angular-specific + Universal |
| `requirements.txt` / `pyproject.toml` | Python | Python-specific + Universal |
| `go.mod` | Go | Go-specific + Universal |
| `Cargo.toml` | Rust | Rust-specific + Universal |
| `composer.json` | PHP/Laravel | PHP-specific + Universal |
| `pom.xml` / `build.gradle` | Java/Kotlin | JVM-specific + Universal |
| `*.csproj` / `*.sln` | C#/.NET | .NET-specific + Universal |
| `pubspec.yaml` | Flutter/Dart | Flutter-specific + Universal |
| `astro.config.*` | Astro | Astro-specific + Universal |
| `package.json` with `"@remix-run"` | Remix | Remix-specific + Universal |
| `package.json` with `"solid-js"` | SolidJS | Solid-specific + Universal |
| `package.json` with `"hono"` | Hono | Hono-specific + Universal |
| `*.html` (standalone, no framework) | Vanilla HTML/JS/CSS | Web standards + Universal |
| No recognizable config | Ask user | — |

**Important:** Only apply stack-specific items when that stack is detected. A Python API does not get React checklist items. A vanilla HTML game does not get Next.js items.

**Multi-stack projects:** If multiple config files are detected (e.g., `package.json` + `pyproject.toml` in a monorepo), apply ALL matching stack-specific checklists. Review each sub-project against its own stack.

## Input Handling

**Context Awareness:** Check for framework config files (package.json, pyproject.toml, go.mod, etc.) to detect versions. Flag outdated framework versions (2+ major versions behind current stable). Always check current stable version of the detected framework.

**Files/Folders:**
- Single file: Read it
- Multiple files:
  - Glob: Use pattern appropriate to detected stack. Default: `**/*.{js,ts,jsx,tsx,vue,svelte,astro,html,css,py,go,rs,java,kt,php,rb,cs,dart,swift}`
  - Prioritize: 1) Config files (package.json, pyproject.toml, go.mod, etc.) 2) Entry points 3) Core logic
  - Max 30 files: If more, analyze entry points + configs only
- Images: Analyze pixel-perfect for colors, contrast, typography, spacing

## Reference Loading

**STEP 1: Locate Skill Directory**
- Try reading from skill installation path: `./references/[file]`
- Check your AI tool's skill/plugin directory for the `references/` folder (e.g., `~/.claude/skills/brutal-honest/references/`, project-level `.agents/skills/brutal-honest/references/`, or equivalent)

**STEP 2: Attempt to Load (in order)**
1. `references/severity-guide.md` → If NOT FOUND, use "Embedded Severity Guide" below
2. `references/checklists.md` → If NOT FOUND, use "Embedded Checklists" below
3. `references/ui-patterns.md` → If NOT FOUND, use "Embedded UI Patterns" below

**STEP 3: If files found, merge with embedded**
- External files OVERRIDE embedded defaults (for customization)
- If external incomplete, fill gaps with embedded

---

## Embedded Severity Guide (Fallback)

> These embedded sections are fallbacks only. If you have the `references/` folder installed, the external files take priority and these are ignored.

Use this if `references/severity-guide.md` not found.

### CRITICAL - Fix yesterday
- **AI vulnerabilities**: Prompt injection (user input reaches LLM unsanitized), AI tools with DELETE permissions without confirmation, auth tokens in AI conversation logs
- **Security**: Hardcoded secrets, SQL injection, XSS, no input validation, CSRF without protection
- **Accessibility**: WCAG 2.2 violations = legal risk
- **Stability**: Crashes, infinite loops, memory leaks, unhandled exceptions in critical paths

### MAJOR - Ship blocker
- **Outdated framework**: Using a framework version 2+ major versions behind current stable release
- **Architecture**: 2000+ line files without clear section separation (unless architecturally intentional, e.g., single-file apps, embeddable widgets), copy-paste code, God objects/functions, circular dependencies
- **Type safety**: No strict mode (where applicable), untyped interfaces at boundaries
- **UX**: No loading states, broken mobile, no error feedback to users
- **AI Architecture**: Client-side LLM calls instead of server-side (exposes API keys)
- **Framework-specific rendering errors**: Hydration mismatches, template compilation errors, runtime binding failures
- **Missing error handling**: Silent failures, no try/catch in async operations, unhandled promise rejections

### MEDIUM - Code review nightmare
- **Not using framework idioms**: Not leveraging the framework's recommended patterns and latest stable features
- **Performance**: No image optimization, missing lazy loading, no caching strategy
- **Missing tests**: No unit tests, no integration tests, no E2E tests for critical paths
- **No CI/CD pipeline**: No automated builds, no linting on PR, manual deployments
- **State management**: Prop drilling / global mutation where framework provides better patterns
- **Missing keyboard shortcuts**: No keyboard navigation for power users (where applicable)

### MINOR - Nitpick, but fix it
- **Inconsistent code style**: Mixed formatting, naming conventions, quote styles in same file
- **Missing documentation**: Public APIs without types/docs
- **Debug code in production**: console.log, print(), debug flags left in code
- **Div soup / non-semantic markup**: Where applicable to web projects

---

## Embedded Checklists (Fallback)

Use this if `references/checklists.md` not found.

### Universal Checklist (applies to ALL projects)

**Security:**
- [ ] No secrets in code — API keys in env vars / secret manager only
- [ ] Input validation — Validate at system boundaries (user input, external APIs)
- [ ] Injection prevention — ORM / parameterized queries / sanitized output
- [ ] Dependency audit clean — No known CVEs
- [ ] AI security (if applicable) — Prompt injection prevention, LLM output sanitization

**Performance:**
- [ ] Appropriate for platform — Web Vitals for web, startup time for mobile, response time for APIs
- [ ] No unnecessary computation — Caching, lazy loading, pagination where appropriate
- [ ] Asset optimization — Images, fonts, bundle sizes appropriate for target

**Architecture:**
- [ ] Separation of concerns — No God objects/functions
- [ ] Clean dependency graph — No circular dependencies
- [ ] DRY without over-abstraction — Repeated code extracted, but no premature abstraction
- [ ] Error handling — Graceful failures, user-facing error messages, logging

**Testing:**
- [ ] Unit tests exist for core logic
- [ ] Integration tests for critical paths
- [ ] E2E tests for key user flows (where applicable)

**Accessibility (web/mobile):**
- [ ] WCAG 2.2 compliance — Focus rings, keyboard nav
- [ ] Semantic markup — Correct elements for their purpose
- [ ] Color contrast — 4.5:1 minimum
- [ ] Reduced motion — Respect prefers-reduced-motion

### Stack-Specific (applied ONLY when detected)

**React/Next.js:** RSC usage, Server Actions, React 19.2 hooks (useActionState, useOptimistic), PPR, App Router patterns, error.tsx + loading.tsx
**Vue/Nuxt:** Composition API, auto-imports, Nuxt 4 patterns, defineModel, Pinia stores
**Svelte/SvelteKit:** Runes ($state, $derived, $effect), server load functions, form actions
**Angular:** Signals, standalone components, zoneless change detection, control flow (@if, @for)
**Python:** Type hints (3.13+), async patterns, proper ORM usage, virtual environments
**Go:** Error handling patterns (errors.Is/As), goroutine management, interfaces, go vet/staticcheck
**Rust:** Ownership patterns, error handling (Result/Option), unsafe audit, clippy clean
**PHP/Laravel:** Eloquent patterns, middleware, queue patterns, Laravel 12 features
**Vanilla HTML/JS/CSS:** Progressive enhancement, semantic HTML, no-build patterns, Web Standards

---

## Embedded UI Patterns (Fallback)

Use this if `references/ui-patterns.md` not found.

### Layout Architecture
- **Bento Grid (Asymmetric)**: Visual hierarchy through size, not symmetry
- **View Transitions API**: `document.startViewTransition()` for navigation
- **Anchor Positioning**: CSS `anchor-name` for popovers without JS

### Visual Hierarchy
- **60-30-10 Color Rule**: 60% neutral, 30% primary, 10% accent
- **Typography Scale**: Max 6 sizes, prefer rem over px
- **Spacing System**: 4px grid (0.25rem, 0.5rem, 1rem, 1.5rem, 2rem, 3rem)

### Interaction Design
- **Micro-Interactions**: 0.1s–0.2s, never >300ms, never linear
- **Loading States**: Skeleton > Spinner > Blank
- **Focus States**: WCAG 2.2 AAA (3px outline, 3px offset)

### Anti-Patterns (context-dependent)
- Rainbow Gradients (>2 colors), Box-Shadow on static elements, Border-Radius 50% on rectangles
- Placeholder text as label (accessibility violation)
- **Font choice is context-dependent**: System fonts are valid for performance-critical apps (zero FOIT/FOUT, zero CLS); custom fonts for brand-heavy apps
- **Scroll pattern is context-dependent**: Use pagination for searchable content; virtual/infinite scroll is standard for feeds and data tables

---

## Conditional Output Logic

Check user prompt for keywords ["-fix"] and ["-features"].

**IF "-fix" only** → Review + FIX-PROMPT (individual or phased if >10 issues)
**IF "-features" only** → Review + FEATURE-PROMPT (new features, no bugfixes)
**IF "-fix -features"** → Review + FIX-PROMPT + FEATURE-PROMPT (3 separate blocks)
**IF no flags** → Review + "Shall I generate the fix prompt or feature ideas?"

## Analysis Process

1. **Detect Stack** (Step 0): Check config files to identify tech stack and versions
2. **Gather Context**: Analyze file structure, entry points, core logic
3. **Load References**: Try external files first, fallback to embedded
4. **Brutal Review**: Categorize by severity (CRITICAL/MAJOR/MEDIUM/MINOR) — only apply stack-relevant items
5. **Apply Conditional Output Logic**

## Output Structure

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

## Example

**Input:**
```bash
/skill:brutal-honest Review my auth system -fix
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

## Error Messages

- **No files:** "No files to tear apart. Give me code or images."
- **Unclear topic:** "What exactly? Code? UI? Architecture? Be specific."

## Rules

- NO AI comments in code
- Preserve existing features when fixing
- Max 30 files context
- Always check current stable version of the detected framework — flag anything 2+ major versions behind
- AI security is CRITICAL — never downplay prompt injection
- Always attempt external references first, but NEVER fail if not found
- Only apply stack-specific checklist items when that stack is detected

## Changelog

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
- Removed hardcoded version numbers — now checks "2+ major versions behind current stable"
- Added AI Model Compatibility section
- Added Mobile UI Patterns section
- Deleted duplicate `brutal-honest-Skill-1.0.0/` subfolder
- Fixed file limit contradiction (unified to 30)
