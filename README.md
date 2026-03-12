<div align="center">

<br>

```
  _                _        _       _                          _
 | |__  _ __ _   _| |_ __ _| |     | |__   ___  _ __   ___  ___| |_
 | '_ \| '__| | | | __/ _` | |_____| '_ \ / _ \| '_ \ / _ \/ __| __|
 | |_) | |  | |_| | || (_| | |_____| | | | (_) | | | |  __/\__ \ |_
 |_.__/|_|   \__,_|\__\__,_|_|     |_| |_|\___/|_| |_|\___||___/\__|
```

**code review that doesn't lie to you.**

[![v3.2.0](https://img.shields.io/badge/v3.2.0-black?style=flat-square)](./CHANGELOG.md)
&nbsp;
[![33+ stacks](https://img.shields.io/badge/33+_stacks-black?style=flat-square)](./SKILL.md)
&nbsp;
[![any LLM](https://img.shields.io/badge/any_LLM-black?style=flat-square)](./SKILL.md)

<br>

</div>

---

### What it does

One command. Full review. Every finding has `file:line` proof or it doesn't exist.

```
/brutal-honest
```

The wizard handles the rest.

---

### The pipeline

```
Wizard → Scanner + Research → Questions → Parallel Review → Findings → Fix Cycle → Done
```

| Phase | What happens |
|-------|-------------|
| **Wizard** | Pick domains, scope, extras |
| **Scanner** | SubAgent reads every file, builds summaries |
| **Research** | SubAgent searches versions, CVEs, APIs, patterns |
| **Questions** | One at a time. Clear options. You decide what's intentional |
| **Review** | Parallel agents per domain. Evidence required |
| **Fix Cycle** | Implement → Spec Review → Quality Review → Commit |

---

### Severity

```
CRITICAL    security holes, crashes, data loss
MAJOR       ship blockers, broken architecture
MEDIUM      missing tests, wrong patterns
MINOR       style, debug code, naming
```

One verdict. No sugarcoating.

---

### 33+ stacks

| | |
|---|---|
| **Frontend** | React, Vue, Svelte, Angular, Astro, Remix, SolidJS |
| **Backend** | Go, Rust, Python, PHP/Laravel, Ruby/Rails, Elixir/Phoenix, Hono |
| **Native** | Swift, Kotlin, C/C++, C#/.NET, Flutter, Java/JVM, React Native |
| **Games** | Unity, Unreal, Godot, Bevy, Phaser, Three.js, PixiJS, Kaplay, Pygame, Love2D |
| **Vanilla** | HTML/JS/CSS, Web Canvas |

---

### Install

```bash
# Claude Code (recommended)
cp -r brutal-honest ~/.claude/skills/

# Any AI tool
cp -r brutal-honest ./.agents/skills/

# Raw — paste SKILL.md as system prompt
```

<details>
<summary>Windows</summary>

```powershell
Copy-Item -Recurse brutal-honest $env:USERPROFILE\.claude\skills\
```

</details>

---

### Works with

| Tool | Method |
|------|--------|
| **Claude Code** | Native skill |
| **GPT / Codex** | System prompt |
| **Gemini** | Gems |
| **Any LLM** | Paste `SKILL.md` |

---

### v3.2.0 — Lean Main Chat

Main Chat no longer reads source files. Two SubAgents handle the heavy lifting:

- **File Scanner** reads every file, returns structured summaries
- **Web Researcher** checks versions, CVEs, API docs, competitor patterns

Questions asked one at a time with clear descriptions. Fix cycle dispatches findings + paths only — agents read files themselves.

**~70-80% fewer tokens** in the main context window.

[Full changelog](./CHANGELOG.md)

---

<div align="center">
<sub>by <a href="https://gitlab.com/Gakuseeii">gakuseeii</a></sub>
</div>
