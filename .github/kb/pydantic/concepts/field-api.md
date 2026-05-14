# Field API

> **Purpose**: Declare constraints, defaults, aliases, and metadata on model fields
> **Confidence**: 1.00
> **MCP Validated**: 2026-05-02

## Overview

`Field()` extends plain type annotations with validation constraints, default factories, JSON aliases, and documentation metadata. In Pydantic v2 the preferred pattern is `Annotated[type, Field(...)]` for reuse across models.

## The Pattern

```python
from decimal import Decimal
from pydantic import BaseModel, Field
from typing import Annotated


# Reusable annotated type
PositivePrice = Annotated[Decimal, Field(gt=0, max_digits=10, decimal_places=2)]
ShortStr     = Annotated[str, Field(min_length=1, max_length=100)]


class Product(BaseModel):
    # Required — no default
    name: ShortStr

    # Numeric constraints
    price: PositivePrice
    quantity: int = Field(ge=0, description="Units in stock")

    # String pattern
    sku: str = Field(pattern=r"^[A-Z]{2}\d{6}$", examples=["AB123456"])

    # Alias — accept "category_name" in input, expose as "category"
    category: str = Field(alias="category_name", default="general")

    # Mutable default via factory
    tags: list[str] = Field(default_factory=list)

    # Computed field — derived, not stored, included in model_dump()
    from pydantic import computed_field

    @computed_field
    @property
    def display_name(self) -> str:
        return f"{self.sku} — {self.name}"


# Accept both field name and alias during construction
from pydantic import ConfigDict

class FlexProduct(Product):
    model_config = ConfigDict(populate_by_name=True)
```

## Quick Reference

| Constraint | Applies To | Example |
|------------|-----------|---------|
| `gt` / `ge` | int, float, Decimal | `Field(ge=0)` |
| `lt` / `le` | int, float, Decimal | `Field(le=1000)` |
| `min_length` / `max_length` | str, list, bytes | `Field(max_length=255)` |
| `pattern` | str | `Field(pattern=r"^\d{5}$")` |
| `max_digits` / `decimal_places` | Decimal | `Field(max_digits=8, decimal_places=2)` |
| `alias` | any | Input key name different from attribute |
| `default_factory` | any mutable | `Field(default_factory=dict)` |
| `description` | any | Shown in JSON Schema / docs |
| `examples` | any | OpenAPI examples |

## Common Mistakes

### Wrong

```python
class Order(BaseModel):
    items: list[str] = []              # shared mutable default
    price: float = Field(gt=0)        # float for money — loses precision
```

### Correct

```python
from decimal import Decimal

class Order(BaseModel):
    items: list[str] = Field(default_factory=list)
    price: Decimal = Field(gt=0, max_digits=12, decimal_places=2)
```

## Related

- [BaseModel](../concepts/base-model.md)
- [Validators](../concepts/validators.md)
- [Nested Models Pattern](../patterns/nested-models.md)
