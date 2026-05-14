# Plugin Lifecycle

> **Purpose**: Understand how Obsidian loads and unloads plugins, and where to put initialization and cleanup logic.
> **Confidence**: 0.98
> **MCP Validated**: 2026-05-02

## Overview

Every Obsidian plugin extends the `Plugin` class and uses lifecycle hooks to manage behavior. `onload()` is where commands, events, and UI are registered. `onunload()` should release resources your plugin created manually.

## The Pattern

```ts
import { Notice, Plugin } from "obsidian";

export default class ExamplePlugin extends Plugin {
  async onload(): Promise<void> {
    new Notice("Example plugin loaded");

    this.addCommand({
      id: "say-hello",
      name: "Say hello",
      callback: () => new Notice("Hello from plugin"),
    });

    this.registerEvent(
      this.app.workspace.on("active-leaf-change", () => {
        // lightweight reaction to workspace changes
      })
    );
  }

  onunload(): void {
    // Most resources added through register* and add* APIs are cleaned automatically.
    // Keep only explicit manual cleanup here.
  }
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| Plugin enabled | `onload()` called | Register features here |
| Plugin disabled | `onunload()` called | Cleanup manual resources |
| `registerEvent(...)` usage | Auto cleanup | Preferred over ad-hoc listeners |

## Common Mistakes

### Wrong

```ts
async onload() {
  // Heavy synchronous startup work can block plugin load UX.
  for (let i = 0; i < 100000000; i++) {}
}
```

### Correct

```ts
async onload() {
  // Keep startup minimal; defer heavy operations.
  window.setTimeout(() => this.rebuildLargeIndex(), 0);
}

private rebuildLargeIndex() {
  // expensive work outside critical load path
}
```

## Related

- [Manifest and Compatibility](../concepts/manifest-and-compatibility.md)
- [Safe Resource Registration](../patterns/safe-resource-registration.md)
