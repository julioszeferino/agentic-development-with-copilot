# Settings and Persistence

> **Purpose**: Persist user configuration safely with typed defaults and migration-friendly loading.
> **Confidence**: 0.97
> **MCP Validated**: 2026-05-02

## Overview

Obsidian plugins persist settings via `loadData()` and `saveData()`, typically backed by `data.json` in the plugin folder. A robust pattern is to merge stored values over a `DEFAULT_SETTINGS` object and save immediately after UI changes.

## The Pattern

```ts
import { App, Plugin, PluginSettingTab, Setting } from "obsidian";

interface PluginSettings {
  enableBoard: boolean;
  defaultColumn: string;
}

const DEFAULT_SETTINGS: PluginSettings = {
  enableBoard: true,
  defaultColumn: "Backlog",
};

export default class SettingsPlugin extends Plugin {
  settings: PluginSettings;

  async onload(): Promise<void> {
    await this.loadSettings();
    this.addSettingTab(new BoardSettingTab(this.app, this));
  }

  async loadSettings(): Promise<void> {
    const loaded = (await this.loadData()) ?? {};
    this.settings = Object.assign({}, DEFAULT_SETTINGS, loaded);
  }

  async saveSettings(): Promise<void> {
    await this.saveData(this.settings);
  }
}

class BoardSettingTab extends PluginSettingTab {
  plugin: SettingsPlugin;

  constructor(app: App, plugin: SettingsPlugin) {
    super(app, plugin);
    this.plugin = plugin;
  }

  display(): void {
    const { containerEl } = this;
    containerEl.empty();

    new Setting(containerEl)
      .setName("Enable board")
      .addToggle((toggle) =>
        toggle
          .setValue(this.plugin.settings.enableBoard)
          .onChange(async (value) => {
            this.plugin.settings.enableBoard = value;
            await this.plugin.saveSettings();
          })
      );
  }
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| No saved data yet | Defaults used | Use fallback object for first run |
| UI setting changed | `saveData()` persists | Keep saves small and atomic |
| Added new setting key | Backfilled by defaults | Supports forward-compatible updates |

## Common Mistakes

### Wrong

```ts
this.settings = await this.loadData();
// May be null and may miss new keys.
```

### Correct

```ts
const loaded = (await this.loadData()) ?? {};
this.settings = Object.assign({}, DEFAULT_SETTINGS, loaded);
```

## Related

- [Robust Settings Tab](../patterns/robust-settings-tab.md)
- [Manifest and Compatibility](../concepts/manifest-and-compatibility.md)
