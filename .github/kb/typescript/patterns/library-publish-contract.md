# Pattern: Library Publish Contract

> **Purpose**: Publish TypeScript libraries with stable runtime and type entry points.

## Problem

Library consumers break when `exports`, emitted JS, and declaration paths are inconsistent or ambiguous across module systems.

## Solution

Use an explicit package contract: declaration emit, controlled exports map, and CI checks for build + type integrity.

## Implementation

```json
{
  "name": "my-lib",
  "type": "module",
  "exports": {
    ".": "./dist/index.js"
  },
  "types": "./dist/index.d.ts",
  "files": [
    "dist"
  ],
  "scripts": {
    "build": "tsc -p tsconfig.json",
    "check": "tsc -p tsconfig.json --noEmit"
  }
}
```

```json
{
  "compilerOptions": {
    "declaration": true,
    "declarationMap": true,
    "outDir": "dist",
    "verbatimModuleSyntax": true
  }
}
```

## Why It Works

- Declares single source of truth for consumer imports and types
- Reduces ESM/CJS ambiguity with explicit syntax and output
- Improves release confidence through reproducible checks

## Trade-offs

- Requires careful migration when changing package entry points
- Dual-format publishing needs extra validation to avoid hazards

## Related

- [Declaration Files and API Surface](../concepts/declaration-files-and-api-surface.md)
- [Module Systems and Resolution](../concepts/module-systems-and-resolution.md)
