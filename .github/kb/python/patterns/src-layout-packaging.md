# Pattern: src Layout Packaging

> **Purpose**: Prevent accidental import-path bugs and validate packaging behavior before publishing.

## Problem

Flat repository layouts can hide packaging mistakes because tests may import directly from source tree paths instead of installed artifacts.

## Solution

Use a `src/` project layout, install in editable mode during development, and run tests against the installed package path.

## Implementation

```text
project/
  pyproject.toml
  src/
    my_package/
      __init__.py
      core.py
  tests/
```

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -e .
pytest
```

## Why It Works

- Ensures imports resolve through installed package metadata
- Exposes missing-package-data and build config issues early
- Keeps packaging and runtime behavior aligned

## Trade-offs

- Slightly more setup than flat layout
- Requires editable install in contributor workflow

## Related

- [pyproject and Build System](../concepts/pyproject-and-build-system.md)
