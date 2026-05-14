# TypeScript Quick Reference

> Fast lookup tables for compiler, module, and publish decisions.

## tsconfig Strictness Baseline

| Option | Recommended | Why |
|--------|-------------|-----|
| `strict` | `true` | Enables full strict type-checking family |
| `noImplicitAny` | `true` | Prevents untyped inference leaks |
| `noUncheckedIndexedAccess` | `true` | Models possibly missing index access values |
| `noImplicitOverride` | `true` | Enforces explicit override markers |
| `noEmitOnError` | `true` | Blocks artifact emission when types fail |

## Module and Resolution

| Scenario | `module` | `moduleResolution` | Notes |
|----------|----------|--------------------|-------|
| Node-first app/library | `node18` or `nodenext` | `nodenext` | Best model for modern Node semantics |
| Bundler-centered app | `esnext` | `bundler` | Works with Vite/Webpack/Rspack pipelines |
| Legacy CJS output | `commonjs` | `node10`/`node` | Keep only when environment requires it |

## Declaration and Publish

| Goal | Setting / Practice |
|------|--------------------|
| Emit declarations | `declaration: true` |
| Publish maps for better DX | `declarationMap: true` |
| Keep import semantics explicit | `verbatimModuleSyntax: true` |
| Stable package entry points | Define `exports` and `types` in `package.json` |

## Build and References

| Goal | Tooling |
|------|---------|
| Incremental builds | `incremental: true` |
| Multi-project build graph | `references` + `composite: true` |
| Orchestrated build | `tsc --build` |

## Decision Matrix

| Use Case | Choose |
|----------|--------|
| Publish reusable library | Strict config + declarations + explicit exports |
| Large monorepo | Project references and build mode |
| App with modern bundler | `moduleResolution: bundler` and bundler-aligned emit |
| Node runtime package | Node module mode (`node18`/`nodenext`) and tested interop |

## Common Pitfalls

| Do Not | Do Instead |
|--------|------------|
| Mix module settings without runtime validation | Align tsconfig and `package.json` `type`/`exports` |
| Disable strictness broadly to silence errors | Fix types or narrow suppression scope |
| Publish without `.d.ts` validation | Run type-check and declaration emit in CI |
| Assume CJS/ESM dual build is always safe | Prefer single format when possible; document interop clearly |

## Related Documentation

| Topic | Path |
|-------|------|
| TSConfig foundations | `concepts/tsconfig-foundations.md` |
| Module systems and resolution | `concepts/module-systems-and-resolution.md` |
| Declaration files and API surface | `concepts/declaration-files-and-api-surface.md` |
| Project references and build mode | `concepts/project-references-and-build-mode.md` |
| Library publish contract | `patterns/library-publish-contract.md` |
| Full Index | `index.md` |
