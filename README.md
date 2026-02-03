# 🔥 brutal-honest

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.0-red?style=for-the-badge&logo=semver&logoColor=white" alt="Version">
  <img src="https://img.shields.io/badge/last_updated-02.02.2026-blue?style=for-the-badge" alt="Last Updated">
  <img src="https://img.shields.io/badge/license-MIT-green?style=for-the-badge&logo=opensourceinitiative&logoColor=white" alt="License">
  <img src="https://img.shields.io/badge/status-stable-brightgreen?style=for-the-badge" alt="Status">
</p>

<p align="center">
  <b>Ruthless expert analysis for code, UI, and architecture.</b><br>
  <i>No sugarcoating. No compromises. Just brutal honesty.</i>
</p>

<p align="center">
  <a href="#-quick-start">🚀 Quick Start</a> •
  <a href="#-features">✨ Features</a> •
  <a href="#-usage">🎯 Usage</a> •
  <a href="#-severity-levels">📊 Severity</a> •
  <a href="#-installation">📦 Installation</a> •
  <a href="#-architecture">🏗️ Architecture</a>
</p>

---

## 🚀 Quick Start

```bash
# Clone the repository
git clone https://github.com/Gakuseei/brutal-honest-Skill.git

# Install for Kimi CLI
cp -r brutal-honest ~/.kimi/skills/

# Or install project-level (recommended for teams)
cp -r brutal-honest ./.agents/skills/
```

**Start analyzing:**
```bash
/skill:brutal-honest Review my codebase -fix
```

---

## ✨ Features

<table>
<tr>
<td width="50%">

### 🔍 Comprehensive Analysis
- **Security:** Prompt injection, XSS, SQLi, secrets detection
- **Performance:** Core Web Vitals, bundle size, image optimization
- **Architecture:** React 19, Next.js 15, Server Components
- **AI-Native:** LLM sanitization, tool permissions, rate limiting

</td>
<td width="50%">

### 🎯 Smart Output Modes
- **Review Only:** Quick analysis without suggestions
- **`-fix`:** Get actionable FIX-PROMPT with file:line references
- **`-features`:** Discover new feature opportunities
- **Both:** Complete analysis → fixes → features (3 blocks)

</td>
</tr>
<tr>
<td width="50%">

### 🏗️ Hybrid Architecture
- **Embedded:** All references built-in (works offline)
- **External:** Override with custom `references/*.md`
- **Merge:** External extends embedded (customize what you need)
- **Universal:** Works with any AI assistant

</td>
<td width="50%">

### ⚡ 2026 Standards
- React 19+ (useActionState, useOptimistic)
- Next.js 15+ (App Router, Server Actions, PPR)
- TypeScript Strict
- AI-Native UI patterns
- WCAG 2.2 Accessibility

</td>
</tr>
</table>

---

## 🎯 Usage

### Command Reference

| Command | Description | Output |
|---------|-------------|--------|
| `/skill:brutal-honest Review my auth system` | Basic review | BRUTAL REVIEW only |
| `/skill:brutal-honest Review my API -fix` | Review with fixes | Review + FIX-PROMPT |
| `/skill:brutal-honest Review my app -features` | Review with ideas | Review + FEATURE-PROMPT |
| `/skill:brutal-honest Review my project -fix -features` | Complete analysis | Review → Fix → Features |

### Example Output

**Input:**
```bash
/skill:brutal-honest Review my auth system -fix
```

**Output:**
```markdown
## BRUTAL REVIEW: Authentication

### 🔴 CRITICAL
- Hardcoded API key - app.js:12 (security disaster)

### 🟠 MAJOR
- No rate limiting on login endpoint - api/auth.ts:45

### 🎯 VERDICT
Security Swiss cheese. Fix CRITICAL before prod.

### FIX-PROMPT
"Fix auth system:
app.js:12 Move API_KEY to .env → process.env.API_KEY
api/auth.ts:45 Add rate limiting → use express-rate-limit
Rules: NO AI comments, test each change"
```

---

## 📊 Severity Levels

<table>
<tr>
<td width="15%" align="center"><h1>🔴</h1><br><b>CRITICAL</b></td>
<td>

**Fix yesterday.**
- AI vulnerabilities (prompt injection, auth tokens in logs)
- Hardcoded secrets, SQL injection, XSS
- WCAG 2.2 violations = legal risk
- Crashes, infinite loops, memory leaks

</td>
</tr>
<tr>
<td align="center"><h1>🟠</h1><br><b>MAJOR</b></td>
<td>

**Ship blocker.**
- React 18 in 2026, no Server Components
- Client-side LLM calls (exposes API keys)
- Hydration errors, missing error handling
- 2000+ line monoliths, copy-paste code

</td>
</tr>
<tr>
<td align="center"><h1>🟡</h1><br><b>MEDIUM</b></td>
<td>

**Code review nightmare.**
- Missing React 19 hooks (useActionState, etc.)
- Manual state instead of TanStack Query
- Prop drilling at 3+ levels
- No React DevTools profiling

</td>
</tr>
<tr>
<td align="center"><h1>🟢</h1><br><b>MINOR</b></td>
<td>

**Nitpick, but fix it.**
- Mixed `"` and `'` in same file
- `console.log` in production (especially AI logs)
- Missing JSDoc, inconsistent naming

</td>
</tr>
</table>

---

