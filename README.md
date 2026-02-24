<div align="center">

# brutal-honest

**Ruthless code review. No sugarcoating. Any tech stack.**

[![Version](https://img.shields.io/badge/v2.7.3-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)
[![Stacks](https://img.shields.io/badge/33_stacks-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)
[![AI Agnostic](https://img.shields.io/badge/any_LLM-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)

<br>

![brutal-honest demo](https://github.com/Gakuseei/brutal-honest-Skill/raw/main/promo/out/brutal-honest-promo-readme.mp4)

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

## Commands

```bash
# Review
/brutal-honest Review src/                      # Review with severity breakdown
/brutal-honest Review my auth module            # Review specific module

# Review + Fix
/brutal-honest Review src/ -fix                 # Review + generate fix prompt
/brutal-honest Review my app -fix -features     # Review + fix prompt + feature ideas

# Features only
/brutal-honest Review my app -features          # Review + feature suggestions

# Verify implementation
/brutal-honest -check                           # Auto-detect phase commits, verify against git history
/brutal-honest -check 3                         # Verify last 3 commits as phases
/brutal-honest -check -fix                      # Verify + generate fix prompt for failed items
```

## How It Works

**Review** — Auto-detects your stack, applies framework-specific checklists, categorizes every finding by severity.

```
CRITICAL    — security holes, crashes, data loss
MAJOR       — ship blockers, missing error handling
MEDIUM      — missing tests, wrong patterns
MINOR       — style issues, debug code

VERDICT     — one brutal sentence
```

**Fix** (`-fix`) — Generates a copy-paste fix prompt, phased by severity. CRITICAL first, then MAJOR, then the rest.

**Features** (`-features`) — Concrete feature ideas with priority (High/Medium/Low).

**Verify** (`-check`) — 4-step skeptical pipeline that reads your git history and proves fixes actually work: Existence + Correctness → Cross-Reference → Regression → Devil's Advocate.

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
<sub>by <a href="https://github.com/Gakuseei">gakuseei</a></sub>
</div>
