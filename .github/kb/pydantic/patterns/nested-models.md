# Nested Models

> **Purpose**: Compose complex data structures from reusable sub-models with deep validation
> **MCP Validated**: 2026-05-02

## When to Use

- Representing hierarchical data (order → items → product)
- Reusing sub-schemas across multiple parent models
- Serializing nested structures to JSON for APIs or storage
- Mapping ORM relationships to typed Python objects

## Implementation

```python
from __future__ import annotations

from datetime import datetime
from decimal import Decimal
from enum import Enum

from pydantic import BaseModel, ConfigDict, Field, computed_field


class OrderStatus(str, Enum):
    pending = "pending"
    processing = "processing"
    shipped = "shipped"
    delivered = "delivered"
    cancelled = "cancelled"


class Address(BaseModel):
    street: str
    city: str
    state: str = Field(min_length=2, max_length=2)
    zip_code: str = Field(pattern=r"^\d{5}-\d{3}$")


class Product(BaseModel):
    model_config = ConfigDict(frozen=True)  # immutable value object

    id: str
    name: str
    price: Decimal = Field(gt=0, max_digits=10, decimal_places=2)
    category: str


class OrderItem(BaseModel):
    product: Product
    quantity: int = Field(ge=1)

    @computed_field
    @property
    def subtotal(self) -> Decimal:
        return self.product.price * self.quantity


class Order(BaseModel):
    id: str
    customer_id: str
    status: OrderStatus = OrderStatus.pending
    items: list[OrderItem] = Field(default_factory=list, min_length=1)
    shipping_address: Address
    created_at: datetime = Field(default_factory=datetime.utcnow)

    @computed_field
    @property
    def total(self) -> Decimal:
        return sum(item.subtotal for item in self.items)


# --- Construction from nested dict (API payload / DB row) ---

raw = {
    "id": "ORD-001",
    "customer_id": "CUST-42",
    "shipping_address": {
        "street": "Av. Paulista, 1000",
        "city": "São Paulo",
        "state": "SP",
        "zip_code": "01310-100",
    },
    "items": [
        {
            "product": {
                "id": "PROD-1",
                "name": "Tênis Nike",
                "price": "459.90",
                "category": "calçados",
            },
            "quantity": 2,
        }
    ],
}

order = Order.model_validate(raw)
print(order.total)          # Decimal('919.80')
print(order.status)         # OrderStatus.pending

# Serialize — JSON-safe
payload = order.model_dump(mode="json", exclude_none=True)
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| `frozen=True` on sub-model | — | Makes sub-model hashable/immutable |
| `min_length=1` on list field | — | Prevents empty collections |
| `model_dump(mode="json")` | — | Converts Decimal/datetime to JSON types |
| `from_attributes=True` | — | Add to config for ORM row → model |

## Example Usage

```python
# ORM integration
from pydantic import ConfigDict

class OrderFromDB(Order):
    model_config = ConfigDict(from_attributes=True)

# order_row is a SQLAlchemy Row object
typed_order = OrderFromDB.model_validate(order_row)
```

## See Also

- [BaseModel](../concepts/base-model.md)
- [Field API](../concepts/field-api.md)
- [LLM Output Parsing Pattern](../patterns/llm-output-parsing.md)
