# Package JSON Contract

> **Purpose**: Define stable package metadata and API boundaries for Node.js applications and libraries.
> **Confidence**: 0.99
> **MCP Validated**: 2026-05-02

## Overview

`package.json` is the behavioral contract for Node runtime, package managers, and downstream consumers. For published packages, `name` and `version` are mandatory. Fields such as `type`, `exports`, and `engines.node` strongly affect interoperability and compatibility.

## The Pattern

```json
{
  "name": "@acme/toolkit",
  "version": "1.4.0",
  "type": "module",
  "exports": {
    ".": "./dist/index.js",
    "./package.json": "./package.json"
  },
  "main": "./dist/index.js",
  "engines": {
    "node": ">=20"
  },
  "files": [
    "dist",
    "README.md",
    "LICENSE"
  ],
  "scripts": {
    "build": "node ./scripts/build.mjs",
    "test": "node --test",
    "prepublishOnly": "npm run test && npm run build"
  }
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| Add `exports` field | Encapsulated public API | Deep imports can break if not explicitly exported |
| Add `engines.node` | Consumer runtime guard | Prevents installs on unsupported Node versions |
| Set `private: true` | Publish disabled | Useful for internal apps/monorepo packages |

## Common Mistakes

### Wrong

```json
{
  "name": "my-lib",
  "version": "1.0.0",
  "exports": "./dist/index.js"
}
```

### Correct

```json
{
  "name": "my-lib",
  "version": "1.0.0",
  "type": "module",
  "exports": {
    ".": "./dist/index.js",
    "./package.json": "./package.json"
  },
  "engines": {
    "node": ">=20"
  }
}
```

## Related

- [Module Systems](../concepts/module-systems.md)
- [Dual Module Publish Safe](../patterns/dual-module-publish-safe.md)
