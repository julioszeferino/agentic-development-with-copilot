# Pattern: Reference Monorepo Build

> **Purpose**: Speed up builds and isolate responsibility in multi-package TypeScript repositories.

## Problem

Monorepos with many packages become slow and error-prone when every package compiles independently without dependency-aware orchestration.

## Solution

Model package dependencies with TypeScript project references and run `tsc --build` from a root solution config.

## Implementation

```json
{
  "files": [],
  "references": [
    { "path": "./packages/shared" },
    { "path": "./packages/web" },
    { "path": "./packages/cli" }
  ]
}
```

```json
{
  "compilerOptions": {
    "composite": true,
    "declaration": true,
    "incremental": true,
    "tsBuildInfoFile": "./.tsbuildinfo"
  }
}
```

```bash
tsc --build --verbose
```

## Why It Works

- Computes topological build order automatically
- Rebuilds only impacted projects when inputs change
- Encourages clean boundaries between package APIs

## Trade-offs

- Requires disciplined inter-package import boundaries
- Misconfigured references can create confusing build graph errors

## Related

- [Project References and Build Mode](../concepts/project-references-and-build-mode.md)
