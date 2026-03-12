# brutal-honest v3.0 — Design Document

**Date:** 2026-03-12
**Status:** Approved
**Breaking Changes:** Yes — removes `-fix`, `-feature`, `-check` flags, replaces with interactive wizard

## Problem Statement

The current brutal-honest skill (v2.7.5) has three structural weaknesses:

1. **No research enforcement** — The skill says "Analyze" and "Check" but never instructs Claude to actually Read files or use Grep. Claude assumes it already knows everything and hallucinates findings.
2. **Single-pass without SubAgents** — One agent tries to review Security, Architecture, Performance, UI/UX, and Stack-specific patterns simultaneously. This overloads context and leads to guessing.
3. **No evidence requirement** — No rule prevents Claude from inventing findings without ever reading a file. No file:line references required.

## Design Overview

Replace the flag-based CLI interface (`-fix`, `-feature`, `-check`) with an interactive wizard using `AskUserQuestion`, add parallel SubAgent reviews with mandatory evidence rules, and implement a subagent-driven-development fix cycle.

```
/brutal-honest → Wizard → Research → Informed Questions → SubAgent Review → Output → Action Wizard → Fix Cycle → Summary
```

## Phase 1: Initial Wizard (AskUserQuestion)

All user interaction MUST use `AskUserQuestion` (interactive terminal UI with selectable options). Never ask questions as plain chat text.

### Step 1 — "What to review?" (multiSelect)

| Option | Description |
|--------|-------------|
| Security | OWASP, secrets, auth, input validation, AI security |
| Architecture & Code Quality | File structure, dependencies, DRY, error handling, types, testing |
| Performance | Core Web Vitals / frame budget, assets, caching, lazy loading, N+1 |
| UI/UX & Accessibility | WCAG, mobile, visual hierarchy, anti-AI-slop patterns |

"Other" option always available for custom focus areas.

### Step 2 — "Review Settings"

Two questions:
- **Scope:** git diff only / Entire project / Specific files or folders
- **Stack Override:** Auto-detect (Recommended) / Manual selection

Research is ALWAYS automatic — the skill decides when it needs web search (framework versions, CVEs, best practices). No user input needed for this.

### Step 3 — "What after the review?" (multiSelect)

| Option | Description |
|--------|-------------|
| Fix Prompt | Generate fix instructions grouped by severity |
| Feature Ideas | Spawn a Feature Scout SubAgent that explores the whole project and suggests 3-5 concrete, implementable features |
| Verify against Commits | Run check pipeline against git history |
| Review only | No extras, just the review |

## Phase 2: Research + Discovery

Runs automatically after the wizard:

1. **Stack Detection** — Check config files (package.json, go.mod, pyproject.toml, etc.)
2. **File Collection** — Gather files based on scope selection. Max 30 files, prioritized: Configs → Entry Points → Core Logic
3. **Framework Version Check** — Compare detected version against current stable
4. **Automatic Web Research** — When uncertain about framework versions, known CVEs, or best practices: spawn a Web Search SubAgent. Results are passed to all review agents as context.
5. **Codebase Understanding** — Read files, understand patterns, structure, identify potential issues

## Phase 3: Informed Follow-Up Questions (AskUserQuestion)

After reading the actual code, ask targeted questions based on real findings. Each question uses `AskUserQuestion` with recommendations.

### When to ask:

- **Intentional decisions** — "Smart Suggestions is disabled via early return at line 142. Intentional?" → Yes, testing something (Recommended) / No, forgot to re-enable
- **Unusual patterns** — "Found `dangerouslySkipPermissions: true` in config. On purpose?" → Yes, deliberate / No, should be removed
- **Disabled features** — "Early return in ChatInterface.jsx:87 skips the entire suggestion flow. Intended?" → Yes, temporary / No, bug
- **Framework versions** — "React 17 detected, current stable is 19. Flag as MAJOR?" → Yes, upgrade important / No, staying on 17 deliberately
- **Suspicious code** — "Found 3 API keys in .env.example. Deep security check?" → Yes, full audit / No, they're just examples

### Core rule:

If something looks suspicious but COULD be a deliberate decision → **ASK, don't flag**. One question too many is better than one false finding.

## Phase 4: Parallel SubAgent Reviews

Spawn one SubAgent per selected review domain. Only the domains the user selected in Step 1 are spawned.

| Agent | Receives | Checklist Source |
|-------|----------|-----------------|
| Security Agent | Files + Research results + Security checklist | references/checklists.md (Security section) |
| Architecture Agent | Files + Research results + Architecture checklist | references/checklists.md (Architecture + Testing sections) |
| Performance Agent | Files + Research results + Performance checklist | references/checklists.md (Performance section) |
| UI/UX Agent | Files + Research results + UI patterns | references/ui-patterns.md + references/checklists.md (Accessibility section) |

If user selected "All" → spawn all 4 in parallel.

Stack-specific checklist items are distributed to the most relevant agent (e.g., React hooks → Architecture Agent, Core Web Vitals → Performance Agent).

