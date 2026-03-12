<div align="center">

# brutal-honest

**Ruthless code review with evidence. No guessing, no hallucinating, no ego.**

[![Version](https://img.shields.io/badge/v3.0.0-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)
[![Stacks](https://img.shields.io/badge/33_stacks-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)
[![AI Agnostic](https://img.shields.io/badge/any_LLM-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)

<br>

![brutal-honest demo](/uploads/60973b9ed1e4780af65e5491c396f358/brutal-honest-promo.mp4)

</div>

## Install

```bash
# Claude Code
cp -r brutal-honest ~/.claude/skills/          # macOS/Linux
Copy-Item -Recurse brutal-honest $env:USERPROFILE\.claude\skills\  # Windows

# Any AI tool — project-level
cp -r brutal-honest ./.agents/skills/

# Any LLM — paste SKILL.md as system context
```

## Usage

```bash
# Start a review — wizard guides you through options
/brutal-honest Review src/
/brutal-honest Review my auth module
/brutal-honest Review the whole project
```

No flags to memorize. The interactive wizard asks what to review, how to review, and what extras you want.

## How It Works

```
/brutal-honest → Wizard → Research → Follow-Up Questions → SubAgent Review → Output → Action → Fix Cycle → Summary
```

**1. Wizard** — Choose what to review (Security, Architecture, Performance, UI/UX), scope (diff/project/files), and extras (fix, features, verify).

**2. Research** — Auto-detects stack, reads all files, checks framework versions, searches for CVEs and best practices when uncertain.

**3. Follow-Up Questions** — Asks about suspicious patterns AFTER reading code. Disabled features? Unusual configs? The AI asks before assuming.

**4. Parallel SubAgent Reviews** — One agent per domain, all running in parallel. Every finding requires file:line evidence. No guessing.

**5. Output** — Findings sorted by severity with evidence:

```
CRITICAL    — security holes, crashes, data loss
MAJOR       — ship blockers, missing error handling
MEDIUM      — missing tests, wrong patterns
MINOR       — style issues, debug code

VERDICT     — one brutal sentence
```

**6. Fix Cycle** — Choose how to fix: SubAgents (recommended), Agent Teams, or in-chat. SubAgent mode runs Implement → Spec Review → Code Quality Review → Commit per severity phase.

## Supported Stacks

| Category | Stacks |
|----------|--------|
| **Frontend** | React, Vue, Svelte, Angular, Astro, Remix, SolidJS |
| **Backend** | Go, Rust, Python, PHP/Laravel, Ruby/Rails, Elixir/Phoenix, Hono |
| **Native** | Swift/iOS, Kotlin, C/C++, C#/.NET, Flutter/Dart, Java/JVM, React Native |
| **Game Engines** | Unity, Unreal, Godot, Bevy, Phaser, Three.js, PixiJS, Kaplay, Pygame, Love2D |
| **Vanilla** | HTML/JS/CSS, Web Canvas Games |

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
<sub>by <a href="https://gitlab.com/Gakuseeii">gakuseeii</a></sub>
</div>
