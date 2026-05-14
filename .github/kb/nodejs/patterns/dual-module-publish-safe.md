# Dual Module Publish Safe

> **Purpose**: Publish Node.js packages with predictable interop while minimizing dual-package hazards.
> **MCP Validated**: 2026-05-02

## When to Use

- You are shipping a reusable npm package
- You need compatibility across consumers using ESM and CommonJS
- You want explicit public entry points and controlled package surface

## Implementation

```json
{
  "name": "@acme/kit",
  "version": "1.0.0",
  "type": "module",
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    },
    "./package.json": "./package.json"
  },
  "main": "./dist/index.cjs",
  "types": "./dist/index.d.ts",
  "engines": {
    "node": ">=20"
  },
  "files": [
    "dist",
    "README.md",
    "LICENSE"
  ],
  "scripts": {
    "build": "npm run build:esm && npm run build:cjs",
    "build:esm": "node ./scripts/build-esm.mjs",
    "build:cjs": "node ./scripts/build-cjs.cjs",
    "test": "node --test",
    "prepublishOnly": "npm run test && npm run build"
  }
}
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| `exports.import` | none | ESM entry point |
| `exports.require` | none | CommonJS entry point |
| `engines.node` | advisory | Consumer runtime guardrail |
| `files` | all files | Restricts published artifacts |

## Example Usage

```js
// ESM consumer
import { createClient } from "@acme/kit";

// CommonJS consumer
const { createClient } = require("@acme/kit");
```

## See Also

- [Module Systems](../concepts/module-systems.md)
- [Package JSON Contract](../concepts/package-json-contract.md)