### Iron Rules (hardcoded, every agent, non-negotiable)

```
1. Read EVERY file before judging — no exceptions
2. Include file:line for EVERY finding — no evidence = no finding
3. Use Grep to verify patterns before claiming they're missing
4. If uncertain → mark as "UNVERIFIED" and explain what couldn't be confirmed
5. NEVER invent findings — hallucinated findings are worse than no findings
6. If something looks intentional but you're unsure → flag as "NEEDS CONFIRMATION" not as a bug
```

### Agent Prompt Template

Each agent receives:
- The relevant checklist section (pasted, not a file reference)
- The files to review (content or paths)
- Research results (framework versions, CVE findings)
- User's answers from Phase 3 (so agents know what's intentional)
- The Iron Rules above
- Stack-specific checklist items relevant to their domain

## Phase 5: Aggregation + Output

1. Collect all agent findings
2. Deduplicate (same issue flagged by multiple agents)
3. Sort by severity: CRITICAL → MAJOR → MEDIUM → MINOR
4. Format as BRUTAL REVIEW:

```
## BRUTAL REVIEW: [Topic]

### CRITICAL
- [Finding with file:line evidence]

### MAJOR
- [Finding with file:line evidence]

### MEDIUM
- [Finding]

### MINOR
- [Finding]

### VERDICT
One brutal sentence.
```

## Phase 6: Action Wizard (AskUserQuestion)

After presenting the review, ask via `AskUserQuestion`:

**"How do you want to fix these?"** (single select)

| Option | Description |
|--------|-------------|
| SubAgents — phase by phase (Recommended) | Cheapest. Implement → Spec Review → Code Quality Review → Commit per phase |
| Agent Teams — parallel | More powerful but 3-5x more expensive |
| Fix it yourself in chat | AI assists but you drive |
| Discuss / Custom | Talk through the findings first |

## Phase 7: Fix Cycle (subagent-driven-development Pattern)

When SubAgents selected, findings are grouped into phases by severity:

- **Phase 1:** CRITICAL fixes
- **Phase 2:** MAJOR fixes
- **Phase 3:** MEDIUM + MINOR fixes

### Per Phase:

```
Step 1: Spawn Implementer SubAgent
  → Receives: Findings for this phase + relevant files
  → Implements fixes, writes tests, commits, self-reviews
  → Reports back: what was fixed, what was tested, files changed

Step 2: Spawn Spec Review SubAgent
  → Receives: Original findings + Implementer's report
  → Does NOT trust the implementer's claims
  → Reads actual code to verify each finding is truly fixed
  → ✅ All fixed → proceed to Step 3
  → ❌ Issues found → Implementer fixes → Spec Review re-runs

Step 3: Spawn Code Quality Review SubAgent
  → Receives: git diff since before the fix
  → Checks: Is the fix clean? No new bugs? No regressions?
  → ✅ Approved → Commit + Push → Next Phase
  → ❌ Issues found → Implementer fixes → Code Quality re-reviews

Step 4: Commit + Push → Next Phase
```

### Feature Scout (if selected in Step 3)

Runs as a separate SubAgent (parallel to reviews or after):
- Explores the entire project
- Analyzes architecture, patterns, existing features
- Suggests 3-5 concrete, implementable features
- Each suggestion includes: what it does, why it's valuable, rough complexity

## Phase 8: Final Summary

Human-readable summary of everything that was done. No line numbers, no technical jargon overload:

> "The authentication system had 2 critical security vulnerabilities (hardcoded API key and missing input validation) and 3 performance issues. All fixed, tested, and committed."

### Final AskUserQuestion:

**"What now?"** (single select)

| Option | Description |
|--------|-------------|
| Start another review | Review different project or files |
| Re-review same files | Check if everything is clean now |
| Done | End the session |

## What's Preserved from v2.x

- Stack Detection table (33 stacks)
- All checklists (universal + stack-specific)
- Severity definitions (CRITICAL/MAJOR/MEDIUM/MINOR)
- UI Patterns reference
- References directory (customization layer)
- VERDICT format

## What's Removed

- `-fix` flag → replaced by Action Wizard
- `-feature` flag → replaced by Feature Scout option in Step 3
- `-check` flag → replaced by Verify option in Step 3
- Conditional output logic (6 branches) → replaced by wizard selections
- Single-pass analysis → replaced by parallel SubAgent reviews
- Assumption-based analysis → replaced by research-first + evidence rules

## What's New

- Interactive wizard via `AskUserQuestion` (all user interaction)
- Automatic research (web search when uncertain)
- Informed follow-up questions based on actual code reading
- Parallel SubAgent reviews with Iron Rules
- subagent-driven-development fix cycle (Implement → Spec Review → Code Quality → Commit)
- Feature Scout SubAgent
- Evidence requirement: no finding without file:line proof
- "Ask, don't assume" rule for suspicious-but-possibly-intentional code
- Human-readable final summary
