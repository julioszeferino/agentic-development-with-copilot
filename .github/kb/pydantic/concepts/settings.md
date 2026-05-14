# Settings

> **Purpose**: Environment-driven configuration management using BaseSettings and .env files
> **Confidence**: 1.00
> **MCP Validated**: 2026-05-02

## Overview

`pydantic-settings` extends `BaseModel` with automatic loading from environment variables and `.env` files. Values are resolved in priority order: explicit kwargs > environment variables > `.env` file > model defaults.

## The Pattern

```python
from functools import lru_cache
from pydantic import Field
from pydantic_settings import BaseSettings, SettingsConfigDict


class Settings(BaseSettings):
    model_config = SettingsConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
        env_prefix="APP_",          # APP_DATABASE_URL, APP_DEBUG, etc.
        extra="ignore",             # ignore unknown env vars
    )

    # Required — must be set in env or .env
    database_url: str
    anthropic_api_key: str = Field(repr=False)  # hidden in repr/logs

    # Optional with defaults
    debug: bool = False
    log_level: str = "INFO"
    max_connections: int = Field(default=10, ge=1, le=100)

    # Derived (not from env)
    @property
    def is_production(self) -> bool:
        return not self.debug


# Singleton — parse env once per process
@lru_cache(maxsize=1)
def get_settings() -> Settings:
    return Settings()


# Usage in application code
settings = get_settings()
print(settings.database_url)
print(settings.is_production)
```

## Quick Reference

| Input | Output | Notes |
|-------|--------|-------|
| `APP_DEBUG=true` env var | `debug=True` | Auto-coerced from string |
| `.env` file key `APP_DATABASE_URL` | `database_url` set | env_prefix stripped |
| Multiple `.env` files | Later file wins | `env_file=(".env", ".env.local")` |
| Missing required field | `ValidationError` at startup | Fail fast, good for prod |

## Common Mistakes

### Wrong

```python
# New Settings() on every call — reads .env file repeatedly
def get_db():
    settings = Settings()   # slow + wasteful
    return create_engine(settings.database_url)
```

### Correct

```python
from functools import lru_cache

@lru_cache(maxsize=1)
def get_settings() -> Settings:
    return Settings()       # cached singleton

def get_db():
    return create_engine(get_settings().database_url)
```

## Related

- [BaseModel](../concepts/base-model.md)
- [Field API](../concepts/field-api.md)
