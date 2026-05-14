# Commands and Events

> **Purpose**: Implement command palette actions and event listeners with correct IDs and cleanup behavior.
> **Confidence**: 0.97
> **MCP Validated**: 2026-05-02

## Overview

Commands are the primary user-facing integration point for many plugins. Use `addCommand()` for command palette actions and prefer registration APIs (`registerEvent`, `registerDomEvent`, `registerInterval`) to ensure cleanup on unload.

## The Pattern

```ts
import { Notice, Plugin } from "obsidian";

export default class CommandsPlugin extends Plugin {
  async onload(): Promise<void> {
    this.addCommand({
      id: "open-dashboard",
      name: "Open development dashboard",
      callback: () => new Notice("Opening dashboard"),
    });

    this.addCommand({
      id: "run-contextual-action",
      name: "Run contextual action",
      checkCallback: (checking) => {
        const canRun = this.app.workspace.getActiveFile() != null;
        if (!canRun) return false;
        if (!checking) new Notice("Contextual action executed");
        return true;
      },
    });

    this.registerEvent(
      this.app.vault.on("create", (file) => {
        console.log("created", file.path);
      })
    );
  }
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `callback` command | Always executable | Use for unconditional commands |
| `checkCallback` command | Context-aware command | Return `true` only when valid |
| Command ID with plugin prefix | Redundant naming | Obsidian prefixes command IDs automatically |

## Common Mistakes

### Wrong

```ts
this.addCommand({
  id: "my-plugin-open-panel",
  name: "Open panel",
  callback: () => {}
});
```

### Correct

```ts
this.addCommand({
  id: "open-panel",
  name: "Open panel",
  callback: () => {}
});
```

## Related

- [Plugin Lifecycle](../concepts/plugin-lifecycle.md)
- [Safe Resource Registration](../patterns/safe-resource-registration.md)
