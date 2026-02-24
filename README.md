<div align="center">

# brutal-honest

**Ruthless code review. No sugarcoating. Any tech stack.**

[![Version](https://img.shields.io/badge/v2.7.3-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)
[![Stacks](https://img.shields.io/badge/33_stacks-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)
[![AI Agnostic](https://img.shields.io/badge/any_LLM-1a1a2e?style=flat-square&labelColor=1a1a2e&color=2d2d44)](./SKILL.md)

<br>

<video src="https://github.com/Gakuseei/brutal-honest-Skill/releases/download/promo/brutal-honest-promo.mp4" width="100%" autoplay loop muted playsinline></video>

</div>

## Install

```bash
# Claude Code
cp -r brutal-honest ~/.claude/skills/          # macOS/Linux
Copy-Item -Recurse brutal-honest $env:USERPROFILE\.claude\skills\  # Windows

# Any LLM — paste SKILL.md as system context
```

## Usage

```bash
/brutal-honest Review src/ -fix -features   # Review + fix prompt + feature ideas
/brutal-honest -check                       # Verify fixes against git history
```

Auto-detects your stack. 33 stacks supported — React, Vue, Go, Rust, Unity, Godot, and [more](./SKILL.md).

## How It Works

1. **Review** — Scans your code, categorizes every finding by severity (CRITICAL/MAJOR/MEDIUM/MINOR), writes a verdict
2. **Fix** (`-fix`) — Generates a copy-paste fix prompt, phased by severity
3. **Features** (`-features`) — Suggests concrete features with priority
4. **Verify** (`-check`) — 4-step skeptical pipeline that reads git history and proves fixes actually work

---

<div align="center">
<sub>by <a href="https://github.com/Gakuseei">gakuseei</a></sub>
</div>
