# Typing & Structure (Python)

## 1. Mutable default arguments

```python
# BAD: the same list is reused across every call
def add_tag(tag: str, tags: list[str] = []):
    tags.append(tag)
    return tags

# GOOD: default to None, create a fresh list inside
def add_tag(tag: str, tags: list[str] | None = None) -> list[str]:
    if tags is None:
        tags = []
    tags.append(tag)
    return tags
```

## 2. Type hints on public APIs

```python
# BAD: no hints, caller has to guess what's valid
def calculate_discount(user, total, date=None):
    ...

# GOOD: hints document the contract and let type checkers catch mistakes
from datetime import date as Date


def calculate_discount(
    user: User,
    total: float,
    on_date: Date | None = None,
) -> float:
    ...
```

Hint at least every public function's parameters and return type. Internal helpers can skip
hints if the types are obvious from context, but prefer hinting those too.

## 3. Dataclasses instead of boilerplate classes

```python
# BAD: manual __init__, __eq__, __repr__
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

    def __repr__(self):
        return f"Point({self.x}, {self.y})"

# GOOD: dataclass generates all of that
from dataclasses import dataclass


@dataclass
class Point:
    x: float
    y: float
```

Use `@dataclass(frozen=True)` when the object should be immutable (e.g. value objects like
`Email` or `Money`).

## 4. Generators vs. comprehensions

```python
# List comprehension: builds the whole list in memory, use when you need it all at once
squares = [n * n for n in range(1_000_000)]

# Generator expression: produces values lazily, use for large/streaming data
squares_gen = (n * n for n in range(1_000_000))
total = sum(squares_gen)  # never holds all million values in memory at once
```

Rule of thumb: if you're going to iterate once and don't need indexing or `len()`,
prefer a generator expression to save memory.

## 5. `is`/`is not` for None, `isinstance` for types

```python
# BAD
if x == None:
    ...
if type(x) == list:
    ...

# GOOD
if x is None:
    ...
if isinstance(x, list):
    ...
```

`isinstance` also correctly handles subclasses, `type(x) ==` does not.

## 6. Naming conventions

| Kind | Convention | Example |
| --- | --- | --- |
| Module | `snake_case` | `order_service.py` |
| Class | `PascalCase` | `OrderService` |
| Function/method/variable | `snake_case` | `calculate_discount` |
| Constant | `UPPER_SNAKE_CASE` | `MAX_RETRIES` |
| Private/internal | leading underscore | `_fetch_valid_order` |

Keep names consistent across a module — don't mix `get_user` in one file with `fetch_user` in another for the same concept.
