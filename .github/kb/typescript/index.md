# TypeScript Knowledge Base

> **Purpose**: Build safe, maintainable TypeScript applications and libraries with predictable compiler and module behavior.
> **MCP Validated**: 2026-05-02

## Quick Navigation

### Concepts (< 150 lines each)

| File | Purpose |
|------|---------|
| [concepts/tsconfig-foundations.md](concepts/tsconfig-foundations.md) | Core compiler options and strictness baseline for reliable type checking |
| [concepts/module-systems-and-resolution.md](concepts/module-systems-and-resolution.md) | ESM/CJS behavior, moduleResolution strategy, and interop constraints |
| [concepts/declaration-files-and-api-surface.md](concepts/declaration-files-and-api-surface.md) | Public type contracts and declaration publishing expectations |
| [concepts/project-references-and-build-mode.md](concepts/project-references-and-build-mode.md) | Multi-project architecture with references and incremental builds |

### Patterns (< 200 lines each)

| File | Purpose |
|------|---------|
| [patterns/strict-tsconfig-baseline.md](patterns/strict-tsconfig-baseline.md) | Production-ready strict tsconfig baseline for apps and libraries |
| [patterns/library-publish-contract.md](patterns/library-publish-contract.md) | Package exports/types contract for robust consumer compatibility |
| [patterns/reference-monorepo-build.md](patterns/reference-monorepo-build.md) | Project references setup for scalable monorepo and CI builds |

---

## Quick Reference

- [quick-reference.md](quick-reference.md) - tsconfig flags, module choices, declaration strategy, and release checks

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Strictness by Default** | `strict` and related checks catch incorrect assumptions before runtime |
| **Module Clarity** | Align `module`, `moduleResolution`, and `package.json` fields to avoid interop bugs |
| **Stable Public Types** | `.d.ts` output defines your package contract as much as runtime code |
| **Composable Builds** | Project references and `tsc --build` scale TypeScript compilation in larger repos |

---

## Learning Path

| Level | Files |
|-------|-------|
| **Beginner** | concepts/tsconfig-foundations.md -> concepts/module-systems-and-resolution.md |
| **Intermediate** | concepts/declaration-files-and-api-surface.md -> concepts/project-references-and-build-mode.md |
| **Advanced** | patterns/strict-tsconfig-baseline.md -> patterns/library-publish-contract.md |

---

## Agent Usage

| Agent | Primary Files | Use Case |
|-------|---------------|----------|
| typescript-developer | concepts/tsconfig-foundations.md, patterns/library-publish-contract.md | Compiler config, API typing, and library architecture |
| code-reviewer | concepts/module-systems-and-resolution.md, quick-reference.md | Interop risk review and release-readiness checks |
| kb-architect | index.md, quick-reference.md | Extend TS KB with framework-specific tracks |
