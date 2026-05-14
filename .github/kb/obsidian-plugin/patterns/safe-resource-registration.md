# Safe Resource Registration

> **Purpose**: Prevent memory leaks and stale handlers by centralizing plugin resource registration.
> **MCP Validated**: 2026-05-02

## When to Use

- Your plugin listens to workspace/vault events
- You attach DOM listeners on persistent UI elements
- You schedule timers or polling intervals

## Implementation

```ts
import { Plugin } from "obsidian";

export default class SafeRegistrationPlugin extends Plugin {
  async onload(): Promise<void> {
    this.registerEvent(
      this.app.vault.on("modify", (file) => {
        console.debug("File changed", file.path);
      })
    );

    this.registerEvent(
      this.app.workspace.on("file-open", (file) => {
        if (file) console.debug("Opened", file.path);
      })
    );

    this.registerInterval(
      window.setInterval(() => {
        this.flushBufferedMetrics();
      }, 30_000)
    );

    const root = document.body;
    this.registerDomEvent(root, "click", (evt: MouseEvent) => {
      if ((evt.target as HTMLElement)?.matches(".dev-board-refresh")) {
        this.refreshBoard();
      }
    });
  }

  private flushBufferedMetrics(): void {
    // send batched diagnostics or housekeeping updates
  }

  private refreshBoard(): void {
    // refresh view state
  }
}
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| Poll interval | `30000` ms | Balance responsiveness with battery usage |
| Event scope | vault/workspace | Keep listeners narrow to reduce overhead |
| DOM selector | explicit class | Avoid broad selectors on global roots |

## Example Usage

```ts
// Good: central helper keeps registration style consistent.
private registerWorkspaceListeners(): void {
  this.registerEvent(this.app.workspace.on("layout-change", () => {
    this.recomputeLayout();
  }));
}
```

## See Also

- [Plugin Lifecycle](../concepts/plugin-lifecycle.md)
- [Commands and Events](../concepts/commands-and-events.md)
