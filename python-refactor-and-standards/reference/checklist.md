# Final Review Checklist

Use this after a refactor is functionally "done", before calling it finished.

## Code quality

- [ ] Functions are small and do one thing
- [ ] Names are descriptive (no `data`, `tmp`, `x` for anything non-trivial)
- [ ] No magic numbers/strings — named constants or enums instead
- [ ] No commented-out code left behind

## Structure

- [ ] Related code lives together (module/class boundaries make sense)
- [ ] No circular imports
- [ ] Each class has a single, clear responsibility

## Type safety

- [ ] Public functions/methods have type hints
- [ ] No unexplained `Any` types
- [ ] Optional values are typed as `X | None`, not silently assumed

## Safety

- [ ] No bare `except:`
- [ ] No string-formatted SQL
- [ ] No `pickle`/`eval` on untrusted input
- [ ] Secrets/tokens generated with `secrets`, not `random`

## Testing

- [ ] Tests existed before the refactor started (or were added first)
- [ ] All tests still pass
- [ ] Edge cases are covered (empty input, None, boundary values)

## Tooling

- [ ] `ruff check` / `ruff format` pass cleanly
- [ ] `mypy` (or your project's type checker) passes
- [ ] Pre-commit hooks pass

## Documentation

- [ ] Public functions have docstrings that explain *why*, not just *what*
- [ ] Log messages use parameterized logging, not f-strings
