# brutal-honest

<p align="center">
  <img src="https://img.shields.io/badge/version-2.0.0-red?style=for-the-badge&logo=semver&logoColor=white" alt="Version">
  <img src="https://img.shields.io/badge/last_updated-2026--02--22-blue?style=for-the-badge" alt="Last Updated">
  <img src="https://img.shields.io/badge/license-MIT-green?style=for-the-badge&logo=opensourceinitiative&logoColor=white" alt="License">
  <img src="https://img.shields.io/badge/status-stable-brightgreen?style=for-the-badge" alt="Status">
</p>

<p align="center">
  <b>Ruthless expert analysis for code, UI, and architecture. Any tech stack.</b><br>
  <i>No sugarcoating. No compromises. Just brutal honesty.</i>
</p>

<p align="center">
  <a href="#-quick-start">Quick Start</a> &bull;
  <a href="#-supported-stacks">Supported Stacks</a> &bull;
  <a href="#-usage">Usage</a> &bull;
  <a href="#-severity-levels">Severity</a> &bull;
  <a href="#-installation">Installation</a> &bull;
  <a href="#-architecture">Architecture</a>
</p>

---

## Quick Start

```bash
# Clone the repository
git clone https://github.com/Gakuseei/brutal-honest-Skill.git

# Install for Claude Code
cp -r brutal-honest-Skill ~/.claude/skills/brutal-honest

# Or project-level (any AI tool)
cp -r brutal-honest-Skill ./.agents/skills/brutal-honest

# Or paste SKILL.md as system context in any LLM
```

**Start analyzing:**
```bash
/skill:brutal-honest Review my codebase -fix
```

---

## Features

<table>
<tr>
<td width="50%">

### Comprehensive Analysis
- **Security:** Prompt injection, XSS, SQLi, secrets, CSRF
- **Performance:** Core Web Vitals, asset optimization, caching
- **Architecture:** Separation of concerns, dependency graph, error handling
- **Testing:** Unit, integration, E2E coverage checks
- **CI/CD:** Automated builds, linting, deployment strategy

</td>
<td width="50%">

### Smart Output Modes
- **Review Only:** Quick analysis without suggestions
- **`-fix`:** Actionable FIX-PROMPT with file:line references
- **`-features`:** Discover new feature opportunities
- **Both:** Complete analysis &rarr; fixes &rarr; features (3 blocks)

</td>
</tr>
<tr>
<td width="50%">

### Tech-Stack Agnostic
- **Auto-detects** your stack from config files
- **16 stacks** supported out of the box
- **Universal checklist** applied to every project
- **Stack-specific** items only when that stack is detected
- **Multi-stack** monorepo support

</td>
<td width="50%">

### AI-Model Agnostic
- **Claude** (Opus, Sonnet, Haiku) via Claude Code skills
- **GPT** (4.x, 5.x, Codex) via custom instructions
- **Gemini** (2.x, 3.x Pro) via gems or system instructions
- **Kimi** (K2.5+) via Kimi CLI skills
- **Any LLM** &mdash; paste SKILL.md as system context

</td>
</tr>
</table>

---

## Supported Stacks

Auto-detects and applies relevant checklists. Only stack-specific items for detected stacks are applied.

| Stack | Detection | Checklist Items |
|-------|-----------|-----------------|
| **React / Next.js** | `package.json` with `react` | RSC, Server Actions, React 19.2 hooks, PPR |
| **Vue / Nuxt** | `package.json` with `vue` | Composition API, Pinia, defineModel, auto-imports |
| **Svelte / SvelteKit** | `package.json` with `svelte` | Runes ($state, $derived, $effect), form actions |
| **Angular** | `package.json` with `@angular` | Signals, standalone components, zoneless, control flow |
| **Astro** | `astro.config.*` | Astro-specific patterns |
| **Remix** | `package.json` with `@remix-run` | Remix-specific patterns |
| **SolidJS** | `package.json` with `solid-js` | Solid-specific patterns |
| **Python** | `requirements.txt` / `pyproject.toml` | Type hints, async, ORM, virtual environments |
| **Go** | `go.mod` | Error handling, goroutines, interfaces, go vet |
| **Rust** | `Cargo.toml` | Ownership, Result/Option, unsafe audit, clippy |
| **PHP / Laravel** | `composer.json` | Eloquent, middleware, queues, PHP 8.3+ types |
| **Java / Kotlin** | `pom.xml` / `build.gradle` | JVM-specific patterns |
| **C# / .NET** | `*.csproj` / `*.sln` | .NET-specific patterns |
| **Flutter / Dart** | `pubspec.yaml` | Platform conventions, state management, accessibility |
| **Astro** | `astro.config.*` | Astro-specific patterns |
| **Remix** | `package.json` with `@remix-run` | Remix-specific patterns |
| **SolidJS** | `package.json` with `solid-js` | Solid-specific patterns |
| **Hono** | `package.json` with `hono` | Hono-specific patterns |
| **Vanilla HTML/JS/CSS** | Standalone `.html` files | Progressive enhancement, semantic HTML, Web Standards |

