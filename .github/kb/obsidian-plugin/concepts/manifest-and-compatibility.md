# Manifest and Compatibility

> **Purpose**: Define plugin identity and compatibility using a correct `manifest.json`.
> **Confidence**: 0.99
> **MCP Validated**: 2026-05-02

## Overview

`manifest.json` is the contract between your plugin, Obsidian, and the community review process. It controls identity (`id`, `name`), release compatibility (`version`, `minAppVersion`), and runtime support (`isDesktopOnly`).

## The Pattern

```json
{
  "id": "development-board",
  "name": "Development Board",
  "version": "1.0.0",
  "minAppVersion": "1.6.0",
  "description": "Organize plugin development tasks and review steps in a focused board view.",
  "author": "Your Name",
  "authorUrl": "https://github.com/your-handle",
  "isDesktopOnly": false
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `id` contains `obsidian` | Review rejection risk | IDs cannot include `obsidian` |
| Uses Node/Electron API | `isDesktopOnly: true` | Required for desktop-only APIs |
| Unknown minimum version | Use latest stable build | Safe default per docs |

## Common Mistakes

### Wrong

```json
{
  "id": "obsidian-dev-board",
  "description": "This is a plugin that helps people build software"
}
```

### Correct

```json
{
  "id": "development-board",
  "description": "Plan and track Obsidian plugin development tasks.",
  "isDesktopOnly": false
}
```

## Related

- [Plugin Lifecycle](../concepts/plugin-lifecycle.md)
- [Community Submission Checklist](../patterns/community-submission-checklist.md)
