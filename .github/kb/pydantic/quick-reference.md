# Pydantic Quick Reference

> Fast lookup tables. For code examples, see linked files.

## Field Types & Defaults

| Scenario | Declaration | Notes |
|----------|-------------|-------|
| Required field | `name: str` | No default → required |
| Optional with None | `name: str \| None = None` | Accepts None |
| Default value | `status: str = "active"` | Immutable defaults inline |
| Mutable default | `tags: list[str] = Field(default_factory=list)` | Never `= []` |
| Computed | `@computed_field @property def full(self) -> str` | Derived, not stored |

## Field() Constraints

| Constraint | Type | Example |
|------------|------|---------|
| `gt` / `ge` | int, float | `Field(ge=0)` → non-negative |
| `lt` / `le` | int, float | `Field(le=100)` |
| `min_length` / `max_length` | str, list | `Field(min_length=1)` |
| `pattern` | str | `Field(pattern=r"^\d{4}$")` |
| `max_digits` / `decimal_places` | Decimal | `Field(max_digits=10)` |
| `alias` | any | `Field(alias="user_name")` |
| `description` | any | Used in JSON Schema / OpenAPI |

## ConfigDict Options

| Option | Values | Effect |
|--------|--------|--------|
| `extra` | `"ignore"` / `"allow"` / `"forbid"` | Handle unknown fields |
| `frozen` | `True` | Immutable + hashable |
| `strict` | `True` | No type coercion |
| `from_attributes` | `True` | ORM / object attribute access |
| `str_strip_whitespace` | `True` | Auto-strip strings |
| `populate_by_name` | `True` | Accept both field name and alias |

## Validator Modes

| Decorator | Mode | Input | Use When |
|-----------|------|-------|----------|
| `@field_validator("f", mode="before")` | pre | raw value | parse/coerce |
| `@field_validator("f", mode="after")` | post | validated value | check rules |
| `@model_validator(mode="before")` | pre | raw dict | rename keys |
| `@model_validator(mode="after")` | post | model instance | cross-field |

## model_dump() Flags

| Flag | Effect |
|------|--------|
| `exclude_none=True` | Drop None values |
| `exclude_unset=True` | Drop fields not explicitly set |
| `by_alias=True` | Use alias names as keys |
| `mode="json"` | JSON-safe types (datetime → str) |

## Decision Matrix

| Use Case | Choose |
|----------|--------|
| Simple data container | `BaseModel` with type hints |
| Environment config | `BaseSettings` from pydantic-settings |
| Immutable value object | `model_config = ConfigDict(frozen=True)` |
| LLM structured output | `BaseModel` + `model_validate()` |
| ORM row → model | `model_config = ConfigDict(from_attributes=True)` |
| Strict no-coercion | `model_config = ConfigDict(strict=True)` |

## Common Pitfalls

| Don't | Do |
|-------|-----|
| `tags: list = []` | `tags: list[str] = Field(default_factory=list)` |
| `@validator` (v1) | `@field_validator` (v2) |
| `class Config:` inner class | `model_config = ConfigDict(...)` |
| Access `.dict()` | Use `.model_dump()` |
| Access `.schema()` | Use `.model_json_schema()` |

## Related Documentation

| Topic | Path |
|-------|------|
| BaseModel basics | `concepts/base-model.md` |
| Field constraints | `concepts/field-api.md` |
| Validators | `concepts/validators.md` |
| Settings / env | `concepts/settings.md` |
| Full Index | `index.md` |
