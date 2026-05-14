# React Quick Reference

> Fast lookup tables. For implementation examples, see linked files.

## Rules of Hooks

| Rule | Do | Do Not |
|------|----|--------|
| Hook call location | Call at top level | Call inside loops/conditions/events |
| Hook scope | Function component or custom hook | Class components or random helpers |
| Dependency arrays | List all reactive dependencies | Omit referenced props/state/functions |

## useEffect Dependency Behavior

| Dependency Form | Behavior | Typical Use |
|-----------------|----------|-------------|
| Omitted deps | Runs after every render | Rare; debugging or global sync edge cases |
| `[]` | Runs once after mount | Setup/teardown with no reactive values |
| `[a, b]` | Runs when any dependency changes | External synchronization with specific values |

## Lists and Keys

| Pattern | Recommendation | Why |
|---------|----------------|-----|
| Key from persistent id | Yes | Preserves component identity across reorder |
| Array index as key | Avoid for mutable lists | Causes state mismatch on insert/remove/reorder |
| Random key per render | Never | Forces remount and loses local state |

## State Placement Matrix

| Scenario | Place State |
|----------|-------------|
| Single component concern | Local component state |
| Two siblings need same value | Lift state to closest common parent |
| Many descendants read/write | Context + reducer/custom hook |

## Memoization Decision Matrix

| Situation | Tool |
|-----------|------|
| Expensive derived value | useMemo |
| Stable callback prop for memoized child | useCallback |
| Child rerenders with same props | memo |
| No measured bottleneck | No memoization |

## Common Pitfalls

| Do Not | Do Instead |
|--------|------------|
| Use effect to derive pure render data | Compute directly in render or useMemo |
| Duplicate same state in parent and child | Lift state up |
| Add key using Math.random() | Use stable data id |
| Ignore exhaustive-deps lints | Restructure logic or include dependencies |

## Related Documentation

| Topic | Path |
|-------|------|
| State and rendering | `concepts/state-and-rendering.md` |
| Hooks and effects | `concepts/hooks-and-effects.md` |
| Lists and identity | `concepts/lists-keys-and-identity.md` |
| Shared state | `concepts/state-sharing-and-lifting.md` |
| Memoization pattern | `patterns/targeted-memoization.md` |
| Full Index | `index.md` |
