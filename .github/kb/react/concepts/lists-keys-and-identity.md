# Lists, Keys, and Identity

> **Purpose**: Preserve stable UI identity in React lists and prevent state corruption during reorder operations.
> **Confidence**: 0.99
> **MCP Validated**: 2026-05-02

## Overview

React uses keys to match sibling elements between renders. Stable keys let React preserve component identity, state, and input values across insertions, deletions, and sorting. Keys should be unique among siblings and derived from stable data IDs.

## The Pattern

```jsx
function TodoList({ todos }) {
  const openTodos = todos.filter((t) => !t.done);

  return (
    <ul>
      {openTodos.map((todo) => (
        <li key={todo.id}>
          <label>
            <input type="checkbox" checked={todo.done} readOnly />
            {todo.title}
          </label>
        </li>
      ))}
    </ul>
  );
}
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| Stable id key | Correct diffing | Preserves state per logical item |
| Index key on mutable list | Misaligned state | Breaks on reorder/insert/delete |
| Random key | Forced remounts | Loses input focus and local state |

## Common Mistakes

### Wrong

```jsx
{items.map((item, index) => (
  <Row key={index} item={item} />
))}
```

### Correct

```jsx
{items.map((item) => (
  <Row key={item.id} item={item} />
))}
```

## Related

- [State Sharing and Lifting](../concepts/state-sharing-and-lifting.md)
- [Targeted Memoization](../patterns/targeted-memoization.md)
