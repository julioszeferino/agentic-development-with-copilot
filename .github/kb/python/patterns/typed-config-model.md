# Pattern: Typed Config Model

> **Purpose**: Load runtime configuration safely using explicit schema and validation.

## Problem

Raw environment variables are stringly typed and error-prone, causing runtime failures far from startup.

## Solution

Centralize configuration parsing into a typed model with defaults, coercion, and validation. Fail fast during startup.

## Implementation

```python
from dataclasses import dataclass
import os


@dataclass(frozen=True)
class AppConfig:
    host: str
    port: int
    debug: bool


def load_config() -> AppConfig:
    host = os.getenv("APP_HOST", "127.0.0.1")
    port = int(os.getenv("APP_PORT", "8000"))
    debug = os.getenv("APP_DEBUG", "false").lower() in {"1", "true", "yes"}

    if not (1 <= port <= 65535):
        raise ValueError(f"APP_PORT out of range: {port}")

    return AppConfig(host=host, port=port, debug=debug)
```

## Why It Works

- Moves configuration errors to startup boundary
- Makes config contract discoverable and testable
- Reduces hidden parsing logic spread through codebase

## Trade-offs

- Requires a small upfront schema definition
- Complex config may need dedicated validation libraries

## Related

- [Typing and Static Safety](../concepts/typing-and-static-safety.md)
- [Exceptions and Error Boundaries](../concepts/exceptions-and-error-boundaries.md)
