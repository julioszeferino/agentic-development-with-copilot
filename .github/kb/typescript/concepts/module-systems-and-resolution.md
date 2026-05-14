# Module Systems and Resolution

> **Purpose**: Align TypeScript module settings with actual runtime loading behavior.
> **Confidence**: 0.98
> **MCP Validated**: 2026-05-02

## Overview

TypeScript projects fail in production when compile-time module assumptions diverge from runtime semantics. Keep `module`, `moduleResolution`, and `package.json` `type/exports` coherent, then validate in target runtime.

## The Pattern

```json
{
  "compilerOptions": {
    "module": "node18",
    "moduleResolution": "nodenext",
    "verbatimModuleSyntax": true,
    "esModuleInterop": true
  }
}
```

```json
{
  "name": "example-lib",
  "type": "module",
  "exports": {
    ".": "./dist/index.js"
  },
  "types": "./dist/index.d.ts"
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `moduleResolution: nodenext` | Node-accurate import checks | Good default for Node-targeted code |
| `moduleResolution: bundler` | Bundler-like checks | Better for app bundles, riskier for externalized libs |
| `verbatimModuleSyntax: true` | Explicit import/export semantics | Reduces interop ambiguity |

## Common Mistakes

### Wrong

```json
{
  "compilerOptions": {
    "moduleResolution": "bundler"
  }
}
```

### Correct

```json
{
  "compilerOptions": {
    "module": "node18",
    "moduleResolution": "nodenext"
  }
}
```

## Related

- [Declaration Files and API Surface](../concepts/declaration-files-and-api-surface.md)
- [Library Publish Contract](../patterns/library-publish-contract.md)
