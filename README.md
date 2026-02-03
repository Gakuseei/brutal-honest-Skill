# 🔥 brutal-honest

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.0-red?style=for-the-badge" alt="Version">
  <img src="https://img.shields.io/badge/last_updated-02.02.2026-blue?style=for-the-badge" alt="Last Updated">
  <img src="https://img.shields.io/badge/license-MIT-green?style=for-the-badge" alt="License">
</p>

<p align="center">
  <b>Ruthless code/UI/architecture analysis. No sugarcoating.</b>
</p>

<p align="center">
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-usage">Usage</a> •
  <a href="#-severity-levels">Severity</a> •
  <a href="#-architecture">Architecture</a> •
  <a href="#-what-it-checks">Features</a>
</p>

---

## 🚀 Quick Start

```bash
# Kimi CLI
cp -r brutal-honest ~/.kimi/skills/

# Claude, GPT, Cursor, etc.
# Copy to your AI's skills directory

# Project-level (recommended for teams)
cp -r brutal-honest ./.agents/skills/
```

---

## 🎯 Usage

| Command | Output |
|---------|--------|
| `/skill:brutal-honest Review my auth system` | Basic review only |
| `/skill:brutal-honest Review my API -fix` | Review + **FIX-PROMPT** |
| `/skill:brutal-honest Review my app -features` | Review + **FEATURE-PROMPT** |
| `/skill:brutal-honest Review my project -fix -features` | Review → Fix → Features (3 blocks) |

### Example Output

**Input:**
```bash
/skill:brutal-honest Review my auth system -fix
```

**Output:**
```markdown
## BRUTAL REVIEW: Authentication

### 🔴 CRITICAL
- Hardcoded API key - app.js:12

### 🎯 VERDICT
Security Swiss cheese. Fix CRITICAL before prod.

### FIX-PROMPT
"Fix auth system:
app.js:12 Move API_KEY to .env → process.env.API_KEY
Rules: NO AI comments, test after change"
```

---

## 📊 Severity Levels

<table>
<tr>
<td width="20%" align="center"><h1>🔴</h1></td>
<td>

### CRITICAL — Fix yesterday
- AI vulnerabilities (prompt injection, auth tokens in logs)
- Hardcoded secrets, SQL injection, XSS
- WCAG 2.2 violations = legal risk
- Crashes, infinite loops, memory leaks

</td>
</tr>
<tr>
<td align="center"><h1>🟠</h1></td>
<td>

### MAJOR — Ship blocker
- React 18 in 2026, no Server Components
- 2000+ line monoliths, copy-paste code
- Client-side LLM calls (exposes API keys)
- Hydration errors, missing error handling

</td>
</tr>
<tr>
<td align="center"><h1>🟡</h1></td>
<td>

### MEDIUM — Code review nightmare
- Missing React 19 hooks (useActionState, etc.)
- Manual state instead of TanStack Query
- Prop drilling at 3+ levels
- No React DevTools profiling

</td>
</tr>
<tr>
<td align="center"><h1>🟢</h1></td>
<td>

### MINOR — Nitpick, but fix it
- Mixed `"` and `'` in same file
- `console.log` in production (especially AI logs)
- Missing JSDoc, inconsistent naming

</td>
</tr>
</table>

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────┐
│           brutal-honest/                │
├─────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────────┐  │
│  │  SKILL.md   │  │  README.md      │  │
│  │  (embedded  │  │  (this file)    │  │
│  │   refs)     │  └─────────────────┘  │
│  └─────────────┘  ┌─────────────────┐  │
│  ┌─────────────┐  │  LICENSE.md     │  │
│  │ references/ │  │  (MIT)          │  │
│  │  (optional  │  └─────────────────┘  │
│  │   overrides)│                       │
│  └─────────────┘                       │
└─────────────────────────────────────────┘
```

**Hybrid Design:**
1. 📁 **Try external** `references/*.md` files (for customization)
2. 💾 **Fallback** to embedded defaults (guaranteed to work)
3. 🔀 **Merge** external overrides with embedded base

> ✅ Works with **any AI** — Kimi CLI, Claude, GPT, Cursor, etc.
> ✅ Never fails due to missing files
> ✅ Fully customizable

---

## 🔍 What It Checks

<div align="center">

| Category | Checks |
|----------|--------|
| **🔐 Security** | Prompt injection, AI tool permissions, LLM sanitization, secrets, XSS, SQLi |
| **⚡ Performance** | Core Web Vitals, Partial Pre-rendering, bundle size, AVIF images |
| **🏛️ Architecture** | React 19 hooks, Server Actions, RSC, TypeScript strict, error boundaries |
| **🖥️ Backend** | App Router, rate limiting (AI endpoints), auth, edge compatibility |
| **♿ Accessibility** | WCAG 2.2, focus management, reduced motion, color contrast |
| **🎨 UI** | Bento grids, view transitions, AI chat interfaces, dark mode, micro-animations |

</div>

### 2026 Standards

- ✅ React 19+ (useActionState, useOptimistic, useFormStatus)
- ✅ Next.js 15+ (App Router, Server Actions, PPR)
- ✅ Node.js 20+
- ✅ AI-native UI patterns (streaming, chat interfaces)

---

## 🛠️ Tech Stack

![React](https://img.shields.io/badge/React-19+-61DAFB?logo=react&logoColor=black)
![Next.js](https://img.shields.io/badge/Next.js-15+-000000?logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-Strict-3178C6?logo=typescript)
![Node.js](https://img.shields.io/badge/Node.js-20+-339933?logo=node.js)

---

## 📜 Rules

1. **NO AI comments** in code
2. **Preserve existing features** when fixing
3. **Max 30 files** context (prioritized)
4. **AI security** = CRITICAL severity — never downplay prompt injection
5. Always attempt external references first, but **NEVER fail** if not found

---

## 👤 Author

**gakuseei**

[![GitHub](https://img.shields.io/badge/GitHub-@Gakuseei-181717?logo=github)](https://github.com/Gakuseei)
[![Repo](https://img.shields.io/badge/Repo-brutal--honest--Skill-blue)](https://github.com/Gakuseei/brutal-honest-Skill)

---

<p align="center">
  <i>Be brutal. Fix yesterday. Ship blockers must die.</i>
</p>
