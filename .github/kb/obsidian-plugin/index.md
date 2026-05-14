# Obsidian Plugin Knowledge Base

> **Purpose**: Build production-ready Obsidian plugins and submit them to the Community Plugins directory.
> **MCP Validated**: 2026-05-02

## Quick Navigation

### Concepts (< 150 lines each)

| File | Purpose |
|------|---------|
| [concepts/plugin-lifecycle.md](concepts/plugin-lifecycle.md) | Plugin class lifecycle (`onload`, `onunload`) and registration APIs |
| [concepts/manifest-and-compatibility.md](concepts/manifest-and-compatibility.md) | `manifest.json` schema and mobile/desktop compatibility rules |
| [concepts/commands-and-events.md](concepts/commands-and-events.md) | Command APIs, event wiring, and command ID conventions |
| [concepts/settings-and-persistence.md](concepts/settings-and-persistence.md) | `loadData()` / `saveData()` settings model and version-safe defaults |

### Patterns (< 200 lines each)

| File | Purpose |
|------|---------|
| [patterns/robust-settings-tab.md](patterns/robust-settings-tab.md) | Strongly typed settings tab with safe defaults and persistence |
| [patterns/safe-resource-registration.md](patterns/safe-resource-registration.md) | Auto-cleanup pattern using `registerEvent`, `registerDomEvent`, and `registerInterval` |
| [patterns/community-submission-checklist.md](patterns/community-submission-checklist.md) | End-to-end release and submission workflow for Obsidian Community Plugins |

---

## Quick Reference

- [quick-reference.md](quick-reference.md) - Fast lookup for manifest fields, release artifacts, and submission constraints

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Plugin Lifecycle** | `onload()` sets up behavior; `onunload()` must release plugin-owned resources |
| **Manifest Contract** | `manifest.json` fields define identity, compatibility, and review metadata |
| **Registration APIs** | Prefer `register*` APIs so Obsidian can automatically clean up on unload |
| **Community Submission** | Release assets + PR to `obsidian-releases/community-plugins.json` |

---

## Learning Path

| Level | Files |
|-------|-------|
| **Beginner** | concepts/manifest-and-compatibility.md -> concepts/plugin-lifecycle.md |
| **Intermediate** | concepts/commands-and-events.md -> concepts/settings-and-persistence.md |
| **Advanced** | patterns/safe-resource-registration.md -> patterns/community-submission-checklist.md |

---

## Agent Usage

| Agent | Primary Files | Use Case |
|-------|---------------|----------|
| plugin-developer | concepts/plugin-lifecycle.md, concepts/commands-and-events.md | Build plugin features safely |
| release-maintainer | patterns/community-submission-checklist.md | Ship, tag, and submit to community |
| qa-reviewer | quick-reference.md, patterns/safe-resource-registration.md | Pre-submission compliance checks |
