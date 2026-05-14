# LLM Output Parsing

> **Purpose**: Validate and parse structured JSON output from LLMs with retry on validation failure
> **MCP Validated**: 2026-05-02

## When to Use

- Parsing structured JSON returned by Claude, GPT, or Gemini
- Enforcing schema on LLM output before storing or processing
- Retrying when LLM produces malformed JSON or missing fields
- Extracting typed data from unstructured LLM responses

## Implementation

```python
import json
import re
from typing import Any

from anthropic import Anthropic
from pydantic import BaseModel, Field, ValidationError


class ProductExtraction(BaseModel):
    name: str = Field(min_length=1, description="Product name")
    price: float = Field(gt=0, description="Price in BRL")
    category: str
    in_stock: bool
    tags: list[str] = Field(default_factory=list)


SYSTEM_PROMPT = """Extract product information from the text.
Return ONLY valid JSON matching this schema:
{
  "name": string,
  "price": number (positive),
  "category": string,
  "in_stock": boolean,
  "tags": [string]
}"""


def extract_json(text: str) -> dict[str, Any]:
    """Extract JSON from LLM response, handling markdown fences."""
    # Strip ```json ... ``` if present
    match = re.search(r"```(?:json)?\s*([\s\S]*?)```", text)
    raw = match.group(1) if match else text
    return json.loads(raw.strip())


def parse_product(text: str, max_retries: int = 2) -> ProductExtraction:
    client = Anthropic()
    last_error: str = ""

    for attempt in range(max_retries + 1):
        user_content = text
        if attempt > 0:
            user_content = (
                f"{text}\n\n"
                f"Previous attempt failed: {last_error}\n"
                "Fix the JSON and try again."
            )

        response = client.messages.create(
            model="claude-opus-4-5",
            max_tokens=512,
            system=SYSTEM_PROMPT,
            messages=[{"role": "user", "content": user_content}],
        )
        raw_text = response.content[0].text

        try:
            data = extract_json(raw_text)
            return ProductExtraction.model_validate(data)
        except (json.JSONDecodeError, ValidationError) as exc:
            last_error = str(exc)
            if attempt == max_retries:
                raise ValueError(
                    f"LLM output failed validation after {max_retries + 1} attempts: {last_error}"
                ) from exc

    raise RuntimeError("Unreachable")
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| `max_retries` | `2` | Retry attempts on parse failure |
| `model` | `claude-opus-4-5` | LLM model to use |
| `max_tokens` | `512` | Enough for structured JSON |
| `mode="json"` on `model_dump()` | — | JSON-safe output types |

## Example Usage

```python
text = "Tênis Nike Air Max, R$459,90, categoria calçados, disponível"

product = parse_product(text)
print(product.name)      # "Tênis Nike Air Max"
print(product.price)     # 459.9
print(product.in_stock)  # True

# Serialize for storage
payload = product.model_dump(mode="json")
```

## See Also

- [BaseModel](../concepts/base-model.md)
- [Validators](../concepts/validators.md)
- [Custom Validators Pattern](../patterns/custom-validators.md)
