---
description: Generate comprehensive, production-ready README.md by analyzing codebase with explorer + documenter agents
name: "readme-maker"
agent: "agent"
tools:
  - read
  - edit
  - search
  - agent
---

# README Maker Prompt

Generates a professional README.md by combining codebase exploration with documentation best practices.

## Usage

Invoke this prompt from the Copilot Chat prompt picker (type `/` in chat to browse available prompts).

| Option | Effect |
|--------|--------|
| (no argument) | Full analysis → README.md |
| `--output docs/` | Output to specific directory |
| `--style minimal` | Minimal README (Quick Start focus) |
| `--style comprehensive` | Full documentation (default) |
| `--dry-run` | Preview without saving |

---

## What It Does

1. **Explores** codebase using codebase-explorer patterns
2. **Analyzes** project structure, tech stack, and patterns
3. **Generates** README.md following documentation best practices
4. **Validates** all examples and links before saving

---

## Execution Flow

```text
┌─────────────────────────────────────────────────────────────┐
│  README-MAKER WORKFLOW                                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Phase 1: EXPLORE (codebase-explorer patterns)              │
│  ├─ Scan root structure (ls, package files, configs)        │
│  ├─ Map source code (search by language patterns)           │
│  ├─ Identify tech stack and frameworks                      │
│  ├─ Count files, tests, documentation                       │
│  └─ Calculate health score                                  │
│                                                             │
│  Phase 2: EXTRACT (project metadata)                        │
│  ├─ Read package.json / pyproject.toml / Cargo.toml         │
│  ├─ Parse existing README (if present)                      │
│  ├─ Detect entry points (main, index, handler)              │
│  ├─ Find installation/setup commands                        │
│  └─ Identify environment variables                          │
│                                                             │
│  Phase 3: GENERATE (documentation patterns)                  │
│  ├─ Create compelling project description                   │
│  ├─ Build Quick Start with tested commands                  │
│  ├─ Document features with examples                         │
│  ├─ Add architecture overview (if complex)                  │
│  └─ Include contributing guidelines                         │
│                                                             │
│  Phase 4: VALIDATE (quality checks)                         │
│  ├─ Test all installation commands                          │
│  ├─ Verify code examples work                               │
│  ├─ Check all links resolve                                 │
│  └─ Ensure no placeholder text remains                      │
│                                                             │
│  Phase 5: OUTPUT                                            │
│  ├─ Write README.md to project root                         │
│  └─ Report summary of what was generated                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Analysis Steps

### Step 1: Scan Project Structure

Search for files matching these patterns to detect language and framework:

- `**/package.json` — Node.js
- `**/pyproject.toml` — Python
- `**/Cargo.toml` — Rust
- `**/go.mod` — Go
- `**/pom.xml` — Java/Maven
- `**/build.gradle` — Java/Gradle
- `src/**/*` — Main source
- `lib/**/*` — Library code
- `tests/**/*` — Test files
- `docs/**/*` — Documentation

Also read root directory listing and any existing `README.md` to preserve manual content.

### Step 2: Extract Project Metadata

```text
# From package files
- Project name
- Version
- Description
- Author/maintainers
- License
- Dependencies (key ones)
- Scripts/commands

# From codebase
- Primary language
- Framework(s)
- Entry points
- Environment variables (from .env.example, config)
- API endpoints (if applicable)
```

### Step 3: Generate README Sections

Use this template structure:

```markdown
# {Project Name}

> {Compelling one-line description from package or inferred}

{Badges: build status, version, license}

## Overview

{2-3 paragraphs explaining:}
- What the project does
- Why it exists (problem it solves)
- Who it's for

## Features

{Bullet list with brief descriptions}
- Feature 1: Description
- Feature 2: Description

## Quick Start

{60-second setup - MUST be tested commands}

### Prerequisites

- {requirement 1}
- {requirement 2}

### Installation

```bash
{installation commands}
```

### Basic Usage

```bash
{usage example}
```

## Documentation

{Table linking to detailed docs if they exist}

| Topic | Link |
| ----- | ---- |
| API Reference | [docs/api.md](docs/api.md) |
| Configuration | [docs/config.md](docs/config.md) |

## Architecture

{Only include if project is complex enough}

```text
{Simple ASCII diagram}
```

## Configuration

{Environment variables and config options}

| Variable | Description | Default |
| -------- | ----------- | ------- |
| `VAR_NAME` | What it does | `default` |

## Development

### Setup

```bash
{dev setup commands}
```

### Running Tests

```bash
{test commands}
```

## Contributing

{Link to CONTRIBUTING.md or brief guidelines}

## License

{License name} - see [LICENSE](LICENSE)
```

---

## Output Styles

### Minimal (`--style minimal`)

Sections included:
- Project name + description
- Quick Start (installation + basic usage)
- License

Best for: Small utilities, scripts, simple libraries

### Comprehensive (`--style comprehensive`) - Default

Sections included:
- All sections in template
- Architecture diagram
- Full configuration reference
- Development guide
- API documentation links

Best for: Production projects, open source, team codebases

---

## Quality Checklist

Before saving README.md, verify:

```text
CONTENT
[ ] Project description is clear and compelling
[ ] Quick Start works (commands tested)
[ ] Features accurately reflect codebase
[ ] No placeholder text like "TODO" or "{}"

FORMAT
[ ] Badges are valid (if included)
[ ] Code blocks have language hints
[ ] Tables properly formatted
[ ] Links point to existing files

ACCURACY
[ ] Version matches package file
[ ] Dependencies are current
[ ] Installation actually works
[ ] Examples produce expected output
```

---

## Example Output

```text
README GENERATION
━━━━━━━━━━━━━━━━━

Exploring codebase...
✓ Detected: Python project (pyproject.toml)
✓ Found: 17 source files, 4 test files
✓ Framework: Click CLI, Pydantic
✓ Entry point: src/invoice_gen/cli.py

Extracting metadata...
✓ Name: invoice-gen
✓ Version: 0.1.0
✓ License: MIT
✓ Dependencies: click, pydantic, faker

Generating README...
✓ README.md written (142 lines)
```
