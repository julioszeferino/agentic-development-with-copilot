---
description: "Migrated workflow command for phase design."
agent: "agent"
tools: [read, edit, search, web]
---

# Design Command

> Create architecture and technical specification in one pass (Phase 2)

## Usage

Invoke this prompt from the Copilot Chat prompt picker (type `/` in chat and select `workflow_design`).

| Option | Effect |
|--------|--------|
| Input argument | Uses the provided file, path, or text as context |
| No argument | Runs the full default workflow for this phase |

## Examples

```bash
/workflow_design .github/sdd/features/DEFINE_CLOUD_RUN_FUNCTIONS.md
/workflow_design DEFINE_USER_AUTH.md
/workflow_design .github/sdd/features/DEFINE_INVOICE_EXTRACTION.md
```

---

## Overview

This is **Phase 2** of the 5-phase AgentSpec workflow:

```text
Phase 0: /workflow_brainstorm → .github/sdd/features/BRAINSTORM_{FEATURE}.md (optional)
Phase 1: /workflow_define     → .github/sdd/features/DEFINE_{FEATURE}.md
Phase 2: /workflow_design     → .github/sdd/features/DESIGN_{FEATURE}.md (THIS COMMAND)
Phase 3: /workflow_build      → Code + .github/sdd/reports/BUILD_REPORT_{FEATURE}.md
Phase 4: /workflow_ship       → .github/sdd/archive/{FEATURE}/SHIPPED_{DATE}.md
```

The `/workflow_design` command combines what used to be Plan + Spec + ADRs into a single document with architecture decisions inline.

---

## What This Command Does

1. **Analyze** - Understand requirements from DEFINE
2. **Architect** - Design high-level solution with diagrams
3. **Decide** - Document key decisions with rationale (inline ADRs)
4. **Specify** - Create file manifest and code patterns
5. **Plan Testing** - Define testing strategy

---

## Process

### Step 1: Load Context

```markdown
read the file at .github/sdd/features/DEFINE_{FEATURE}.md
read the file at .github/sdd/templates/DESIGN_TEMPLATE.md
read the file at .github/copilot-instructions.md

# Explore codebase for patterns:
search for files matching **/*.py | head -20
search for "class |def " across the repository | sample
```

### Step 2: Create Architecture

Design the solution:

| Component | Content |
|-----------|---------|
| **Overview** | ASCII diagram of system |
| **Components** | List of modules/services |
| **Data Flow** | How data moves through system |
| **Integration Points** | External dependencies |

### Step 3: Document Decisions (Inline ADRs)

For each significant choice:

```markdown
### Decision: {Name}

| Attribute | Value |
|-----------|-------|
| **Status** | Accepted |
| **Date** | YYYY-MM-DD |

**Context:** Why this decision was needed

**Choice:** What we're doing

**Rationale:** Why this approach

**Alternatives Rejected:**
1. Option A - rejected because X
2. Option B - rejected because Y

**Consequences:**
- Trade-off we accept
- Benefit we gain
```

### Step 4: Create File Manifest

List all files to create/modify:

| # | File | Action | Purpose | Dependencies |
|---|------|--------|---------|--------------|
| 1 | `path/to/file.py` | Create | Main handler | None |
| 2 | `path/to/config.yaml` | Create | Configuration | None |
| 3 | `path/to/handler.py` | Create | Request handler | 1, 2 |

### Step 5: Define Code Patterns

Provide copy-paste ready code snippets for key patterns.

### Step 6: Plan Testing Strategy

| Test Type | Scope | Tools |
|-----------|-------|-------|
| Unit | Functions | pytest |
| Integration | API | pytest + requests |
| E2E | Full flow | Manual/automated |

### Step 7: Save

```markdown
write to .github/sdd/features/DESIGN_{FEATURE_NAME}.md
```

---

## Output

| Artifact | Location |
|----------|----------|
| **DESIGN** | `.github/sdd/features/DESIGN_{FEATURE_NAME}.md` |

**Next Step:** `/workflow_build .github/sdd/features/DESIGN_{FEATURE_NAME}.md`

---

## Quality Gate

Before saving, verify:

```text
[ ] Architecture diagram is clear
[ ] All major decisions documented with rationale
[ ] File manifest is complete (all files listed)
[ ] Code patterns are copy-paste ready
[ ] Testing strategy covers requirements
[ ] No circular dependencies in architecture
```

---

## Tips

1. **Diagram First** - ASCII art clarifies thinking
2. **Decisions Are Permanent** - Document the "why" not just "what"
3. **Self-Contained Files** - Each file should work independently
4. **Config Over Code** - Use YAML for tunables, not hardcoded values
5. **Test Early** - Design for testability from the start

---

## References

- Agent: `.github/agents/workflow/design.agent.md`
- Template: `.github/sdd/templates/DESIGN_TEMPLATE.md`
- Contracts: `.github/sdd/architecture/WORKFLOW_CONTRACTS.yaml`
- Next Phase: `.github/prompts/workflow_build.prompt.md`
