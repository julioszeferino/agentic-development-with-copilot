# Custom Validators

> **Purpose**: Reusable field and model validation with rich error messages and cross-field logic
> **MCP Validated**: 2026-05-02

## When to Use

- Enforcing business rules beyond built-in constraints (e.g., CNPJ, CPF, date ranges)
- Normalizing input before type validation (strip, uppercase, parse)
- Cross-field invariants (password confirmation, start < end date)
- Producing user-friendly, actionable error messages

## Implementation

```python
from datetime import date
from typing import Self

from pydantic import BaseModel, Field, ValidationError, field_validator, model_validator


# --- Reusable validator helpers ---

def validate_cpf(value: str) -> str:
    """Normalize and validate Brazilian CPF."""
    digits = "".join(filter(str.isdigit, value))
    if len(digits) != 11 or len(set(digits)) == 1:
        raise ValueError(f"CPF inválido: '{value}'")
    # Simplified check-digit logic
    for i in range(9, 11):
        total = sum(int(digits[j]) * (i + 1 - j) for j in range(i))
        expected = (total * 10 % 11) % 10
        if int(digits[i]) != expected:
            raise ValueError(f"CPF inválido: dígito verificador errado em '{value}'")
    return digits  # return normalized (digits only)


# --- Model using validators ---

class CustomerForm(BaseModel):
    cpf: str
    email: str
    birth_date: date
    start_date: date
    end_date: date
    discount_pct: float = Field(ge=0.0, le=1.0)

    @field_validator("cpf", mode="before")
    @classmethod
    def normalize_cpf(cls, v: str) -> str:
        return validate_cpf(v)

    @field_validator("email", mode="before")
    @classmethod
    def lowercase_email(cls, v: str) -> str:
        return v.strip().lower()

    @field_validator("birth_date", mode="after")
    @classmethod
    def must_be_adult(cls, v: date) -> date:
        from datetime import date as dt
        age = (dt.today() - v).days // 365
        if age < 18:
            raise ValueError(f"Cliente deve ter 18+, calculado: {age} anos")
        return v

    @model_validator(mode="after")
    def date_range_valid(self) -> Self:
        if self.end_date <= self.start_date:
            raise ValueError(
                f"end_date ({self.end_date}) deve ser posterior a start_date ({self.start_date})"
            )
        return self


# --- Collecting all validation errors at once ---

def validate_form(data: dict) -> tuple[CustomerForm | None, list[str]]:
    try:
        return CustomerForm.model_validate(data), []
    except ValidationError as exc:
        errors = [
            f"{'.'.join(str(loc) for loc in e['loc'])}: {e['msg']}"
            for e in exc.errors()
        ]
        return None, errors
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| `mode="before"` | — | Validator sees raw input string |
| `mode="after"` | — | Validator sees typed, validated value |
| `@model_validator(mode="after")` returns `Self` | — | Pydantic v2 signature |
| `exc.errors()` | list of dicts | Each has `loc`, `msg`, `type` |

## Example Usage

```python
customer, errors = validate_form({
    "cpf": "111.111.111-11",          # invalid CPF
    "email": "  ANA@EXAMPLE.COM  ",   # auto-normalized
    "birth_date": "2010-01-01",       # too young
    "start_date": "2026-06-01",
    "end_date": "2026-05-01",         # end before start
    "discount_pct": 0.15,
})

if errors:
    for e in errors:
        print(e)
# cpf: CPF inválido: '111.111.111-11'
# birth_date: Cliente deve ter 18+, calculado: 16 anos
# : end_date (2026-05-01) deve ser posterior a start_date (2026-06-01)
```

## See Also

- [Validators](../concepts/validators.md)
- [Field API](../concepts/field-api.md)
- [BaseModel](../concepts/base-model.md)
