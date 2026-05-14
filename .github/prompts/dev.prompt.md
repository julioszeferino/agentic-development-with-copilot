---
description: |
  Dev Loop prompt for Agentic Development (Level 2) with structured iteration and intelligent routing.
  Routes between PROMPT crafting and PROMPT execution flows.
agent: "agent"
tools:
  - read
  - edit
  - search
  - execute
  - agent
---

# Dev Loop Prompt

> Dev Loop - Agentic Development (Level 2) with structured iteration and intelligent routing.

## Usage

Invoke this prompt from the Copilot Chat prompt picker (type `/` in chat to browse available prompts).

| Input Pattern | Behavior |
|--------|--------|
| Natural language description | Trigger prompt-crafter flow (questions + PROMPT generation) |
| `tasks/PROMPT_*.md` | Trigger dev-loop-executor flow (execute an existing PROMPT) |
| `--list` | List available PROMPT files in `.github/dev/tasks/` |
| `--resume` | Resume from existing PROGRESS file |
| `--dry-run` | Validate a PROMPT and show plan without executing |
| `--mode hitl` or `--mode afk` | Choose human-in-the-loop or autonomous execution |
| `--max <number>` | Override max iterations |

### Interpretation Rules

1. If input includes `tasks/PROMPT_*.md`, use Execute mode.
2. If input has no task path and includes free-text intent, use Craft mode.
3. Flags `--resume`, `--dry-run`, `--mode`, and `--max` only apply in Execute mode.
4. If no rule matches clearly, ask a clarifying question before proceeding.

### Precedence

1. `--list` has highest priority and does not require other arguments.
2. Task path (`tasks/PROMPT_*.md`) takes precedence over free text.
3. Free-text description is used only when no task path is provided.

### Examples

1. `/dev "I want to build a date parser"`
2. `/dev tasks/PROMPT_CACHE.md --resume`
3. `/dev tasks/PROMPT_AUTH.md --dry-run --mode hitl --max 20`

## How It Works

This prompt routes between two modes:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              /dev COMMAND ROUTING                                │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│   User Input                              Action                                 │
│   ----------                              ------                                 │
│                                                                                  │
│   /dev "description"         ->  prompt-crafter (ask questions, build PROMPT)    │
│   /dev tasks/PROMPT_*.md     ->  dev-loop-executor (execute the PROMPT)          │
│   /dev --list                ->  Show available PROMPTs                          │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## Mode 1: Craft (New Request)

When the user provides a description (not a file path), route to the prompt-crafter agent:

1. Explores the codebase for context
2. Asks targeted questions to clarify requirements
3. Generates a complete PROMPT.md file
4. Confirms with the user before handoff

Expected artifact path:
- `.github/dev/tasks/PROMPT_{NAME}.md`

---

## Mode 2: Execute (Existing PROMPT)

When the user provides a PROMPT file path, route to the dev-loop-executor agent:

1. Loads PROMPT.md + existing PROGRESS.md
2. Picks next task by priority (RISKY -> CORE -> POLISH)
3. Executes task (invokes `@agent-name` if specified)
4. Verifies with objective commands
5. Updates progress (memory bridge)
6. Loops until done or safeguard triggers

Supported execution flags:
- `--mode afk`
- `--mode hitl`
- `--max N`
- `--dry-run`
- `--resume`

---

## Arguments

| Argument | Description |
|----------|-------------|
| `"description"` | Natural language request -> triggers prompt-crafter |
| `tasks/PROMPT_*.md` | Path to PROMPT file -> triggers executor |
| `--list` | List available PROMPTs in `.github/dev/tasks/` |
| `--mode` | Execution mode: `hitl` (default) or `afk` |
| `--resume` | Resume from existing PROGRESS file (memory bridge) |
| `--dry-run` | Validate and show plan without executing |
| `--max N` | Override max iterations (default: 30) |

---

## Workflow

```text
1. /dev "I want to build X"       # Craft phase
2. Clarify requirements            # Interactive
3. PROMPT.md generated             # Ready to execute
4. /dev tasks/PROMPT_X.md          # Execute phase
5. Loop with verification          # Automated
6. EXIT_COMPLETE                   # Done
```

Manual path flow (skip crafting):
1. Copy `.github/dev/templates/PROMPT_TEMPLATE.md`
2. Create `.github/dev/tasks/PROMPT_MY_TASK.md`
3. Invoke `/dev tasks/PROMPT_MY_TASK.md`

---

## Session Recovery

Recovery files:

| File | Purpose |
|------|---------|
| `progress/PROGRESS_{NAME}.md` | Tracks completed tasks, key decisions, iteration log |
| `logs/LOG_{NAME}_{TS}.md` | Final execution report with statistics |

Folder structure:

```text
.github/dev/
├── _index.md
├── tasks/
│   └── PROMPT_*.md
├── progress/
│   └── PROGRESS_*.md
├── logs/
│   └── LOG_*.md
└── templates/
    ├── PROMPT_TEMPLATE.md
    ├── PROGRESS_TEMPLATE.md
    ├── PROMPT_EXAMPLE_FEATURE.md
    └── PROMPT_EXAMPLE_KB.md
```

---

## See Also

- `.github/dev/_index.md`
- `.github/agents/dev/prompt-crafter.agent.md`
- `.github/agents/dev/dev-loop-executor.agent.md`
- `.github/dev/templates/PROMPT_TEMPLATE.md`

---

Dev Loop v1.1 - Ask first, execute perfectly, recover gracefully.
