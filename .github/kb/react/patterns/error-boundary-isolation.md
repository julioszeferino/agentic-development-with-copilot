# Error Boundary Isolation

> **Purpose**: Keep React apps resilient by isolating failing subtrees behind fallback UI.
> **MCP Validated**: 2026-05-02

## When to Use

- Third-party widgets may throw during render
- Route-level sections should fail independently
- Data-driven panels require graceful fallback and retry affordances

## Implementation

```jsx
import { Component } from "react";

class ErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { error: null };
  }

  static getDerivedStateFromError(error) {
    return { error };
  }

  componentDidCatch(error, info) {
    console.error("boundary caught", error, info.componentStack);
  }

  reset = () => this.setState({ error: null });

  render() {
    if (this.state.error) {
      return (
        <div>
          <h3>Something went wrong</h3>
          <p>{this.state.error.message}</p>
          <button onClick={this.reset}>Try again</button>
        </div>
      );
    }

    return this.props.children;
  }
}

export function RiskyArea() {
  return (
    <ErrorBoundary>
      <WidgetThatMayThrow />
    </ErrorBoundary>
  );
}
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| Boundary granularity | subtree | Smaller boundaries preserve more app interactivity |
| Fallback message | generic | Prefer user-actionable guidance |
| Error reporting | console | Integrate telemetry in componentDidCatch |

## Example Usage

```jsx
<ErrorBoundary>
  <Suspense fallback={<p>Loading...</p>}>
    <AnalyticsPanel />
  </Suspense>
</ErrorBoundary>
```

## See Also

- [Hooks and Effects](../concepts/hooks-and-effects.md)
- [Targeted Memoization](../patterns/targeted-memoization.md)
