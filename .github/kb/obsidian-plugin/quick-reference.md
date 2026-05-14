# Obsidian Plugin Quick Reference

> Fast lookup tables. For implementation examples, see linked files.

## Required Repository Files (Before Submission)

| File | Required | Notes |
|------|----------|-------|
| `README.md` | Yes | Describe plugin purpose and usage |
| `LICENSE` | Yes | Required for open-source distribution clarity |
| `manifest.json` | Yes | Metadata used by Obsidian and review process |

## Manifest Fields (Plugin-Specific)

| Property | Required | Notes |
|----------|----------|-------|
| `id` | Yes | Must be unique; cannot contain `obsidian`; should match folder name in local dev |
| `name` | Yes | Human-friendly plugin name |
| `version` | Yes | Semantic version in `x.y.z` format |
| `minAppVersion` | Yes | Minimum compatible Obsidian version |
| `description` | Yes | <= 250 chars, concise, ends with period |
| `isDesktopOnly` | Yes | Set `true` when using Node.js/Electron APIs |
| `author` | Yes | Author name |
| `authorUrl` | No | Optional website |
| `fundingUrl` | No | Use only for donation/support links |

## Plugin Lifecycle Essentials

| Stage | Typical Work | Notes |
|-------|--------------|-------|
| `onload()` | Register commands/events/UI/settings | Keep startup light for fast enable/load |
| `onunload()` | Cleanup explicit resources not auto-registered | Avoid aggressive teardown of workspace leaves |

## Registration API Matrix

| Need | API | Cleanup behavior |
|------|-----|------------------|
| App/workspace events | `registerEvent(...)` | Auto-unregistered on unload |
| DOM event on long-lived element | `registerDomEvent(...)` | Auto-unregistered on unload |
| Timers | `registerInterval(...)` | Auto-cleared on unload |
| Command palette entry | `addCommand(...)` | Removed on unload |
| Settings tab | `addSettingTab(...)` | Unloaded with plugin |

## Release Assets (GitHub Release)

| Asset | Required | Notes |
|------|----------|-------|
| `manifest.json` | Yes | Must match plugin metadata |
| `main.js` | Yes | Compiled production bundle |
| `styles.css` | Optional | Include if plugin ships styles |

## Decision Matrix

| Use Case | Choose |
|----------|--------|
| Need mobile support | Avoid Node.js/Electron APIs and keep `isDesktopOnly: false` |
| Need filesystem/process APIs | Use Node.js APIs and set `isDesktopOnly: true` |
| Command only in some contexts | `checkCallback` command variant |
| User configurable behavior | Persist settings with `loadData()` / `saveData()` |

## Common Pitfalls

| Don't | Do |
|-------|-----|
| Prefix command IDs with plugin ID | Use short IDs; Obsidian prefixes automatically |
| Leave sample plugin class names/code | Rename and remove sample scaffolding before submission |
| Put unbounded work in `onload()` | Defer heavy work and keep startup fast |
| Forget to align tag and manifest version | Match release tag to `manifest.json` version exactly |
| Submit without checking `community-plugins.json` ID uniqueness | Search for ID collisions first |

## Related Documentation

| Topic | Path |
|-------|------|
| Plugin lifecycle | `concepts/plugin-lifecycle.md` |
| Manifest and compatibility | `concepts/manifest-and-compatibility.md` |
| Commands and events | `concepts/commands-and-events.md` |
| Settings persistence | `concepts/settings-and-persistence.md` |
| Submission workflow | `patterns/community-submission-checklist.md` |
| Full Index | `index.md` |
