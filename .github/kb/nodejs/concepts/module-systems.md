# Module Systems

> **Purpose**: Understand and control Node.js module loading behavior across ESM and CommonJS.
> **Confidence**: 0.99
> **MCP Validated**: 2026-05-02

## Overview

Node.js supports two module systems: ECMAScript Modules (ESM) and CommonJS (CJS). Module interpretation is governed by explicit markers such as file extension (`.mjs`, `.cjs`) and the nearest `package.json` `type` field. Being explicit avoids ambiguous runtime behavior and tooling inconsistencies.

## The Pattern

```js
// package.json
{
  "name": "my-node-package",
  "type": "module",
  "version": "1.0.0"
}

// ESM file: index.js (treated as ESM because type=module)
import { readFile } from "node:fs/promises";
import cjsPkg from "./legacy.cjs";

const text = await readFile("./README.md", "utf8");
console.log(text.length, cjsPkg.version);

// CommonJS file: legacy.cjs (always CJS)
module.exports = {
  version: "legacy-compat"
};
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `.mjs` file | ESM | Always ESM regardless of package type |
| `.cjs` file | CommonJS | Always CJS regardless of package type |
| `.js` + `type: module` | ESM | Nearest package boundary wins |
| `.js` + no explicit type | CommonJS | Default behavior |

## Common Mistakes

### Wrong

```js
// package.json omitted type
// file uses ESM syntax but runtime assumes CommonJS in many contexts
import { join } from "node:path";
```

### Correct

```js
// package.json
{ "type": "module" }

import { join } from "node:path";
```

## Related

- [Package JSON Contract](../concepts/package-json-contract.md)
- [Dual Module Publish Safe](../patterns/dual-module-publish-safe.md)
