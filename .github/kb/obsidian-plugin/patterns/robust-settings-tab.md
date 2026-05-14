# Robust Settings Tab

> **Purpose**: Create a typed, user-friendly settings tab that persists immediately and remains migration-safe.
> **MCP Validated**: 2026-05-02

## When to Use

- Plugin behavior must be configurable by end users
- You want stable defaults across upgrades
- Settings changes should take effect immediately

## Implementation

```ts
import { App, Plugin, PluginSettingTab, Setting, Notice } from "obsidian";

interface BoardSettings {
  defaultStatus: string;
  autoOpenBoard: boolean;
  maxItemsPerColumn: number;
}

const DEFAULT_SETTINGS: BoardSettings = {
  defaultStatus: "Todo",
  autoOpenBoard: false,
  maxItemsPerColumn: 30,
};

export default class DevelopmentBoardPlugin extends Plugin {
  settings: BoardSettings;

  async onload(): Promise<void> {
    await this.loadSettings();
    this.addSettingTab(new DevelopmentBoardSettingTab(this.app, this));
  }

  async loadSettings(): Promise<void> {
    const persisted = (await this.loadData()) ?? {};
    this.settings = Object.assign({}, DEFAULT_SETTINGS, persisted);
  }

  async saveSettings(): Promise<void> {
    await this.saveData(this.settings);
  }
}

class DevelopmentBoardSettingTab extends PluginSettingTab {
  plugin: DevelopmentBoardPlugin;

  constructor(app: App, plugin: DevelopmentBoardPlugin) {
    super(app, plugin);
    this.plugin = plugin;
  }

  display(): void {
    const { containerEl } = this;
    containerEl.empty();

    new Setting(containerEl)
      .setName("Default status")
      .setDesc("Initial lane when creating board items")
      .addText((text) =>
        text
          .setPlaceholder("Todo")
          .setValue(this.plugin.settings.defaultStatus)
          .onChange(async (value) => {
            const trimmed = value.trim();
            this.plugin.settings.defaultStatus = trimmed || "Todo";
            await this.plugin.saveSettings();
          })
      );

    new Setting(containerEl)
      .setName("Auto-open board on startup")
      .addToggle((toggle) =>
        toggle
          .setValue(this.plugin.settings.autoOpenBoard)
          .onChange(async (value) => {
            this.plugin.settings.autoOpenBoard = value;
            await this.plugin.saveSettings();
          })
      );

    new Setting(containerEl)
      .setName("Max items per column")
      .setDesc("Clamp to avoid rendering huge columns")
      .addText((text) =>
        text
          .setValue(String(this.plugin.settings.maxItemsPerColumn))
          .onChange(async (value) => {
            const parsed = Number(value);
            if (!Number.isFinite(parsed) || parsed < 1) {
              new Notice("Max items must be a positive number");
              return;
            }
            this.plugin.settings.maxItemsPerColumn = Math.floor(parsed);
            await this.plugin.saveSettings();
          })
      );
  }
}
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| `defaultStatus` | `"Todo"` | Default lane label for new items |
| `autoOpenBoard` | `false` | Open board UI automatically on load |
| `maxItemsPerColumn` | `30` | Guardrail for rendering and performance |

## Example Usage

```ts
if (this.settings.autoOpenBoard) {
  this.openBoardView();
}
```

## See Also

- [Safe Resource Registration](../patterns/safe-resource-registration.md)
- [Settings and Persistence](../concepts/settings-and-persistence.md)
