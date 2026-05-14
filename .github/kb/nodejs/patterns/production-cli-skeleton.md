# Production CLI Skeleton

> **Purpose**: Create reliable Node.js command-line apps with validation, clear exits, and composable command handlers.
> **MCP Validated**: 2026-05-02

## When to Use

- Building internal automation tools and developer CLIs
- Shipping npm binaries used in CI/CD
- Standardizing command structure and runtime error behavior

## Implementation

```js
#!/usr/bin/env node
import { argv, exitCode } from "node:process";
import { readFile } from "node:fs/promises";

function parseArgs(args) {
  const [,, command, ...rest] = args;
  return { command, rest };
}

async function runValidate(filePath) {
  if (!filePath) throw new Error("Missing file path");
  const content = await readFile(filePath, "utf8");
  if (!content.trim()) throw new Error("Input file is empty");
  console.log("VALID");
}

async function main() {
  const { command, rest } = parseArgs(argv);

  switch (command) {
    case "validate":
      await runValidate(rest[0]);
      break;
    case "help":
    case undefined:
      console.log("Usage: mycli validate <file>");
      break;
    default:
      throw new Error(`Unknown command: ${command}`);
  }
}

main().catch((err) => {
  console.error("ERROR", err.message);
  process.exitCode = 1;
});
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| `bin` in package.json | none | Maps executable name to entry file |
| `process.exitCode` | `0` | Set non-zero on controlled failure |
| command dispatch | switch | Deterministic routing for subcommands |

## Example Usage

```bash
mycli validate ./payload.json
```

## See Also

- [Async and Errors](../concepts/async-and-errors.md)
- [Package JSON Contract](../concepts/package-json-contract.md)
