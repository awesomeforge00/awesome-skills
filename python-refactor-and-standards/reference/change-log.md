# Documenting Refactor Changes

After a refactor is complete, summarize what changed in a table so a reviewer can
scan it without diffing every file. Keep entries short — one row per distinct change.

## Format

| File | Location | Change Type | Before → After | Reason |
| --- | --- | --- | --- | --- |
| `orders.py` | `OrderProcessor.process` | Extract Method | Inline 40-line function → `process` + `_validate_order` + `_apply_discount` | Long function, mixed concerns |
| `orders.py` | `Order` class | Introduce Dataclass | Plain class with `__init__` boilerplate → `@dataclass` | Reduce boilerplate, add equality |
| `utils.py` | `parse_config` | Add Type Hints | Untyped params/return → `dict[str, str] -> Config` | Type safety |
| `db.py` | `get_user` | Fix SQL Injection | String-formatted query → parameterized query | Security (OWASP A03) |

## Change Type vocabulary

Use consistent labels so tables stay scannable:

- **Extract Method** / **Extract Class** — split out a piece of logic
- **Inline** — removed an unnecessary indirection
- **Rename** — improved a name for clarity
- **Introduce Dataclass** — replaced boilerplate with `dataclass`
- **Replace Conditional with Polymorphism/Strategy** — from `design-patterns.md`
- **Add Type Hints** — from `typing-and-structure.md`
- **Fix Exception Handling** / **Fix SQL Injection** / **Fix Untrusted Input** — from `exceptions-and-safety.md`
- **Add/Fix Docstring** / **Fix Logging** — from `docstrings-and-logging.md`
- **Add Tests** — from `testing.md`
- **Concurrency Fix** — from `concurrency-and-async.md`

## Rules

1. One row per meaningful change — don't log every whitespace/formatting fix.
2. "Before → After" should be a short description, not a full code snippet.
3. Group multiple small changes to the same function into one row if they share a reason.
4. Always fill in "Reason" — a change without a reason is a red flag per the Golden Rules.
