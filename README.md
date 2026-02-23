<div align="center">

# brutal-honest

**AI skill for ruthless code review. No sugarcoating. Any tech stack.**

[![Version](https://img.shields.io/badge/v2.4.0-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)
[![Stacks](https://img.shields.io/badge/31+_stacks-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)
[![AI Agnostic](https://img.shields.io/badge/any_LLM-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)

<br>

Senior-level analysis that catches what you missed.<br>
Security holes. Architectural debt. Framework misuse. Dead patterns.<br>
Categorized by severity. Zero fluff.

</div>

<br>

## Quick Start

```bash
# Claude Code (macOS/Linux)
cp -r brutal-honest ~/.claude/skills/

# Claude Code (Windows — PowerShell)
Copy-Item -Recurse brutal-honest $env:USERPROFILE\.claude\skills\

# Any AI tool — project-level
cp -r brutal-honest ./.agents/skills/

# Any LLM — paste SKILL.md as system context
```

## Usage

```bash
/brutal-honest Review my auth module           # Review with severity breakdown
/brutal-honest Review src/ -fix                # Review + generate fix prompt
/brutal-honest Review my app -features         # Review + feature ideas
/brutal-honest -check                          # Verify implementation against git history
/brutal-honest -check 3 -fix                   # Verify last 3 commits + fix prompt
```

## Output

Every review follows the same structure:

```
BRUTAL REVIEW: [Topic]

CRITICAL    — security holes, crashes, data loss
MAJOR       — ship blockers, missing error handling
MEDIUM      — missing tests, wrong patterns
MINOR       — style issues, debug code

VERDICT     — one brutal sentence
```

Add `-fix` for a copy-paste-ready fix prompt. Add `-features` for feature ideas.

## What It Catches

<table>
<tr>
<td width="50%" valign="top">

**Security & Stability**
- Hardcoded secrets, injection vectors
- Prompt injection in AI apps
- Unhandled errors, memory leaks
- Save corruption, soft-locks (games)

**Architecture**
- God objects, circular dependencies
- Copy-paste code, premature abstraction
- Wrong framework patterns

</td>
<td width="50%" valign="top">

**Performance**
- Core Web Vitals violations (web)
- Frame budget breaches (games)
- Missing caching, no lazy loading
- GC pressure in hot paths

**Quality**
- Outdated frameworks (2+ versions behind)
- Missing tests & CI/CD
- WCAG 2.2 accessibility gaps
- AI-generated aesthetic (anti-slop)

</td>
</tr>
</table>

## Stack Detection

Auto-detects your stack from config files. Only applies relevant rules.

| Category | Stacks |
|----------|--------|
| **Frontend** | React, Vue, Svelte, Angular, Astro, Remix, SolidJS |
| **Backend** | Go, Rust, Python, PHP/Laravel, Ruby/Rails, Elixir/Phoenix, Hono |
| **Native** | Swift/iOS, Kotlin, C/C++, C#/.NET, Flutter/Dart, Java |
| **Game Engines** | Unity, Unreal, Godot, Bevy, Phaser, Three.js, PixiJS, Pygame, Love2D |
| **Vanilla** | HTML/JS/CSS, Web Canvas Games |

Multi-stack monorepos? Each sub-project gets its own checklist.

## Verification Mode

`-check` reads your git history and verifies implementation against commit messages.
Requires a git repository with commit history — won't work outside of git repos.

```bash
/brutal-honest -check        # Auto-detect phase commits (Phase 1:, Step 2:, etc.)
/brutal-honest -check 5      # Verify last 5 commits as phases
```

**4-step pipeline:** Existence (is the code there?) → Integration (wired into all sync points?) → Regression (anything broken?) → Runtime (does it compile?)

## Customization

```
brutal-honest/
  SKILL.md                  # Core skill (with embedded fallbacks)
  references/
    severity-guide.md       # Override severity definitions
    checklists.md           # Override checklists
    ui-patterns.md          # Override UI patterns
```

Edit `references/` files to add your own rules. Delete one? The embedded fallback kicks in. Never breaks.

## Works With Any LLM

| AI | How |
|----|-----|
| **Claude** | Claude Code skills (native) |
| **GPT / Codex** | Custom instructions or system prompt |
| **Gemini** | Gems or system instructions |
| **Kimi** | Kimi CLI skills |
| **Others** | Paste SKILL.md as system context |

---

<div align="center">
<sub>by <a href="https://github.com/Gakuseei">gakuseei</a></sub>
</div>
