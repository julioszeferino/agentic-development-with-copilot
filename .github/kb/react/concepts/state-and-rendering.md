# State and Rendering

> **Purpose**: Model UI data with React state and derive deterministic renders from it.
> **Confidence**: 0.99
> **MCP Validated**: 2026-05-02

## Overview

React components render from current props and state. State represents component memory across renders and should only hold minimal mutable data. Derived values should be computed from state/props during render rather than duplicated in additional state.

## The Pattern

```jsx
import { useState } from "react";

export default function CounterPanel() {
  const [count, setCount] = useState(0);

  // derived value: do not store this separately in state
  const isEven = count % 2 === 0;

  return (
    <section>
      <h2>Count: {count}</h2>
      <p>{isEven ? "Even" : "Odd"}</p>
      <button onClick={() => setCount((c) => c + 1)}>Increment</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </section>
  );
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `useState(initial)` | state + setter | Hook state persists across rerenders |
| Setter with function form | Uses previous state safely | Prefer for incremental updates |
| Derived value in render | Consistent UI | Avoid extra synchronization effects |

## Common Mistakes

### Wrong

```jsx
const [fullName, setFullName] = useState(first + " " + last);
useEffect(() => setFullName(first + " " + last), [first, last]);
```

### Correct

```jsx
const fullName = first + " " + last;
```

## Related

- [Hooks and Effects](../concepts/hooks-and-effects.md)
- [State Sharing and Lifting](../concepts/state-sharing-and-lifting.md)
