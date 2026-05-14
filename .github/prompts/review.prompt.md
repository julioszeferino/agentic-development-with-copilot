---
description: |
  Dual AI code review combining CodeRabbit CLI (static analysis, security)
  with Copilot deep analysis (architecture, logic, GenAI patterns) for maximum coverage.
name: "review"
agent: "agent"
tools:
  - read
  - execute
  - search
---

# Review Prompt

> Dual AI code review with CodeRabbit + deep analysis for maximum coverage

## Usage

Invoke this prompt from the Copilot Chat prompt picker (type `/` in chat to browse available prompts).

| Option | Effect |
|--------|--------|
| (no argument) | Review all changes vs main |
| `uncommitted` | Review only uncommitted changes |
| `committed` | Review only committed changes |
| `--base develop` | Compare to specific branch |
| `--quick` | CodeRabbit only (faster) |
| `--deep` | Deep analysis only (no CodeRabbit) |

---

## Overview

This prompt orchestrates a **dual AI review** combining:

| Reviewer | Strengths |
|----------|-----------|
| **CodeRabbit** | Static analysis, security scanning (Gitleaks, Semgrep), linting (Ruff, Pylint), pattern detection |
| **Deep Analysis** | Architectural review, business logic, GenAI patterns, contextual understanding |

```text
┌─────────────────────────────────────────────────────────────────┐
│                    DUAL AI REVIEW PIPELINE                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   ┌───────────────┐          ┌───────────────┐                  │
│   │  CodeRabbit   │          │     Deep      │                  │
│   │     CLI       │          │   Analysis    │                  │
│   └───────┬───────┘          └───────┬───────┘                  │
│           │                          │                           │
│   ┌───────▼───────┐          ┌───────▼───────┐                  │
│   │ • Security    │          │ • Architecture│                  │
│   │ • Linting     │          │ • Logic       │                  │
│   │ • Patterns    │          │ • GenAI       │                  │
│   │ • Style       │          │ • Intent      │                  │
│   └───────┬───────┘          └───────┬───────┘                  │
│           │                          │                           │
│           └────────────┬─────────────┘                          │
│                        │                                         │
│                ┌───────▼───────┐                                 │
│                │   UNIFIED     │                                 │
│                │   REPORT      │                                 │
│                └───────────────┘                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Process

### Step 1: Determine Scope

Run `git status`, `git diff --stat HEAD`, and `git log origin/main..HEAD --oneline` to check the current state.

**Scope Selection:**

| User Input | Scope |
|------------|-------|
| (no argument) | All changes vs main |
| `uncommitted` | Working directory only |
| `committed` | Committed changes only |
| `--base <branch>` | Compare to specific branch |

### Step 2: Run CodeRabbit Analysis

Run the following in a terminal to check CodeRabbit availability and execute the review:

```text
# Source shell to ensure CLI is available
source ~/.zshrc

# Check CLI availability
coderabbit --version || echo "CodeRabbit CLI not available"

# Run review based on mode:
# All changes:       coderabbit review --plain
# Uncommitted only:  coderabbit review --type uncommitted --plain
# Committed only:    coderabbit review --type committed --plain
# Specific branch:   coderabbit review --base <branch> --plain
```

**Note:** Requires CodeRabbit GitHub App installed on the repository.

**Parse CodeRabbit Output:**

```text
SEVERITY MAPPING
├─ [CRITICAL] → Must fix before merge
├─ [ERROR]    → Should fix before merge
├─ [WARNING]  → Recommended to fix
└─ [INFO]     → Nice to have
```

### Step 3: Run Deep Analysis

Use all available capabilities for deep code analysis.

**Focus Areas:**

| Category | Check For |
|----------|-----------|
| **Architecture** | Project pattern alignment, separation of concerns |
| **Business Logic** | Correct implementation, edge cases, error handling |
| **GenAI Patterns** | LangFuse hooks, structured outputs, prompt engineering |
| **Maintainability** | Self-documenting code, type hints, DRY principle |

### Step 4: Synthesize Findings

Combine results from both reviewers:

1. **Deduplicate** — Same issue found by both → keep one, note "Both"
2. **Prioritize** — Critical > Error > Warning > Info
3. **Categorize** — Security, Quality, Performance, Style
4. **Action** — Must fix vs Should fix vs Nice to have

### Step 5: Generate Report

---

## Output Format

```markdown
## 🔍 Dual AI Review Report

**Reviewers:** CodeRabbit + Deep Analysis
**Scope:** {scope_description}
**Files:** {count} files, {lines} lines changed
**Date:** {timestamp}

---

### 📊 Summary

| Source | 🔴 Critical | 🟠 Error | 🟡 Warning | 🔵 Info |
|--------|-------------|----------|------------|---------|
| CodeRabbit | {n} | {n} | {n} | {n} |
| Deep Analysis | {n} | {n} | {n} | {n} |
| **Total** | {n} | {n} | {n} | {n} |

---

### 🔴 Critical Issues

> Must fix before merge

#### [C1] {Title}
- **Source:** {CodeRabbit|Deep Analysis|Both}
- **File:** `{path}:{line}`
- **Issue:** {description}
- **Fix:**
```{lang}
{code}
```

---

### 🟠 Errors

> Should fix before merge

#### [E1] {Title}
- **Source:** {source}
- **File:** `{path}:{line}`
- **Issue:** {description}

---

### 🟡 Warnings

> Recommended to fix

- [{source}] `{file}`: {description}

---

### 🔵 Suggestions

- {suggestion 1}
- {suggestion 2}

---

### ✅ Positive Observations

- {good practice 1}
- {good practice 2}

---

### 📋 Action Checklist

- [ ] Fix: {critical 1}
- [ ] Fix: {critical 2}
- [ ] Consider: {warning 1}

---

**Merge Status:** {✅ Ready | ⚠️ Fix warnings first | 🚫 Fix critical issues}
```

---

## Error Handling

### CodeRabbit CLI Not Available

```text
IF coderabbit command not found:
  1. Attempt: source ~/.zshrc && coderabbit --version
  2. If still fails: Proceed with deep analysis only
  3. Note in report: "CodeRabbit unavailable"
```

### CodeRabbit Not Authenticated

```text
IF authentication error:
  1. Inform user: "Run 'coderabbit auth login' to authenticate"
  2. Proceed with deep analysis only
  3. Note in report: "CodeRabbit not authenticated"
```

### Large Changeset

```text
IF > 50 files changed:
  1. Suggest: "Large changeset detected. Use 'uncommitted' for faster feedback"
  2. Proceed with review but note potential timeout
```

---

## Quick Mode (`--quick`)

CodeRabbit only — for fast feedback.

**Process:**
1. Run `coderabbit review --plain`
2. Parse and format results
3. Skip deep analysis
4. Return immediately

**Use When:**
- Quick sanity check
- Pre-commit validation
- CI/CD integration

---

## Deep Mode (`--deep`)

Full deep analysis only — for thorough review without CodeRabbit.

**Process:**
1. Skip CodeRabbit
2. Full analysis with all capabilities
3. Detailed architectural review
4. Extended recommendations

**Use When:**
- CodeRabbit unavailable
- Need deeper contextual analysis
- Reviewing design decisions

---

## Integration

### In Development Loop

```bash
# Quick feedback on work in progress
/review uncommitted

# Full review before commit
git add .
/review committed
```

---

## Configuration

Respects `.coderabbit.yaml` settings:

| Setting | Effect |
|---------|--------|
| `reviews.path_instructions` | Custom rules per path |
| `reviews.tools` | Which linters are enabled |
