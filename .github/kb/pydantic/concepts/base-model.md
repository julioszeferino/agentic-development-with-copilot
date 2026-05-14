# BaseModel

> **Purpose**: Declare data models with automatic validation, type coercion, and serialization
> **Confidence**: 1.00
> **MCP Validated**: 2026-05-02

## Overview

`BaseModel` is the core Pydantic class. Every attribute with a type annotation becomes a validated field. Pydantic validates and coerces values on instantiation, raising `ValidationError` with detailed error paths when input is invalid.

## The Pattern

```python
from pydantic import BaseModel, ConfigDict


class Address(BaseModel):
    street: str
    city: str
    zip_code: str


class User(BaseModel):
    model_config = ConfigDict(str_strip_whitespace=True, extra="forbid")

    id: int
    name: str
    email: str
    age: int | None = None
    address: Address | None = None
    tags: list[str] = []


# Instantiation — validates immediately
user = User(id=1, name="  Ana  ", email="ana@example.com")
print(user.name)   # "Ana"  (whitespace stripped by config)

# Serialization
user.model_dump()                  # dict, Python types
user.model_dump(mode="json")       # dict, JSON-safe types
user.model_dump(exclude_none=True) # omit None fields
user.model_dump_json()             # JSON string directly

# Construction from dict / ORM row
data = {"id": 2, "name": "Bob", "email": "bob@example.com"}
User.model_validate(data)

# JSON string input
User.model_validate_json('{"id": 3, "name": "Carl", "email": "c@x.com"}')
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `"42"` for `int` field | `42` | Coercion (non-strict mode) |
| `None` for required field | `ValidationError` | Cannot be None |
| Extra fields (extra="forbid") | `ValidationError` | Blocked by config |
| `user.model_dump(exclude_unset=True)` | Only set fields | Useful for PATCH APIs |

## Common Mistakes

### Wrong

```python
# Mutable default — shared across instances (Python gotcha)
class Item(BaseModel):
    tags: list[str] = []   # BAD: same list for every instance
```

### Correct

```python
from pydantic import BaseModel, Field

class Item(BaseModel):
    tags: list[str] = Field(default_factory=list)  # GOOD: new list per instance
```

## Related

- [Field API](../concepts/field-api.md)
- [Validators](../concepts/validators.md)
- [LLM Output Parsing Pattern](../patterns/llm-output-parsing.md)
