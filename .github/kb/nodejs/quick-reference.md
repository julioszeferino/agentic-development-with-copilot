# Node.js Quick Reference

> Fast lookup tables. For implementation examples, see linked files.

## Module System Markers

| Marker | Interpreted As | Notes |
|--------|----------------|-------|
| `.mjs` | ESM | Always ESM |
| `.cjs` | CommonJS | Always CommonJS |
| `.js` + `"type": "module"` | ESM | Package boundary controls behavior |
| `.js` + missing `type` | CommonJS | Default behavior remains CJS |

## Import and Require Interop

| Source | Loader | Key Behavior |
|--------|--------|--------------|
| `import ... from` | ESM loader | Static import in ESM only |
| `import()` | ESM loader | Works in both CJS and ESM |
| `require()` | CJS loader | Primarily CJS; ESM support has constraints |
| ESM importing CJS | Interop namespace | `module.exports` available as default |

## package.json Fields That Matter Most

| Field | Required for Publish | Why It Matters |
|-------|----------------------|----------------|
| `name` | Yes | Package identity |
| `version` | Yes | Semantic release signaling |
| `type` | Strongly recommended | Explicit module behavior for `.js` |
| `exports` | Recommended | Public API contract + encapsulation |
| `main` | Legacy fallback | Older tooling compatibility |
| `engines.node` | Recommended | Runtime compatibility guardrail |
| `private` | Optional | Prevent accidental publish when true |

## SemVer Decision Matrix

| Change Type | Version Bump | Example |
|-------------|--------------|---------|
| Backward-compatible fix | Patch | `1.2.3 -> 1.2.4` |
| Backward-compatible feature | Minor | `1.2.3 -> 1.3.0` |
| Breaking API change | Major | `1.2.3 -> 2.0.0` |

## Dependency Range Guidance

| Range Style | Behavior | Typical Use |
|------------|----------|-------------|
| `^1.2.3` | Allow minor + patch | Default for app dependencies |
| `~1.2.3` | Allow patch only | Sensitive dependencies |
| Exact `1.2.3` | Pin full version | Reproducibility and high-control builds |
| `*` / `latest` | Unbounded drift | Avoid in production |

## Common Pitfalls

| Don't | Do |
|-------|-----|
| Mix ESM/CJS without explicit markers | Declare `type` and use `.mjs`/`.cjs` intentionally |
| Add `exports` without migration review | Treat `exports` as potentially breaking for deep imports |
| Ignore stream errors | Use `pipeline()` and centralized failure handling |
| Publish with accidental files | Control artifacts using package boundaries and prepublish checks |
| Skip engine constraints | Add `engines.node` to communicate supported Node versions |

## Related Documentation

| Topic | Path |
|-------|------|
| Module systems | `concepts/module-systems.md` |
| package.json contract | `concepts/package-json-contract.md` |
| Async error handling | `concepts/async-and-errors.md` |
| Streams and pipeline | `concepts/streams-and-backpressure.md` |
| Publish strategy | `patterns/dual-module-publish-safe.md` |
| Full Index | `index.md` |
