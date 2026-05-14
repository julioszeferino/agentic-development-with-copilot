# Streams and Backpressure

> **Purpose**: Process large data safely using Node.js streams with controlled memory and explicit cancellation.
> **Confidence**: 0.98
> **MCP Validated**: 2026-05-02

## Overview

Streams are the default primitive for scalable I/O in Node.js. `stream/promises` `pipeline()` is the safest composition API because it propagates errors and handles teardown across all stages. For long-running transforms, cancellation should be threaded via `AbortSignal`.

## The Pattern

```js
import { createReadStream, createWriteStream } from "node:fs";
import { pipeline } from "node:stream/promises";
import { createGzip } from "node:zlib";

const controller = new AbortController();

async function compress(inputPath, outputPath) {
  await pipeline(
    createReadStream(inputPath),
    createGzip(),
    createWriteStream(outputPath),
    { signal: controller.signal }
  );
}

try {
  await compress("./input.log", "./input.log.gz");
} catch (err) {
  console.error("pipeline failed", err.message);
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `pipeline(a, b, c)` | Unified flow + teardown | Preferred over manual `pipe()` chains |
| Stream `error` in any stage | Rejects pipeline promise | Handle once at top-level catch |
| `AbortSignal` passed to pipeline | Cooperative cancellation | Enables graceful shutdowns |

## Common Mistakes

### Wrong

```js
readable.pipe(transform).pipe(writable);
// no centralized error handling
```

### Correct

```js
await pipeline(readable, transform, writable);
```

## Related

- [Async and Errors](../concepts/async-and-errors.md)
- [Stream Pipeline Processing](../patterns/stream-pipeline-processing.md)