**Multi-stack projects:** If multiple config files are detected (e.g., monorepo with React frontend + Python API), all matching stack checklists are applied.

---

## Usage

### Command Reference

| Command | Description | Output |
|---------|-------------|--------|
| `/skill:brutal-honest Review my auth system` | Basic review | BRUTAL REVIEW only |
| `/skill:brutal-honest Review my API -fix` | Review with fixes | Review + FIX-PROMPT |
| `/skill:brutal-honest Review my app -features` | Review with ideas | Review + FEATURE-PROMPT |
| `/skill:brutal-honest Review my project -fix -features` | Complete analysis | Review &rarr; Fix &rarr; Features |

### Example Output

**Input:**
```bash
/skill:brutal-honest Review my auth system -fix
```

**Output:**
```markdown
## BRUTAL REVIEW: Authentication

### CRITICAL
- Hardcoded API key - config.js:12 (security disaster)

### MAJOR
- No rate limiting on login endpoint - api/auth.ts:45

### VERDICT
Security Swiss cheese. Fix CRITICAL before prod.

### FIX-PROMPT
"Fix auth system:
config.js:12 Move API_KEY to environment variables (.env or platform secrets manager)
api/auth.ts:45 Add rate limiting middleware
Rules: NO AI comments, test each change"
```

---

## Severity Levels

<table>
<tr>
<td width="15%" align="center"><h3>CRITICAL</h3></td>
<td>

**Fix yesterday.**
- AI vulnerabilities (prompt injection, auth tokens in logs)
- Hardcoded secrets, SQL injection, XSS, CSRF
- WCAG 2.2 violations, crashes, memory leaks
- Data loss: no backups, no transaction safety

</td>
</tr>
<tr>
<td align="center"><h3>MAJOR</h3></td>
<td>

**Ship blocker.**
- Outdated framework (2+ major versions behind current stable)
- Architecture: monoliths without structure, God objects, circular deps
- Client-side LLM calls exposing API keys
- Framework-specific rendering errors, missing error handling

</td>
</tr>
<tr>
<td align="center"><h3>MEDIUM</h3></td>
<td>

**Code review nightmare.**
- Not using framework's recommended patterns/idioms
- Missing tests, no CI/CD pipeline
- No caching strategy, missing lazy loading
- No monitoring or error tracking

</td>
</tr>
<tr>
<td align="center"><h3>MINOR</h3></td>
<td>

**Nitpick, but fix it.**
- Inconsistent code style, mixed naming conventions
- Debug code in production, missing documentation
- Non-semantic markup

</td>
</tr>
</table>

---

## Installation

### Claude Code (Recommended)

```bash
git clone https://github.com/Gakuseei/brutal-honest-Skill.git
cp -r brutal-honest-Skill ~/.claude/skills/brutal-honest
```

### Project-Level (Teams)

```bash
cp -r brutal-honest-Skill ./.agents/skills/brutal-honest

git add .agents/skills/brutal-honest
git commit -m "chore: add brutal-honest skill for code reviews"
```

### Other AI Tools

| AI | Method |
|----|--------|
| **Claude Code** | Copy to `~/.claude/skills/brutal-honest/` |
| **Kimi CLI** | Copy to `~/.kimi/skills/brutal-honest/` |
| **GPT / Codex** | Paste `SKILL.md` as custom instructions |
| **Gemini** | Paste `SKILL.md` as gem or system instructions |
| **Cursor** | Copy to `.cursor/skills/` or project-level |
| **Any LLM** | Paste `SKILL.md` content as system context |

