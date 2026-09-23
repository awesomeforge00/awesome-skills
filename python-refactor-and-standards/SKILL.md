---
name: python-refactor-and-standards
description: 'Use this skill whenever the task involves Python code — writing, reviewing, or refactoring it. Covers code smells, design patterns, exception handling, type hints, dataclasses, generators, testing, docstrings, logging, linting/formatting/type-checking tools, and concurrency — all with Python examples.'
---

# Python Refactor & Standards

## What this skill does

Helps you write and refactor Python code that is clean, safe, well-typed, and testable —
without changing what the code does, only how it's written.

This is a Python-only skill.

## How to use this file

This file is intentionally short. It only tells you **what exists** and **when to open it**.
Do not guess at content — open the specific reference file(s) below that match the task,
instead of loading everything at once.

## Reference Files

| File | Open this when... |
| --- | --- |
| [reference/code-smells.md](reference/code-smells.md) | Code is hard to read/maintain: long functions, god classes, feature envy, primitive obsession, magic values, reaching into other objects' internals |
| [reference/design-patterns.md](reference/design-patterns.md) | Long if/elif chains or nested conditionals should become polymorphism, a Strategy, or a Chain of Responsibility |
| [reference/exceptions-and-safety.md](reference/exceptions-and-safety.md) | Handling errors, building SQL queries, generating secrets/tokens, or accepting/deserializing untrusted input |
| [reference/typing-and-structure.md](reference/typing-and-structure.md) | Adding type hints, replacing boilerplate classes with `dataclass`, choosing between list comprehensions and generators, fixing mutable default arguments, naming things |
| [reference/testing.md](reference/testing.md) | Before or after refactoring: writing/checking pytest tests, fixtures, mocking |
| [reference/docstrings-and-logging.md](reference/docstrings-and-logging.md) | Writing docstrings or adding/reviewing log statements |
| [reference/tooling-and-workflow.md](reference/tooling-and-workflow.md) | Setting up or asking about linting, formatting, type-checking, or environment/dependency tools |
| [reference/concurrency-and-async.md](reference/concurrency-and-async.md) | Code uses (or should use) `threading` or `asyncio`/`async def` |
| [reference/checklist.md](reference/checklist.md) | Doing a final review pass after a refactor is "done" |
| [reference/change-log.md](reference/change-log.md) | Documenting what changed after a refactor, in a clear tabular format for reviewers |

## Golden Rules (always apply, no need to open a reference file for these)

1. **Behavior is preserved** — refactoring changes *how* code is written, never *what* it does.
2. **Small steps** — change one thing, re-run tests, then move on.
3. **Tests first** — if there are no tests for the code you're touching, add them before refactoring.
4. **One concern at a time** — don't mix a refactor with a feature change in the same edit.
5. **Don't refactor without a reason** — working code that won't change again is not a target.
