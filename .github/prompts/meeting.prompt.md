---
description: |
  Fetch meeting data from Krisp MCP and generate structured notes using
  the meeting-analyst agent. Extracts backlog items to tasks/ folder.
name: "meeting"
agent: "agent"
tools:
  - execute
  - edit
  - agent
---

# Meeting Prompt

> Fetch meeting data from Krisp MCP, analyze with meeting-analyst, and extract backlog tasks.

## Usage

Invoke this prompt from the Copilot Chat prompt picker (type `/` in chat to browse available prompts).

| Option | Effect |
|--------|--------|
| (no argument) | List recent meetings, pick one |
| `"sprint review"` | Search by text query |
| `<32-char-hex-id>` | Fetch specific meeting by document ID |
| `--action-items` | List open action items only |

---

## How It Works

```text
KRISP MCP ──> /meeting prompt ──> meeting-analyst agent ──> outputs
                                         |
                            ┌────────────┴────────────┐
                            v                         v
                   notes/MEETING_*.md          tasks/TASK_*.md
                   (10-section analysis)       (backlog items)
```

---

## Execution Instructions

When this prompt is invoked, follow these steps precisely:

### Step 1: Resolve Input

Parse the user's argument to determine the fetch strategy:

| Input | Strategy |
|-------|----------|
| No argument | Call Krisp MCP `search_meetings` with no query to get recent meetings. Present a numbered list and let the user pick. |
| Text in quotes | Call Krisp MCP `search_meetings` with the text as query. If single result, proceed. If multiple, present list. |
| 32-char hex string | This is a document ID. Call `get_document` to fetch the full transcript, and `search_meetings` with the ID for metadata. |
| `--action-items` | Call Krisp MCP `get_action_items` with `completed=false`. Format as a checklist and optionally generate TASK files. Skip meeting-analyst. |

**Important:** Document IDs in Krisp are 32-character lowercase hex strings (UUID without dashes).

### Step 2: Fetch Meeting Data

Once a meeting is selected:

1. Call `search_meetings` to get metadata (title, date, duration, attendees, summary, key points, action items)
2. Call `get_document` with the meeting's document ID to get the full transcript (if available)
3. Call `get_current_datetime` to get the current date for file naming

Compose all fetched data into a single raw meeting data block.

### Step 3: Invoke Meeting-Analyst

Launch the `meeting-analyst` agent to process the raw data:

```text
Agent(
  subagent_type: "meeting-analyst",
  prompt: """
    Analyze the following meeting data using your 10-section extraction framework.

    MEETING DATA:
    {raw meeting data from Step 2}

    Apply ALL 10 sections:
    1. Key Decisions
    2. Action Items (with owners and dates)
    3. Requirements (FR/NFR with IDs)
    4. Blockers & Risks
    5. Architecture & Technical Decisions
    6. Open Questions
    7. Next Steps & Timeline
    8. Implicit Signals & Sentiment
    9. Stakeholders & Roles
    10. Metrics & Success Criteria

    Output in the Single Meeting Analysis template format.
    Mark sections as N/A if no relevant content found.
  """
)
```

### Step 4: Save Meeting Notes

Write the meeting-analyst output to:

```
notes/MEETING_{YYYY-MM-DD}_{slug}.md
```

Where `{slug}` is derived from the meeting title (lowercase, hyphens, no special chars).

Use this header format:

```markdown
# MEETING: {Title} — Analysis

> **Date:** {date} | **Duration:** {duration} | **Attendees:** {count}
> **Source:** Krisp MCP | **Meeting ID:** {id}
> **Analyzed:** {current timestamp}

{meeting-analyst output here}
```

### Step 5: Extract Backlog Items

Parse the meeting-analyst output and identify engineering-actionable items:

- **Action items** that involve coding, infrastructure, or technical work
- **Requirements** (FR/NFR) that need implementation
- **Architecture decisions** that require code changes
- **Blockers** that need technical resolution

For each actionable cluster (group related items), create a TASK file:

```
tasks/TASK_{YYYY-MM-DD}_{slug}.md
```

### TASK File Template

```markdown
# TASK: {SLUG}

> Extracted from: notes/MEETING_{date}_{meeting-slug}.md
> Created: {timestamp}
> Status: PENDING_VALIDATION

## Summary

{1-2 sentence description of what needs to be done}

## Source Context

- **Meeting:** {meeting title}
- **Date:** {meeting date}
- **Section:** {which section of meeting notes this came from}

## Requirements

- FR-1: {specific, testable requirement}
- FR-2: {specific, testable requirement}

## Suggested Priority

{RISKY | CORE | POLISH} — {reasoning based on urgency, dependencies, risk}

## Suggested Agents

| Component | Agent | Reasoning |
|-----------|-------|-----------|
| {component} | @{agent-name} | {why this agent fits} |

## Dependencies

- {dependency on other tasks, external systems, or decisions}

## Raw Quotes

> {relevant quotes from the meeting transcript that support this task}
```

**Rules for TASK extraction:**

- Only extract items that require engineering work (skip pure business/process items)
- Group related items into a single TASK (e.g., "add auth endpoint" + "write auth tests" = one TASK)
- Include raw quotes for traceability back to the meeting
- Set Status to `PENDING_VALIDATION` (the overnight orchestrator handles promotion)
- If no engineering-actionable items found, skip TASK creation and inform the user

### Step 6: Report to User

After all files are written, present a summary:

```text
MEETING PROCESSED
=================
Notes: notes/MEETING_{date}_{slug}.md
  - {count} decisions
  - {count} action items
  - {count} requirements

Tasks extracted: {count}
  - tasks/TASK_{date}_{slug-1}.md — {summary}
  - tasks/TASK_{date}_{slug-2}.md — {summary}

All tasks have Status: PENDING_VALIDATION
```

## Migration Notes

> **Migrated from Claude Code on 2026-05-02.**
> The following capabilities had no direct GitHub Copilot equivalent:
> - `Krisp MCP` (`mcp__krisp__search_meetings`, `mcp__krisp__get_document`, `mcp__krisp__get_action_items`, `mcp__krisp__get_current_datetime`) — Krisp is a proprietary MCP server with no Copilot-native equivalent.
>
> Workaround: If Krisp MCP is configured in your environment, the agent will attempt to invoke it. If unavailable, ask the user to paste meeting transcript text directly and the meeting-analyst agent will process it from the pasted content.
