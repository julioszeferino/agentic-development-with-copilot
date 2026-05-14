# Typing and Static Safety

> **Purpose**: Use Python type hints to improve correctness, readability, and tooling support.
> **Confidence**: 0.98
> **MCP Validated**: 2026-05-02

## Overview

Type hints communicate intent and enable static analysis without changing runtime semantics by default. Modern Python supports concise built-in generic syntax, union operators, and richer typing constructs such as Annotated and Callable signatures.

## The Pattern

```python
from collections.abc import Callable
from typing import Annotated

UserId = Annotated[int, "database primary key"]


def load_user(user_id: UserId) -> dict[str, str] | None:
    if user_id <= 0:
        return None
    return {"id": str(user_id), "status": "active"}


def run_with_retry(task: Callable[[], str], attempts: int = 3) -> str:
    last_error: Exception | None = None
    for _ in range(attempts):
        try:
            return task()
        except Exception as exc:
            last_error = exc
    raise RuntimeError("Task failed after retries") from last_error
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `list[str]` | Typed sequence | Built-in generic style |
| `A | B` | Union type | Python 3.10+ preferred union syntax |
| `Callable[[X], Y]` | Function contract | Explicit parameter and return types |
| `Annotated[T, meta]` | Type + metadata | Metadata available to tools/runtime helpers |

## Common Mistakes

### Wrong

```python
def parse(data):
    return data["value"]
```

### Correct

```python
def parse(data: dict[str, str]) -> str:
    return data["value"]
```

## Related

- [Exceptions and Error Boundaries](../concepts/exceptions-and-error-boundaries.md)
- [Typed Config Model](../patterns/typed-config-model.md)
