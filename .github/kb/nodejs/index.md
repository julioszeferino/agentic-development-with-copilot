# Node.js Knowledge Base

> **Purpose**: Build and publish reliable Node.js applications and packages with correct module semantics and runtime patterns.
> **MCP Validated**: 2026-05-02

## Quick Navigation

### Concepts (< 150 lines each)

| File | Purpose |
|------|---------|
| [concepts/module-systems.md](concepts/module-systems.md) | ESM vs CommonJS semantics, interop, and explicit module markers |
| [concepts/package-json-contract.md](concepts/package-json-contract.md) | Core package metadata and Node-aware fields (`type`, `exports`, `engines`) |
| [concepts/async-and-errors.md](concepts/async-and-errors.md) | Async control flow, EventEmitter error paths, and safe failure handling |
| [concepts/streams-and-backpressure.md](concepts/streams-and-backpressure.md) | Stream pipeline design with backpressure and cancellation |

### Patterns (< 200 lines each)

| File | Purpose |
|------|---------|
| [patterns/dual-module-publish-safe.md](patterns/dual-module-publish-safe.md) | Publish strategy that minimizes dual-package hazards |
| [patterns/production-cli-skeleton.md](patterns/production-cli-skeleton.md) | Production-grade CLI app structure with validation and robust exits |
| [patterns/stream-pipeline-processing.md](patterns/stream-pipeline-processing.md) | Memory-safe file and data processing with `stream/promises` pipeline |

---

## Quick Reference

- [quick-reference.md](quick-reference.md) - Fast lookup for module rules, package fields, and release checks

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Module Markers** | `.mjs`, `.cjs`, and `package.json` `type` determine module interpretation |
| **Exports Contract** | `exports` defines public entry points and encapsulates internals |
| **Runtime Safety** | Async and stream code must propagate and handle errors explicitly |
| **Publish Discipline** | SemVer, `engines`, and artifact curation reduce breakage risk |

---

## Learning Path

| Level | Files |
|-------|-------|
| **Beginner** | concepts/module-systems.md -> concepts/package-json-contract.md |
| **Intermediate** | concepts/async-and-errors.md -> concepts/streams-and-backpressure.md |
| **Advanced** | patterns/stream-pipeline-processing.md -> patterns/dual-module-publish-safe.md |

---

## Agent Usage

| Agent | Primary Files | Use Case |
|-------|---------------|----------|
| python-developer | concepts/package-json-contract.md, quick-reference.md | Cross-language build/release integration planning |
| code-reviewer | concepts/async-and-errors.md, patterns/stream-pipeline-processing.md | Node.js reliability and error-path review |
| kb-architect | index.md, quick-reference.md | Extend Node.js KB with framework-specific sections |
