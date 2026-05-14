# Declaration Files and API Surface

> **Purpose**: Publish TypeScript types that are accurate, stable, and consumer-friendly.
> **Confidence**: 0.97
> **MCP Validated**: 2026-05-02

## Overview

For libraries, `.d.ts` files are part of the public API. If declarations drift from runtime behavior or rely on fragile compiler assumptions, consumers inherit breakage.

## The Pattern

```json
{
  "compilerOptions": {
    "declaration": true,
    "declarationMap": true,
    "emitDeclarationOnly": false,
    "outDir": "dist"
  }
}
```

```json
{
  "name": "example-lib",
  "exports": {
    ".": "./dist/index.js"
  },
  "types": "./dist/index.d.ts"
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `declaration: true` | `.d.ts` output | Required for typed package consumers |
| `declarationMap: true` | Source-linked type maps | Better IDE navigation/debugging |
| `types` field in package | Entry declaration path | Consumer type discovery contract |

## Common Mistakes

### Wrong

```json
{
  "types": "./src/index.ts"
}
```

### Correct

```json
{
  "types": "./dist/index.d.ts"
}
```

## Related

- [Module Systems and Resolution](../concepts/module-systems-and-resolution.md)
- [Library Publish Contract](../patterns/library-publish-contract.md)
