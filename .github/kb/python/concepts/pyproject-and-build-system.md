# pyproject and Build System

> **Purpose**: Define Python package metadata and build behavior using standardized pyproject.toml tables.
> **Confidence**: 0.99
> **MCP Validated**: 2026-05-02

## Overview

pyproject.toml is the central configuration contract for modern Python projects. The build backend and build requirements live in `[build-system]`, core package metadata in `[project]`, and tool-specific settings in `[tool.*]` namespaces.

## The Pattern

```toml
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "example-service"
version = "0.1.0"
description = "Example Python service"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
  "pydantic>=2.0,<3",
  "httpx>=0.27",
]

[project.scripts]
example-service = "example_service.cli:main"

[tool.ruff]
line-length = 100
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `[build-system]` | Build backend contract | Strongly recommended for all package projects |
| `[project]` | Standard metadata | Shared across modern backends |
| `[project.scripts]` | CLI entrypoints | Installs executable wrappers |
| `[tool.*]` | Tool config | Linter/test/type-check settings |

## Common Mistakes

### Wrong

```toml
[project]
name = "my-lib"
# missing requires-python and incomplete metadata
```

### Correct

```toml
[project]
name = "my-lib"
version = "1.0.0"
requires-python = ">=3.10"
```

## Related

- [Environments and Dependencies](../concepts/environments-and-dependencies.md)
- [src Layout Packaging](../patterns/src-layout-packaging.md)
