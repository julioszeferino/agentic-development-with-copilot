---
description: "Migrated workflow command for phase ship."
agent: "agent"
tools: [read, edit, execute]
---

# Ship Command

> Archive completed feature with lessons learned (Phase 4)

## Usage

Invoke this prompt from the Copilot Chat prompt picker (type `/` in chat and select `workflow_ship`).

| Option | Effect |
|--------|--------|
| Input argument | Uses the provided file, path, or text as context |
| No argument | Runs the full default workflow for this phase |

## Examples

```bash
/ship .github/sdd/features/DEFINE_CLOUD_RUN_FUNCTIONS.md
/ship DEFINE_USER_AUTH.md
```

---

## Overview

This is **Phase 4** of the 5-phase AgentSpec workflow:

```text
Phase 0: /workflow_brainstorm → .github/sdd/features/BRAINSTORM_{FEATURE}.md (optional)
Phase 1: /workflow_define     → .github/sdd/features/DEFINE_{FEATURE}.md
Phase 2: /workflow_design     → .github/sdd/features/DESIGN_{FEATURE}.md
Phase 3: /workflow_build      → Code + .github/sdd/reports/BUILD_REPORT_{FEATURE}.md
Phase 4: /workflow_ship       → .github/sdd/archive/{FEATURE}/SHIPPED_{DATE}.md (THIS COMMAND)
```

The `/workflow_ship` command archives all feature artifacts and captures lessons learned.

---

## What This Command Does

1. **Verify** - Confirm all artifacts exist and build passed
2. **Archive** - Move feature documents to archive folder
3. **Document** - Create SHIPPED summary with lessons learned
4. **Clean** - Remove working files from features folder

---

## Process

### Step 1: Verify Completion

```markdown
read the file at .github/sdd/features/DEFINE_{FEATURE}.md
read the file at .github/sdd/features/DESIGN_{FEATURE}.md
read the file at .github/sdd/reports/BUILD_REPORT_{FEATURE}.md

# Verify build report shows success
```

### Step 2: Create Archive Folder

```bash
mkdir -p .github/sdd/archive/{FEATURE_NAME}/
```

### Step 3: Copy Artifacts to Archive

```bash
cp .github/sdd/features/DEFINE_{FEATURE}.md .github/sdd/archive/{FEATURE}/
cp .github/sdd/features/DESIGN_{FEATURE}.md .github/sdd/archive/{FEATURE}/
cp .github/sdd/reports/BUILD_REPORT_{FEATURE}.md .github/sdd/archive/{FEATURE}/
```

### Step 4: Generate SHIPPED Document

Create summary with:

| Section | Content |
|---------|---------|
| **Summary** | What was built |
| **Timeline** | Start → Ship dates |
| **Metrics** | Lines of code, files created |
| **Lessons Learned** | What went well, what to improve |
| **Artifacts** | List of all archived documents |

### Step 5: Update Document Statuses

Update archived documents to "Shipped" status:

```markdown
Edit: archive/{FEATURE}/DEFINE_{FEATURE}.md
  - Status: → "✅ Shipped"
  - Add revision: "Shipped and archived"

Edit: archive/{FEATURE}/DESIGN_{FEATURE}.md
  - Status: → "✅ Shipped"
  - Add revision: "Shipped and archived"
```

### Step 6: Clean Up Working Files

```bash
rm .github/sdd/features/DEFINE_{FEATURE}.md
rm .github/sdd/features/DESIGN_{FEATURE}.md
rm .github/sdd/reports/BUILD_REPORT_{FEATURE}.md
```

### Step 7: Save SHIPPED Document

```markdown
write to .github/sdd/archive/{FEATURE}/SHIPPED_{DATE}.md
```

---

## Output

| Artifact | Location |
|----------|----------|
| **SHIPPED** | `.github/sdd/archive/{FEATURE}/SHIPPED_{DATE}.md` |
| **DEFINE** | `.github/sdd/archive/{FEATURE}/DEFINE_{FEATURE}.md` |
| **DESIGN** | `.github/sdd/archive/{FEATURE}/DESIGN_{FEATURE}.md` |
| **BUILD_REPORT** | `.github/sdd/archive/{FEATURE}/BUILD_REPORT_{FEATURE}.md` |

**Next Step:** Start new feature with `/define`

---

## Quality Gate

Before shipping, verify:

```text
[ ] BUILD_REPORT shows all tasks completed
[ ] No critical issues in build report
[ ] All tests passing
[ ] Code deployed (if applicable)
```

---

## When to Ship

Ship when:
- All acceptance tests from DEFINE pass
- Build report shows 100% completion
- No blocking issues remain

---

## Lessons Learned Categories

Document lessons in these areas:

| Category | Example |
|----------|---------|
| **Process** | "Breaking tasks into smaller chunks helped" |
| **Technical** | "Config files work better than env vars" |
| **Communication** | "Early clarification saved rework" |
| **Tools** | "Using X library simplified Y" |

---

## Tips

1. **Don't Skip This** - Lessons learned prevent future mistakes
2. **Be Honest** - Document what didn't work too
3. **Be Specific** - "Better planning" → "Create architecture diagram before coding"
4. **Archive Everything** - Future you will thank present you

---

## References

- Agent: `.github/agents/workflow/ship.agent.md`
- Template: `.github/sdd/templates/SHIPPED_TEMPLATE.md`
- Contracts: `.github/sdd/architecture/WORKFLOW_CONTRACTS.yaml`
- Previous Phase: `.github/prompts/workflow_build.md`
