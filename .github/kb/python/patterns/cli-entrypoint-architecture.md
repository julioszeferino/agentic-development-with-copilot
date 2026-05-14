# Pattern: CLI Entrypoint Architecture

> **Purpose**: Build predictable command-line tools with clear argument parsing and exit behavior.

## Problem

Many scripts evolve into CLIs without structure, leading to hard-to-test argument handling and inconsistent error semantics.

## Solution

Separate argument parsing, command execution, and process exit code mapping. Keep `main()` thin and deterministic.

## Implementation

```python
import argparse
import logging

logger = logging.getLogger(__name__)


def parse_args(argv: list[str] | None = None) -> argparse.Namespace:
    parser = argparse.ArgumentParser(prog="sync-tool")
    parser.add_argument("--dry-run", action="store_true")
    return parser.parse_args(argv)


def run(dry_run: bool) -> None:
    if dry_run:
        logger.info("dry run mode")


def main(argv: list[str] | None = None) -> int:
    try:
        args = parse_args(argv)
        run(args.dry_run)
        return 0
    except ValueError:
        logger.exception("invalid input")
        return 2
    except Exception:
        logger.exception("unexpected failure")
        return 1
```

## Why It Works

- Improves testability of argument and execution branches
- Produces stable operational behavior via explicit exit codes
- Preserves useful diagnostics through boundary logging

## Trade-offs

- Adds structure that can feel heavy for one-off scripts
- Requires discipline to keep execution side effects out of parsing

## Related

- [Exceptions and Error Boundaries](../concepts/exceptions-and-error-boundaries.md)
