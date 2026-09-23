# Python Refactor & Standards

Skill for writing and refactoring Python code to be clean, safe, well-typed, and testable — without changing behavior.

## Structure

- [SKILL.md](SKILL.md) — entry point; summarizes when to open each reference file.
- `reference/` — topic-specific guidance, loaded on demand:
  - `code-smells.md` — long functions, god classes, feature envy, primitive obsession, magic values
  - `design-patterns.md` — replacing conditional chains with polymorphism/Strategy/Chain of Responsibility
  - `exceptions-and-safety.md` — error handling, SQL building, secrets/tokens, untrusted input
  - `typing-and-structure.md` — type hints, dataclasses, comprehensions vs. generators, mutable defaults, naming
  - `testing.md` — pytest tests, fixtures, mocking
  - `docstrings-and-logging.md` — docstrings and log statements
  - `tooling-and-workflow.md` — linting, formatting, type-checking, environment/dependency tools
  - `concurrency-and-async.md` — `threading` and `asyncio`/`async def`
  - `checklist.md` — final review pass after a refactor
  - `change-log.md` — tabular summary of changes for reviewers

## Golden Rules

1. Behavior is preserved — refactoring changes *how* code is written, never *what* it does.
2. Small steps — change one thing, re-run tests, then move on.
3. Tests first — if there are no tests for the code you're touching, add them before refactoring.
4. One concern at a time — don't mix a refactor with a feature change in the same edit.
5. Don't refactor without a reason — working code that won't change again is not a target.
