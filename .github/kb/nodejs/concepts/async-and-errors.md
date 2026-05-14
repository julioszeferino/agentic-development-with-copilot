# Async and Errors

> **Purpose**: Build Node.js services that fail predictably and recover safely across async and evented code.
> **Confidence**: 0.96
> **MCP Validated**: 2026-05-02

## Overview

Node.js async work spans promises, callbacks, and event emitters. Robust systems avoid silent failures by propagating errors through one strategy per boundary. Evented APIs, especially streams and custom emitters, must always handle `error` explicitly.

## The Pattern

```js
import { EventEmitter } from "node:events";

class Worker extends EventEmitter {
  async run(job) {
    try {
      if (!job?.id) throw new Error("Missing job.id");
      // Async work
      await Promise.resolve();
      this.emit("done", job.id);
    } catch (err) {
      this.emit("error", err);
    }
  }
}

const worker = new Worker({ captureRejections: true });

worker.on("done", (id) => {
  console.log("completed", id);
});

worker.on("error", (err) => {
  console.error("worker failure", err.message);
  process.exitCode = 1;
});

await worker.run({ id: "job-42" });
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| Promise rejection without handling | Process warning/crash risk | Always catch at boundary |
| EventEmitter `error` not handled | Uncaught exception path | Add explicit `error` listener |
| Centralized failure policy | Predictable exits | Set `process.exitCode`, avoid abrupt `process.exit()` |

## Common Mistakes

### Wrong

```js
emitter.on("data", async () => {
  throw new Error("boom");
});
```

### Correct

```js
emitter.on("error", (err) => {
  console.error(err);
});

emitter.on("data", async () => {
  try {
    await riskyStep();
  } catch (err) {
    emitter.emit("error", err);
  }
});
```

## Related

- [Streams and Backpressure](../concepts/streams-and-backpressure.md)
- [Production CLI Skeleton](../patterns/production-cli-skeleton.md)
