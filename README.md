# brutal-honest

[![Last Updated](https://img.shields.io/badge/last_updated-02.02.2026-blue)](./SKILL.md)

Ruthless code/UI/architecture analysis. No sugarcoating.

## Install

```bash
# Kimi CLI
cp -r brutal-honest ~/.kimi/skills/

# Claude, GPT, etc.
# Copy to your AI's skills directory

# Project-level
cp -r brutal-honest ./.agents/skills/
```

## Usage

```bash
# Basic review
/skill:brutal-honest Review my auth system

# With fix prompt (generates FIX-PROMPT section)
/skill:brutal-honest Review my API -fix

# With feature ideas (generates FEATURE-PROMPT section)  
/skill:brutal-honest Review my app -features

# Both (3 distinct sections: Review → Fix → Features)
/skill:brutal-honest Review my project -fix -features
```

## Severity Levels

| Level | Tone | Examples |
|-------|------|----------|
| 🔴 CRITICAL | Fix yesterday | Prompt injection, hardcoded secrets, crashes |
| 🟠 MAJOR | Ship blocker | React 18 in 2026, hydration errors, no error handling |
| 🟡 MEDIUM | Code review nightmare | Missing React 19 hooks, prop drilling, no DevTools |
| 🟢 MINOR | Nitpick, but fix it | Mixed quotes, missing JSDoc, console.log in prod |

## What It Checks

**Security:** Prompt injection, AI tool permissions, LLM output sanitization, secrets, XSS, SQLi, CVEs  
**Performance:** Core Web Vitals, Partial Pre-rendering, bundle size, AVIF images  
**Architecture:** React 19 hooks, Server Actions, RSC, TypeScript strict, error boundaries  
**Backend:** App Router, rate limiting (especially AI endpoints), auth, edge compatibility  
**Accessibility:** WCAG 2.2, focus management, reduced motion, color contrast  
**UI:** Bento grids, view transitions, AI chat interfaces, dark mode, micro-animations

## Example

```bash
/skill:brutal-honest Review my auth system -fix
```

Output:
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
2. If not found → use embedded defaults (guaranteed to work)
3. External files override embedded (your custom rules take precedence)

## Rules

- Max 30 files analyzed (prioritized)
- Checks package.json for framework versions
- React 19+, Next.js 15+, Node 20+ are 2026 standards
- AI security issues = CRITICAL severity
- Never fails due to missing reference files

## Author

gakuseei - https://github.com/Gakuseei
