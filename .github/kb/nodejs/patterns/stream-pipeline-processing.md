# Stream Pipeline Processing

> **Purpose**: Process large files and data feeds in Node.js without memory blowups using pipeline orchestration.
> **MCP Validated**: 2026-05-02

## When to Use

- Transforming large logs, CSV, or JSONL datasets
- Building ingestion jobs for ETL or indexing pipelines
- Compressing, decrypting, or filtering data in streaming mode

## Implementation

```js
import { createReadStream, createWriteStream } from "node:fs";
import { pipeline } from "node:stream/promises";
import { Transform } from "node:stream";

class FilterEmptyLines extends Transform {
  constructor() {
    super({ decodeStrings: false });
    this.buffer = "";
  }

  _transform(chunk, _encoding, callback) {
    this.buffer += chunk;
    const lines = this.buffer.split("\n");
    this.buffer = lines.pop() ?? "";

    for (const line of lines) {
      if (line.trim().length > 0) this.push(line + "\n");
    }
    callback();
  }

  _flush(callback) {
    if (this.buffer.trim().length > 0) this.push(this.buffer + "\n");
    callback();
  }
}

async function processFile(inputPath, outputPath, signal) {
  await pipeline(
    createReadStream(inputPath, { encoding: "utf8" }),
    new FilterEmptyLines(),
    createWriteStream(outputPath),
    { signal }
  );
}

const controller = new AbortController();
processFile("./raw.log", "./clean.log", controller.signal).catch((err) => {
  console.error("pipeline failed", err.message);
  process.exitCode = 1;
});
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| input stream encoding | `Buffer` | Use `utf8` for text transforms |
| transform buffering | custom | Keep partial records between chunks |
| `AbortSignal` | none | Enables cooperative cancellation |

## Example Usage

```bash
node ./scripts/process-log.mjs
```

## See Also

- [Streams and Backpressure](../concepts/streams-and-backpressure.md)
- [Async and Errors](../concepts/async-and-errors.md)
