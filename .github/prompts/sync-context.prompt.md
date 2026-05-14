---
description: Sync project context to .github/copilot-instructions.md by analyzing codebase patterns and conventions
name: "sync-context"
agent: "agent"
tools:
  - read
  - edit
  - search
---

# Sync Context Prompt

Analyzes codebase and intelligently updates `.github/copilot-instructions.md` with current project context.

## Usage

Invoke this prompt from the Copilot Chat prompt picker (type `/` in chat to browse available prompts).

| Option | Effect |
|--------|--------|
| (no argument) | Full analysis and update |
| `--section agents` | Update specific section only |
| `--dry-run` | Preview changes without saving |

---

## What It Does

1. **Analyzes** current codebase structure
2. **Extracts** patterns, conventions, and architecture
3. **Merges** with existing `.github/copilot-instructions.md` content
4. **Preserves** manual customizations
5. **Updates** sections that need refresh

---

## Analysis Process

### Step 1: Scan Codebase Structure

Search for files matching these patterns to build a complete picture of the project:

- `**/*.py` — Python files
- `**/*.ts` — TypeScript files
- `**/package.json` — Node projects
- `**/pyproject.toml` — Python projects
- `**/Dockerfile` — Container configs
- `**/*.tf` — Terraform

### Step 2: Extract Patterns

Search for these code patterns across the repository:

- `@dataclass` — Dataclass usage
- `class.*Parser` — Parser patterns
- `def test_` — Test patterns
- `async def` — Async patterns
- `from src` — Import structure
- `@router` — API patterns
- `BaseModel` — Pydantic patterns

### Step 3: Analyze Agents

Search for files matching `.github/agents/**/*.md` and categorize by folder:

- `ai-ml/` → AI/ML, prompts, GenAI architecture, LLM specialist
- `communication/` → Planning, meetings
- `exploration/` → Codebase explorer, KB architect

### Step 4: Merge Updates

```text
# Sections to update
- Project Structure (from scan)
- Coding Standards (from patterns)
- Agent Usage (from agent analysis)
- Commands (from .github/prompts/)
- Environment (from config files)

# Sections to preserve
- Project Context (manual)
- Core Mission (manual)
- Important Dates (manual)
- Getting Help (manual)
```

---

## copilot-instructions.md Template

Generated `.github/copilot-instructions.md` follows this structure:

```markdown
# {Project Name}

## Project Context

{Manual: What this project does and why}

---

## Architecture Overview

{Auto-generated from codebase scan}

```text
{Data flow diagram}
```

| Stage | Technology | Purpose |
| ----- | ---------- | ------- |
| {stage} | {tech} | {purpose} |

---

## Project Structure

{Auto-generated from file searches}

```text
{project}/
├── src/
│   ├── {folders discovered}
├── tests/
├── .github/
│   ├── agents/
│   ├── prompts/
│   ├── 
│   │   ├── kb/
│   │   └── storage/
```

---

## Agent Usage Guidelines

{Auto-generated from .github/agents/ folder}

### Available Agents by Category

| Category | Agents | Use When |
| -------- | ------ | -------- |
| AI/ML | ai-data-engineer, ai-prompt-specialist, genai-architect, llm-specialist | AI pipelines, prompts, architecture |
| Communication | meeting-analyst, the-planner | Planning, meetings |
| Exploration | codebase-explorer, kb-architect | Codebase analysis, KB management |

---

## Coding Standards

{Auto-generated from detected patterns}

### Language: {Python/TypeScript/etc}

- **Version:** {detected from config}
- **Style:** {detected patterns}
- **Testing:** {detected framework}

### Detected Patterns

| Pattern | Usage | Example File |
| ------- | ----- | ------------ |
| {pattern} | {where used} | {file path} |

---

## Prompts

{Auto-generated from .github/prompts/ folder}

| Prompt | Purpose |
| ------- | ------- |
| /sync-context | Sync project context |
| /memory | Save session insights |
| {prompt} | {purpose} |

---

## Environment Variables

{Auto-generated from .env.example, config files}

| Variable | Purpose |
| -------- | ------- |
| {var} | {purpose} |

---

## MCP Tools Available

{Auto-generated from settings}

| MCP Server | Purpose |
| ---------- | ------- |
| context7 | Library documentation |
| exa | Code context search |
| {mcp} | {purpose} |

---
```
