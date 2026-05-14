# TSConfig Foundations

> **Purpose**: Establish a strict, predictable compiler baseline for TypeScript projects.
> **Confidence**: 0.99
> **MCP Validated**: 2026-05-02

## Overview

`tsconfig.json` is the contract between your code, tooling, and runtime expectations. A strict baseline prevents accidental `any` usage, unsafe indexing, and silent type regressions.

## The Pattern

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "node18",
    "moduleResolution": "nodenext",
    "strict": true,
    "noImplicitOverride": true,
    "noUncheckedIndexedAccess": true,
    "noEmitOnError": true,
    "skipLibCheck": true
  }
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `strict: true` | Full strict checks | Enables strict family defaults |
| `noEmitOnError: true` | No broken artifacts | Prevents shipping invalid type state |
| `noUncheckedIndexedAccess: true` | Safer indexing | Reflects possible `undefined` |

## Common Mistakes

### Wrong

```json
{
  "compilerOptions": {
    "strict": false
  }
}
```

### Correct

```json
{
  "compilerOptions": {
    "strict": true,
    "noEmitOnError": true
  }
}
```

## Related

- [Module Systems and Resolution](../concepts/module-systems-and-resolution.md)
- [Strict TSConfig Baseline](../patterns/strict-tsconfig-baseline.md)
