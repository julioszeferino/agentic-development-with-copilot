# Environments and Dependencies

> **Purpose**: Isolate Python project dependencies and keep installs reproducible across machines.
> **Confidence**: 0.99
> **MCP Validated**: 2026-05-02

## Overview

Python projects should run inside dedicated virtual environments to avoid dependency conflicts and accidental global pollution. Use interpreter-scoped pip commands and record dependency state intentionally for reproducibility and deployment confidence.

## The Pattern

```bash
# create and activate a local environment
python -m venv .venv
source .venv/bin/activate

# install runtime dependencies
python -m pip install fastapi uvicorn

# inspect and record environment
python -m pip list
python -m pip freeze > requirements.txt
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `python -m venv .venv` | Project-local env | Isolates interpreter and installed packages |
| `python -m pip install ...` | Package install | Binds pip execution to active interpreter |
| `python -m pip freeze` | Version snapshot | Reinstallable dependency list |

## Common Mistakes

### Wrong

```bash
pip install requests
```

### Correct

```bash
python -m pip install requests
```

## Related

- [pyproject and Build System](../concepts/pyproject-and-build-system.md)
- [src Layout Packaging](../patterns/src-layout-packaging.md)
