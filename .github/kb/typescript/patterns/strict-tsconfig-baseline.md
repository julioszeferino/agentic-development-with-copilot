# Pattern: Strict TSConfig Baseline

> **Purpose**: Apply a resilient default compiler profile for long-lived TypeScript codebases.

## Problem

Inconsistent compiler options across packages create hidden type debt, fragile refactors, and runtime mismatches.

## Solution

Define a shared strict baseline config and extend it per package only when justified by runtime constraints.

## Implementation

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
    "verbatimModuleSyntax": true,
    "skipLibCheck": true
  }
}
```

## Why It Works

- Catches invalid assumptions at compile time
- Prevents emitting broken output
- Keeps module semantics explicit and portable

## Trade-offs

- Initial migration may expose many latent errors
- Some legacy code requires staged adoption

## Related

- [TSConfig Foundations](../concepts/tsconfig-foundations.md)
