# Project References and Build Mode

> **Purpose**: Scale TypeScript builds using project references and `tsc --build` orchestration.
> **Confidence**: 0.98
> **MCP Validated**: 2026-05-02

## Overview

Project references split large codebases into compile units with explicit dependency edges. Build mode computes order, reuses incremental metadata, and avoids redundant full-project recompiles.

## The Pattern

```json
{
  "files": [],
  "references": [
    { "path": "./packages/core" },
    { "path": "./packages/api" }
  ]
}
```

```json
{
  "compilerOptions": {
    "composite": true,
    "declaration": true,
    "incremental": true
  }
}
```

```bash
tsc --build
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `references` | Explicit project graph | Enables dependency-aware builds |
| `composite: true` | Reference-compatible project | Required for referenced projects |
| `tsc --build` | Ordered incremental build | Faster CI and local loops |

## Common Mistakes

### Wrong

```json
{
  "compilerOptions": {
    "composite": false
  }
}
```

### Correct

```json
{
  "compilerOptions": {
    "composite": true,
    "incremental": true
  }
}
```

## Related

- [TSConfig Foundations](../concepts/tsconfig-foundations.md)
- [Reference Monorepo Build](../patterns/reference-monorepo-build.md)