## 📦 Installation

### Kimi CLI (Recommended)

```bash
# Clone repository
git clone https://github.com/Gakuseei/brutal-honest-Skill.git
cd brutal-honest-Skill

# Install skill
cp -r brutal-honest ~/.kimi/skills/

# Verify installation
ls ~/.kimi/skills/brutal-honest/
```

### Project-Level (Teams)

```bash
# Add to your project
cp -r brutal-honest ./.agents/skills/

# Commit to repo
git add .agents/skills/brutal-honest
git commit -m "chore: add brutal-honest skill for code reviews"
```

### Other AI Assistants

| AI | Installation Path |
|----|-------------------|
| **Claude** | Check Claude Desktop settings for skills directory |
| **Cursor** | Add to `.cursor/skills/` or project-level |
| **GPT** | Import as custom GPT instructions |
| **Generic** | Copy to `./.agents/skills/brutal-honest/` |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    brutal-honest/                       │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌─────────────────┐    ┌───────────────────────────┐  │
│  │   SKILL.md      │    │      references/          │  │
│  │                 │    │      (optional)           │  │
│  │  ┌───────────┐  │    │                           │  │
│  │  │ EMBEDDED  │  │◄───┤  • severity-guide.md      │  │
│  │  │ REFERENCES│  │    │  • checklists.md          │  │
│  │  │ (fallback)│  │    │  • ui-patterns.md         │  │
│  │  └───────────┘  │    │                           │  │
│  │        ▲        │    │  OVERRIDE embedded        │  │
│  │        │        │    │  when present             │  │
│  └────────┼────────┘    └───────────────────────────┘  │
│           │                                             │
│           └──────► Works with any AI (no file deps)     │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### How It Works

1. **📁 Try External** → Load `references/*.md` if available (customization)
2. **💾 Fallback** → Use embedded references (guaranteed to work)
3. **🔀 Merge** → External overrides embedded, gaps filled with defaults

**Result:** Universal compatibility + Customization without breaking.

---

## 🛠️ What It Checks

<div align="center">

| Category | Checks | Standards |
|----------|--------|-----------|
| **🔐 Security** | Prompt injection, AI tool perms, LLM sanitization, XSS, SQLi | OWASP, CVE |
| **⚡ Performance** | Core Web Vitals, PPR, bundle size, AVIF images | Lighthouse 100 |
| **🏛️ Architecture** | React 19 hooks, Server Actions, RSC, TypeScript strict | Clean Code |
| **🖥️ Backend** | App Router, rate limiting, auth, edge compatibility | OAuth 2.1 |
| **♿ Accessibility** | WCAG 2.2, focus management, reduced motion, contrast | AAA |
| **🎨 UI** | Bento grids, view transitions, AI chat interfaces, dark mode | 2026 Design |

</div>

### 2026 Tech Stack

![React](https://img.shields.io/badge/React-19+-61DAFB?logo=react&logoColor=black&style=flat-square)
![Next.js](https://img.shields.io/badge/Next.js-15+-000000?logo=next.js&style=flat-square)
![TypeScript](https://img.shields.io/badge/TypeScript-Strict-3178C6?logo=typescript&style=flat-square)
![Node.js](https://img.shields.io/badge/Node.js-20+-339933?logo=node.js&style=flat-square)

---

## 📋 Checklists Overview

### Security (8 items)
- Prompt injection prevention, secrets management, input validation
- AI Tool Security, LLM Output Sanitization, Model Rate Limiting

### Performance (8 items)
- Core Web Vitals, PPR, instrumentation, asset optimization

### Architecture (9 items)
- React Server Components, React 19 Hooks, Server Actions
- Parallel Data Fetching, Error Boundaries

### Backend (5 items)
- App Router, rate limiting, auth, edge compatibility

### Accessibility (6 items)
- WCAG 2.2, reduced motion, color contrast, focus management

---

## 🎨 UI Patterns Included

- **Bento Grid** (Asymmetric Apple Style)
- **View Transitions API** (2026 Standard)
- **Anchor Positioning** (Popovers without JS)
- **60-30-10 Color Rule**
- **Typography Scale** (rem-based)
- **4px Spacing System**
- **AI Chat Interface** (Streaming, Actions)
- **Anti-Patterns** (What to destroy)

---

## 🤝 Contributing

Contributions welcome! Areas we'd love help with:

- Additional language support (Python, Go, Rust checklists)
- More UI patterns (mobile-specific, desktop-specific)
- Additional AI assistant integrations
- Documentation translations

**How to contribute:**
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'feat: add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📜 License

MIT License - see [LICENSE.md](LICENSE.md) for details.

```
Copyright (c) 2026 gakuseei

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```

---

## 👤 Author

**gakuseei**

[![GitHub](https://img.shields.io/badge/GitHub-@Gakuseei-181717?logo=github&style=for-the-badge)](https://github.com/Gakuseei)

---

<p align="center">
  <a href="https://github.com/Gakuseei/brutal-honest-Skill/releases/latest">
    <img src="https://img.shields.io/github/v/release/Gakuseei/brutal-honest-Skill?style=for-the-badge&color=red" alt="Latest Release">
  </a>
</p>

<p align="center">
  <i>Be brutal. Fix yesterday. Ship blockers must die.</i><br>
  <b>⭐ Star this repo if it helps you write better code!</b>
</p>
