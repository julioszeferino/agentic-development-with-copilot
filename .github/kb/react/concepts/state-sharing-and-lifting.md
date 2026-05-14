# State Sharing and Lifting

> **Purpose**: Coordinate sibling components by moving shared state to the nearest common parent.
> **Confidence**: 0.98
> **MCP Validated**: 2026-05-02

## Overview

When two components need synchronized values, duplicate local state quickly drifts and creates bugs. Lifting state up to the common parent creates a single source of truth and turns children into controlled views via props and callbacks.

## The Pattern

```jsx
import { useState } from "react";

function TemperatureInput({ label, value, onChange }) {
  return (
    <label>
      {label}
      <input
        value={value}
        onChange={(e) => onChange(e.target.value)}
        type="number"
      />
    </label>
  );
}

export default function TemperatureCalculator() {
  const [celsius, setCelsius] = useState("0");
  const fahrenheit = Number(celsius) * (9 / 5) + 32;

  return (
    <section>
      <TemperatureInput label="Celsius" value={celsius} onChange={setCelsius} />
      <p>Fahrenheit: {Number.isFinite(fahrenheit) ? fahrenheit.toFixed(1) : "-"}</p>
    </section>
  );
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| Local duplicate state across siblings | Divergence risk | Avoid sync-by-effects |
| Lifted parent state | Consistent UI | Preferred for coordinated controls |
| Child emits callback | Parent updates source of truth | Standard controlled-component flow |

## Common Mistakes

### Wrong

```jsx
// parent and child both store same activeTab
```

### Correct

```jsx
// parent stores activeTab and passes value+setter to children
```

## Related

- [State and Rendering](../concepts/state-and-rendering.md)
- [Custom Hook Extraction](../patterns/custom-hook-extraction.md)
