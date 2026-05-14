# Validators

> **Purpose**: Custom field and model validation logic using Pydantic v2 decorator API
> **Confidence**: 1.00
> **MCP Validated**: 2026-05-02

## Overview

Pydantic v2 introduces `@field_validator` and `@model_validator` as replacements for v1's `@validator`. Field validators run per-field; model validators run on the entire model. Both decorators support `mode="before"` (raw input) and `mode="after"` (validated value).

## The Pattern

```python
from pydantic import BaseModel, field_validator, model_validator
from typing import Self


class Registration(BaseModel):
    username: str
    email: str
    password: str
    password_confirm: str
    age: int

    # --- Field validators ---

    @field_validator("username", mode="before")
    @classmethod
    def normalize_username(cls, v: str) -> str:
        """Runs on raw input; coerce before validation."""
        return v.strip().lower()

    @field_validator("email", mode="after")
    @classmethod
    def validate_email_domain(cls, v: str) -> str:
        """Runs after type validation; check business rules."""
        if not v.endswith(("@example.com", "@corp.com")):
            raise ValueError("Only example.com or corp.com emails allowed")
        return v

    @field_validator("age", mode="after")
    @classmethod
    def validate_age(cls, v: int) -> int:
        if v < 18:
            raise ValueError(f"Must be 18+, got {v}")
        return v

    # --- Model validator (cross-field) ---

    @model_validator(mode="after")
    def passwords_match(self) -> Self:
        """Runs after all fields are validated."""
        if self.password != self.password_confirm:
            raise ValueError("Passwords do not match")
        return self


# ValidationError — inspect all failures at once
from pydantic import ValidationError

try:
    Registration(
        username="BOB",
        email="bob@gmail.com",
        password="secret",
        password_confirm="different",
        age=16,
    )
except ValidationError as exc:
    for error in exc.errors():
        print(error["loc"], error["msg"])
        # ('email',) 'Only example.com or corp.com emails allowed'
        # ('age',)   'Must be 18+, got 16'
        # ()         'Passwords do not match'
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `mode="before"` | raw Python value | Good for parsing/coercion |
| `mode="after"` | validated, typed value | Good for business rules |
| `@model_validator(mode="after")` returns `Self` | modified model | Cross-field checks |
| `raise ValueError(...)` inside validator | `ValidationError` entry | Pydantic wraps it |

## Common Mistakes

### Wrong

```python
class Broken(BaseModel):
    @field_validator("name")        # missing @classmethod
    def check_name(cls, v):         # cls not a classmethod → runtime error
        return v.strip()
```

### Correct

```python
class Fixed(BaseModel):
    name: str

    @field_validator("name", mode="before")
    @classmethod                    # REQUIRED in v2
    def check_name(cls, v: str) -> str:
        return v.strip()
```

## Related

- [BaseModel](../concepts/base-model.md)
- [Custom Validators Pattern](../patterns/custom-validators.md)
- [Field API](../concepts/field-api.md)
