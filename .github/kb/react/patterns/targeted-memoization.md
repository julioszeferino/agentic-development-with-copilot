# Targeted Memoization

> **Purpose**: Improve React rendering performance by memoizing expensive values and callback props only where measured.
> **MCP Validated**: 2026-05-02

## When to Use

- Profiling shows expensive recomputation or avoidable rerenders
- Memoized children receive unstable callback or object props
- Filtering/sorting derived collections on frequent parent updates

## Implementation

```jsx
import { memo, useCallback, useMemo, useState } from "react";

const ProductList = memo(function ProductList({ items, onBuy }) {
  return (
    <ul>
      {items.map((p) => (
        <li key={p.id}>
          {p.name} - ${p.price}
          <button onClick={() => onBuy(p.id)}>Buy</button>
        </li>
      ))}
    </ul>
  );
});

export default function Catalog({ products }) {
  const [query, setQuery] = useState("");

  const visible = useMemo(() => {
    const q = query.trim().toLowerCase();
    return q ? products.filter((p) => p.name.toLowerCase().includes(q)) : products;
  }, [products, query]);

  const handleBuy = useCallback((id) => {
    console.log("purchase", id);
  }, []);

  return (
    <section>
      <input value={query} onChange={(e) => setQuery(e.target.value)} />
      <ProductList items={visible} onBuy={handleBuy} />
    </section>
  );
}
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| useMemo dependencies | all reactive inputs | Keeps memoized value correct |
| useCallback dependencies | all referenced reactive values | Ensures stable callback identity |
| memo usage | selective | Apply where rerender cost is meaningful |

## Example Usage

```jsx
// Start without memoization, profile first, then apply targeted memo hooks.
```

## See Also

- [Hooks and Effects](../concepts/hooks-and-effects.md)
- [Lists, Keys, and Identity](../concepts/lists-keys-and-identity.md)
