# Exceptions and Error Boundaries

> **Purpose**: Make Python failures diagnosable and predictable with explicit exception handling boundaries.
> **Confidence**: 0.97
> **MCP Validated**: 2026-05-02

## Overview

Well-structured Python systems define where exceptions are caught, logged, transformed, and re-raised. Catch specific exceptions close to recovery logic, and preserve context at application boundaries to keep failures actionable.

## The Pattern

```python
import logging

logger = logging.getLogger(__name__)


class ConfigurationError(RuntimeError):
    pass


def load_port(raw: str) -> int:
    try:
        value = int(raw)
    except ValueError as exc:
        raise ConfigurationError(f"Invalid PORT value: {raw}") from exc

    if not (1 <= value <= 65535):
        raise ConfigurationError(f"PORT out of range: {value}")
    return value


def main() -> int:
    try:
        port = load_port("8080")
        logger.info("Starting service on %s", port)
        return 0
    except ConfigurationError:
        logger.exception("Configuration failed")
        return 2
    except Exception:
        logger.exception("Unexpected failure")
        return 1
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| Specific `except` clause | Targeted recovery | Prefer over broad catch-all |
| `raise ... from exc` | Causal traceback | Preserve original exception context |
| Boundary logger + exit code | Operable failure signal | Useful for CLI and service startup flows |

## Common Mistakes

### Wrong

```python
try:
    run()
except Exception:
    pass
```

### Correct

```python
try:
    run()
except Exception:
    logger.exception("run failed")
    raise
```

## Related

- [Typing and Static Safety](../concepts/typing-and-static-safety.md)
- [CLI Entrypoint Architecture](../patterns/cli-entrypoint-architecture.md)
