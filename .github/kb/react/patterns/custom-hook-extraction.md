# Custom Hook Extraction

> **Purpose**: Encapsulate reusable effect-driven logic into focused, testable custom hooks.
> **MCP Validated**: 2026-05-02

## When to Use

- Multiple components duplicate subscription or fetch logic
- Effect setup and cleanup details pollute UI component code
- You need a consistent contract for shared behavior

## Implementation

```jsx
import { useEffect, useState } from "react";

export function useOnlineStatus() {
  const [online, setOnline] = useState(navigator.onLine);

  useEffect(() => {
    function onOnline() {
      setOnline(true);
    }
    function onOffline() {
      setOnline(false);
    }

    window.addEventListener("online", onOnline);
    window.addEventListener("offline", onOffline);
    return () => {
      window.removeEventListener("online", onOnline);
      window.removeEventListener("offline", onOffline);
    };
  }, []);

  return online;
}

export function StatusBadge() {
  const online = useOnlineStatus();
  return <span>{online ? "Connected" : "Disconnected"}</span>;
}
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| Hook name | starts with use | Required for lint and hook semantics |
| Return value | domain-specific state | Keep API narrow and stable |
| Cleanup strategy | in effect return | Ensure deterministic unsubscription |

## Example Usage

```jsx
function Footer() {
  const online = useOnlineStatus();
  return <small>Network: {online ? "up" : "down"}</small>;
}
```

## See Also

- [Hooks and Effects](../concepts/hooks-and-effects.md)
- [State Sharing and Lifting](../concepts/state-sharing-and-lifting.md)
