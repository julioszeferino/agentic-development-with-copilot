# React Knowledge Base

> **Purpose**: Build predictable, maintainable React UIs using official component, state, and hook patterns.
> **MCP Validated**: 2026-05-02

## Quick Navigation

### Concepts (< 150 lines each)

| File | Purpose |
|------|---------|
| [concepts/state-and-rendering.md](concepts/state-and-rendering.md) | Component state model, rendering flow, and derived UI data |
| [concepts/hooks-and-effects.md](concepts/hooks-and-effects.md) | Rules of Hooks and correct useEffect dependency semantics |
| [concepts/lists-keys-and-identity.md](concepts/lists-keys-and-identity.md) | Stable list identity with keys and state preservation rules |
| [concepts/state-sharing-and-lifting.md](concepts/state-sharing-and-lifting.md) | Lift state to synchronize related components |

### Patterns (< 200 lines each)

| File | Purpose |
|------|---------|
| [patterns/custom-hook-extraction.md](patterns/custom-hook-extraction.md) | Extract reusable side-effect logic into custom hooks |
| [patterns/targeted-memoization.md](patterns/targeted-memoization.md) | Apply useMemo, useCallback, and memo only where measured |
| [patterns/error-boundary-isolation.md](patterns/error-boundary-isolation.md) | Isolate risky subtrees with resilient fallback UX |

---

## Quick Reference

- [quick-reference.md](quick-reference.md) - Fast lookup for hook rules, keys, effects, and memoization decisions

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Top-Level Hooks** | Hooks run only at component/custom-hook top level, never conditionally |
| **Effect Dependencies** | Every reactive value referenced in an effect must be declared |
| **Stable Keys** | Keys identify sibling elements across reorders and updates |
| **Lifted State** | Shared parent state keeps siblings synchronized without duplicated state |

---

## Learning Path

| Level | Files |
|-------|-------|
| **Beginner** | concepts/state-and-rendering.md -> concepts/lists-keys-and-identity.md |
| **Intermediate** | concepts/hooks-and-effects.md -> concepts/state-sharing-and-lifting.md |
| **Advanced** | patterns/custom-hook-extraction.md -> patterns/targeted-memoization.md |

---

## Agent Usage

| Agent | Primary Files | Use Case |
|-------|---------------|----------|
| code-reviewer | concepts/hooks-and-effects.md, patterns/targeted-memoization.md | React correctness and performance review |
| kb-architect | index.md, quick-reference.md | Expand React KB with router or server-component tracks |
| the-planner | concepts/state-sharing-and-lifting.md | UI architecture and component state planning |