---

## Architecture

```
brutal-honest/
├── SKILL.md                 # Main definition + EMBEDDED references (fallback)
│   ├── Severity Guide       # Fallback if external not found
│   ├── Checklists          # Fallback if external not found
│   └── UI Patterns         # Fallback if external not found
├── references/             # OPTIONAL: Override embedded defaults
│   ├── severity-guide.md   # Custom severity definitions
│   ├── checklists.md       # Custom checklists
│   └── ui-patterns.md      # Custom UI patterns
├── README.md
└── LICENSE.md
```

### How It Works

1. **Try External** &mdash; Load `references/*.md` if available (customization)
2. **Fallback** &mdash; Use embedded references in SKILL.md (guaranteed to work)
3. **Merge** &mdash; External overrides embedded, gaps filled with defaults

**Result:** Works with any AI tool, offline, and fully customizable.

---

## What It Checks

### Universal (all projects)

| Category | Items |
|----------|-------|
| **Security** | Secrets, injection prevention, input validation, dependency audit, CSRF, auth, AI security |
| **Performance** | Platform-appropriate metrics, asset optimization, caching, lazy loading, query optimization |
| **Architecture** | Separation of concerns, dependency graph, DRY, error handling, config management, database patterns |
| **Testing** | Unit tests, integration tests, E2E, test isolation, coverage strategy, mocking boundaries |
| **CI/CD** | Automated builds, linting in CI, tests on PR, deployment strategy |
| **Accessibility** | WCAG 2.2, semantic markup, ARIA, color contrast, reduced motion, focus management |

### Stack-Specific (applied only when detected)

Applied on top of the universal checklist. Each stack gets 4-7 framework-specific items covering idiomatic patterns and latest stable features.

### UI Patterns

- Layout: Bento Grid, View Transitions API, Anchor Positioning, CSS 2026 features
- Visual: 60-30-10 color rule, typography scale, spacing system
- Interaction: Micro-interactions, loading states, focus states
- Mobile: Touch targets, safe areas, platform conventions, 60fps scrolling
- AI-Native: Chat interface architecture, streaming states, content markers
- Anti-patterns: Context-dependent (system fonts and infinite scroll are valid in certain contexts)

---

## Changelog

### 2.0.0 (2026-02-22)

**Breaking:** Complete rewrite from React-only to tech-stack agnostic.

- Rewrote entire skill to be tech-stack agnostic (was React/Next.js-only)
- Added Stack Detection (Step 0) with 16 supported stacks
- Added multi-stack project support for monorepos
- Split checklists into Universal + Stack-Specific sections
- Added stack-specific checklists: Vue, Svelte, Angular, Python, Go, Rust, PHP, Vanilla HTML, Mobile
- Added Testing and CI/CD universal checklists
- Added CSS 2026 features (if(), sibling-index(), Scroll State Queries, corner-shape, customizable select)
- Fixed "System Fonts" and "Infinite Scroll" anti-patterns to be context-dependent
- Replaced JSX/TSX examples with framework-agnostic HTML
- Removed vendor-specific paths
- Removed hardcoded version numbers — now checks "2+ major versions behind current stable"
- Added AI Model Compatibility section
- Added Mobile UI Patterns section
- Deleted duplicate subfolder
- Fixed file limit contradiction (unified to 30)

### 1.0.0 (2026-02-02)

- Initial release

---

## Contributing

Contributions welcome! Areas we'd love help with:

- Additional stack-specific checklists (Elixir, Ruby, Swift, Kotlin native)
- More UI patterns (desktop app patterns, game UI patterns)
- Additional AI assistant integration guides
- Documentation translations

**How to contribute:**
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'feat: add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## License

MIT License - see [LICENSE.md](LICENSE.md) for details.

---

## Author

**gakuseei**

[![GitHub](https://img.shields.io/badge/GitHub-@Gakuseei-181717?logo=github&style=for-the-badge)](https://github.com/Gakuseei)

---

<p align="center">
  <i>Be brutal. Fix yesterday. Ship blockers must die.</i><br>
  <b>Star this repo if it helps you write better code!</b>
</p>
