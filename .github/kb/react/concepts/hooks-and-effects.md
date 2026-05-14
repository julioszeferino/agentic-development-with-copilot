# Hooks and Effects

> **Purpose**: Use hooks correctly and synchronize external systems safely with useEffect.
> **Confidence**: 0.99
> **MCP Validated**: 2026-05-02

## Overview

Hooks must be called at the top level of function components or custom hooks. useEffect is specifically for synchronizing with external systems such as subscriptions, browser APIs, and network clients. Dependency arrays must include all reactive values referenced inside the effect logic.

## The Pattern

```jsx
import { useEffect, useState } from "react";

function OnlineStatus() {
  const [online, setOnline] = useState(navigator.onLine);

  useEffect(() => {
    function handleOnline() {
      setOnline(true);
    }

    function handleOffline() {
      setOnline(false);
    }

    window.addEventListener("online", handleOnline);
    window.addEventListener("offline", handleOffline);

    return () => {
      window.removeEventListener("online", handleOnline);
      window.removeEventListener("offline", handleOffline);
    };
  }, []);

  return <p>{online ? "Online" : "Offline"}</p>;
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| Hook at top level | Valid behavior | Required by Rules of Hooks |
| useEffect with cleanup | Safe subscription lifecycle | Prevents leaks and stale listeners |
| Missing dependency | Stale closure risk | Respect exhaustive-deps lint |

## Common Mistakes

### Wrong

```jsx
if (enabled) {
  useEffect(() => {
    connect(roomId);
  }, [roomId]);
}
```

### Correct

```jsx
useEffect(() => {
  if (!enabled) return;
  const conn = connect(roomId);
  return () => conn.disconnect();
}, [enabled, roomId]);
```

## Related

- [State and Rendering](../concepts/state-and-rendering.md)
- [Custom Hook Extraction](../patterns/custom-hook-extraction.md)
