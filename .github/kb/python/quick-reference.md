# Python Quick Reference

> Fast lookup tables. For code examples, see linked files.

## Environment Workflow

| Task | Command | Notes |
|------|---------|-------|
| Create venv | `python -m venv .venv` | Preferred project-local environment |
| Activate (Linux/macOS) | `source .venv/bin/activate` | Shell prompt changes to active env |
| Install package | `python -m pip install <pkg>` | Uses interpreter-bound pip |
| Freeze lock snapshot | `python -m pip freeze > requirements.txt` | Good for app reproducibility |

## Typing Essentials

| Goal | Syntax | Notes |
|------|--------|-------|
| Optional value | `str | None` | Python 3.10+ union syntax |
| Typed list | `list[int]` | Built-in generics |
| Callable signature | `Callable[[int], str]` | Function contract |
| Metadata annotation | `Annotated[T, ...]` | Tool-friendly metadata extensions |

## pyproject.toml Core Tables

| Table | Required | Purpose |
|-------|----------|---------|
| `[build-system]` | Strongly recommended | Build backend and build requirements |
| `[project]` | Recommended | Standardized metadata and dependencies |
| `[tool.*]` | Optional | Tool-specific configuration (ruff, mypy, pytest) |

## Versioning and Dependencies

| Strategy | Recommendation |
|----------|----------------|
| Runtime dependencies | Use compatible ranges, avoid over-pin for libraries |
| App reproducibility | Pin resolved versions in lock artifacts or requirements snapshot |
| Python floor | Set `requires-python` in `[project]` |
| Release bumping | Follow SemVer for public package APIs |

## Decision Matrix

| Use Case | Choose |
|----------|--------|
| Reusable library distribution | `pyproject.toml` with `[project]` metadata and build backend |
| Internal app deployment | Virtual env + pinned dependency snapshot |
| Strong data contracts | Type hints + runtime validation models |
| Script promoted to tool | CLI entrypoint pattern with explicit exit codes |

## Common Pitfalls

| Do Not | Do Instead |
|--------|------------|
| Install dependencies globally for project work | Use per-project `.venv` |
| Leave `requires-python` undefined | Declare supported Python version floor |
| Duplicate package metadata across many files | Centralize in `pyproject.toml` |
| Swallow exceptions silently | Log context and raise meaningful errors |
| Depend on flat-layout imports implicitly | Use `src/` layout for package projects |

## Related Documentation

| Topic | Path |
|-------|------|
| Environments and dependencies | `concepts/environments-and-dependencies.md` |
| Typing and static safety | `concepts/typing-and-static-safety.md` |
| Exceptions and boundaries | `concepts/exceptions-and-error-boundaries.md` |
| pyproject and build system | `concepts/pyproject-and-build-system.md` |
| src layout pattern | `patterns/src-layout-packaging.md` |
| Full Index | `index.md` |
