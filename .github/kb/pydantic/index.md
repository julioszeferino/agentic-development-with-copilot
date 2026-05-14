# Pydantic Knowledge Base

> **Purpose**: Data validation, serialization, and settings management using Python type annotations
> **MCP Validated**: 2026-05-02

## Quick Navigation

### Concepts (< 150 lines each)

| File | Purpose |
|------|---------|
| [concepts/base-model.md](concepts/base-model.md) | BaseModel declaration, field defaults, type coercion |
| [concepts/field-api.md](concepts/field-api.md) | Field() constraints, aliases, computed fields |
| [concepts/validators.md](concepts/validators.md) | @field_validator and @model_validator (v2 syntax) |
| [concepts/settings.md](concepts/settings.md) | BaseSettings, env vars, .env files, priority chain |

### Patterns (< 200 lines each)

| File | Purpose |
|------|---------|
| [patterns/llm-output-parsing.md](patterns/llm-output-parsing.md) | Structured LLM output validation with retry |
| [patterns/nested-models.md](patterns/nested-models.md) | Nested/recursive model composition and serialization |
| [patterns/custom-validators.md](patterns/custom-validators.md) | Field and model validators with error handling |

---

## Quick Reference

- [quick-reference.md](quick-reference.md) — Fast lookup tables for types, Field() options, ConfigDict

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| **BaseModel** | Base class for all Pydantic models; validates on instantiation |
| **Field()** | Declares constraints, defaults, aliases, and metadata on fields |
| **@field_validator** | Per-field validation hook (v2); must be `@classmethod` |
| **@model_validator** | Cross-field or whole-model validation hook (v2) |
| **ConfigDict** | Model-level configuration replacing inner `class Config` |
| **BaseSettings** | Reads values from env vars and .env files; extends BaseModel |
| **model_dump()** | Serializes model to dict; supports `exclude_none`, `by_alias` |

---

## Learning Path

| Level | Files |
|-------|-------|
| **Beginner** | concepts/base-model.md → quick-reference.md |
| **Intermediate** | concepts/field-api.md → concepts/validators.md |
| **Advanced** | patterns/llm-output-parsing.md → patterns/nested-models.md |

---

## Agent Usage

| Agent | Primary Files | Use Case |
|-------|---------------|----------|
| shopagent-builder | concepts/base-model.md, patterns/llm-output-parsing.md | Validate LLM structured outputs |
| python-developer | concepts/field-api.md, concepts/validators.md | Production model design |
| ai-data-engineer | concepts/settings.md | Env-driven config management |
