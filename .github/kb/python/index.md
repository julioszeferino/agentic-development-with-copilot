# Python Knowledge Base

> **Purpose**: Build maintainable Python applications and packages using modern runtime, typing, environment, and packaging practices.
> **MCP Validated**: 2026-05-02

## Quick Navigation

### Concepts (< 150 lines each)

| File | Purpose |
|------|---------|
| [concepts/environments-and-dependencies.md](concepts/environments-and-dependencies.md) | Virtual environments, pip workflow, and reproducible dependency management |
| [concepts/typing-and-static-safety.md](concepts/typing-and-static-safety.md) | Type hints, generics, and practical static analysis conventions |
| [concepts/exceptions-and-error-boundaries.md](concepts/exceptions-and-error-boundaries.md) | Error propagation, exception boundaries, and predictable failure design |
| [concepts/pyproject-and-build-system.md](concepts/pyproject-and-build-system.md) | pyproject.toml structure, metadata, and build backend contracts |

### Patterns (< 200 lines each)

| File | Purpose |
|------|---------|
| [patterns/src-layout-packaging.md](patterns/src-layout-packaging.md) | src layout packaging pattern for reliable imports and distribution testing |
| [patterns/typed-config-model.md](patterns/typed-config-model.md) | Typed runtime configuration with validation and safe defaults |
| [patterns/cli-entrypoint-architecture.md](patterns/cli-entrypoint-architecture.md) | Robust CLI entrypoint with argument parsing and controlled exit behavior |

---

## Quick Reference

- [quick-reference.md](quick-reference.md) - Fast lookup for venv, pyproject fields, typing constructs, and release checks

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Isolated Environments** | Use project-local virtual environments to avoid cross-project dependency drift |
| **Typed Interfaces** | Type hints and static checks reduce runtime ambiguity in larger codebases |
| **Structured Metadata** | pyproject.toml centralizes package identity, dependencies, and build behavior |
| **Controlled Failure** | Explicit exception handling and boundary logging improve reliability |

---

## Learning Path

| Level | Files |
|-------|-------|
| **Beginner** | concepts/environments-and-dependencies.md -> concepts/pyproject-and-build-system.md |
| **Intermediate** | concepts/typing-and-static-safety.md -> concepts/exceptions-and-error-boundaries.md |
| **Advanced** | patterns/src-layout-packaging.md -> patterns/typed-config-model.md |

---

## Agent Usage

| Agent | Primary Files | Use Case |
|-------|---------------|----------|
| python-developer | concepts/typing-and-static-safety.md, patterns/src-layout-packaging.md | Production Python architecture and refactoring |
| code-reviewer | concepts/exceptions-and-error-boundaries.md, quick-reference.md | Reliability and maintainability review |
| kb-architect | index.md, quick-reference.md | Extend Python KB with framework-specific topics |
