# brutal-honest

[![Last Updated](https://img.shields.io/badge/last_updated-2026--02--22-blue)](./SKILL.md) [![Version](https://img.shields.io/badge/version-2.3.0-green)](./SKILL.md)

Ruthless code/UI/architecture analysis. No sugarcoating. **Tech-stack agnostic.**

## Install

```bash
# Claude Code
cp -r brutal-honest ~/.claude/skills/

# Project-level (any AI tool)
cp -r brutal-honest ./.agents/skills/

# Or paste SKILL.md as system context in any LLM
```

## Usage

```bash
# Basic review
/skill:brutal-honest Review my auth system

# With fix prompt (generates FIX-PROMPT section)
/skill:brutal-honest Review my API -fix

# With feature ideas (generates FEATURE-PROMPT section)
/skill:brutal-honest Review my app -features

# Both (3 distinct sections: Review -> Fix -> Features)
/skill:brutal-honest Review my project -fix -features
```

## Supported Stacks

Works with any tech stack. Auto-detects and applies relevant checklists:

| Stack | Detection |
|-------|-----------|
| React / Next.js | `package.json` with `react` |
| Vue / Nuxt | `package.json` with `vue` |
| Svelte / SvelteKit | `package.json` with `svelte` |
| Angular | `package.json` with `@angular` |
| Python (Django, FastAPI) | `requirements.txt` / `pyproject.toml` |
| Go | `go.mod` |
| Rust | `Cargo.toml` |
| PHP / Laravel | `composer.json` |
| Java / Kotlin | `pom.xml` / `build.gradle` |
| C# / .NET | `*.csproj` / `*.sln` |
| Flutter / Dart | `pubspec.yaml` |
| Astro | `astro.config.*` |
| Remix | `package.json` with `@remix-run` |
| SolidJS | `package.json` with `solid-js` |
| Hono | `package.json` with `hono` |
| Ruby / Rails | `Gemfile` with `rails` |
| Elixir / Phoenix | `mix.exs` |
| C / C++ | `CMakeLists.txt` / `*.cpp` + `Makefile` |
| Swift / iOS | `Package.swift` / `*.xcodeproj` |
| Kotlin | `build.gradle.kts` with `kotlin` |
| Vanilla HTML/JS/CSS | Standalone `.html` files |
| Unity | `Assets/` + `ProjectSettings/` + `*.unity` |
| Unreal Engine | `*.uproject` |
| Godot | `project.godot` |
| Phaser / Web 2D | `package.json` with `phaser` |
| Three.js / Web 3D | `package.json` with `three` |
| PixiJS | `package.json` with `pixi.js` |
| Kaplay | `package.json` with `kaplay` |
| Bevy (Rust ECS) | `Cargo.toml` with `bevy` |
| Love2D | `conf.lua` + `main.lua` |
| Pygame | `*.py` with `import pygame` |
| Web Canvas Game | `*.html` with `<canvas>` + game loop |

## Severity Levels

| Level | Tone | Examples |
|-------|------|----------|
| CRITICAL | Fix yesterday | Prompt injection, hardcoded secrets, crashes |
| MAJOR | Ship blocker | Outdated framework (2+ versions behind), no error handling |
| MEDIUM | Code review nightmare | Missing tests, no CI/CD, not using framework idioms |
| MINOR | Nitpick, but fix it | Mixed code style, debug code in prod, missing docs |

## What It Checks

**Universal (all projects):** Security, input validation, dependency audit, error handling, architecture, testing, CI/CD, accessibility
**Web:** Core Web Vitals, asset optimization, semantic HTML, WCAG 2.2, View Transitions API
**Stack-specific:** Framework idioms, latest stable features, recommended patterns (only applied when detected)
**AI apps:** Prompt injection, LLM output sanitization, AI tool permissions, streaming UI patterns
**Game dev:** Frame budget (60fps/16.6ms), object pooling, game loop separation, save integrity, game accessibility (colorblind, remapping, subtitles), HUD/menu patterns

## Example

```bash
/skill:brutal-honest Review my auth system -fix
```

Output:
```markdown
## BRUTAL REVIEW: Authentication

### CRITICAL
- Hardcoded API key - config.js:12

### VERDICT
Security Swiss cheese. Fix CRITICAL before prod.

### FIX-PROMPT
"Fix auth system:
app.js:12 Move API_KEY to .env
Rules: NO AI comments, test after change"
```

## Architecture

**Hybrid Design:** Works with any AI, customizable, never breaks

```
brutal-honest/
├── SKILL.md                 # Main definition + EMBEDDED references
│   ├── Severity Guide       # Fallback if external not found
│   ├── Checklists          # Fallback if external not found
│   └── UI Patterns         # Fallback if external not found
├── references/             # OPTIONAL: Override embedded
│   ├── severity-guide.md   # Custom severity definitions
│   ├── checklists.md       # Custom checklists
│   └── ui-patterns.md      # Custom UI patterns
└── README.md
```

**How it works:**
1. Try loading external `references/*.md` files (for customization)
2. If not found -> use embedded defaults (guaranteed to work)
3. External files override embedded (your custom rules take precedence)

## AI Model Compatibility

Works with any LLM that supports skill/prompt loading:
- **Claude** (Opus, Sonnet, Haiku) via Claude Code skills
- **GPT** (4.x, 5.x, Codex) via custom instructions or system prompts
- **Gemini** (2.x, 3.x Pro) via gems or system instructions
- **Kimi** (K2.5+) via Kimi CLI skills
- **DeepSeek, Grok, or any model** — paste SKILL.md as system context

## Rules

- Max 30 files analyzed (prioritized by importance)
- Auto-detects tech stack from config files
- Flags frameworks 2+ major versions behind current stable
- AI security issues = CRITICAL severity
- Never fails due to missing reference files

## Author

gakuseei - https://github.com/Gakuseei
