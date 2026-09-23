# Design Patterns for Refactoring (Python)

Use these when conditional logic keeps growing and each new case means editing the same function again.

## Strategy Pattern (replace conditional with polymorphism)

```python
# BAD: every new shipping method means editing this function
def calculate_shipping(order: Order, method: str) -> float:
    if method == "standard":
        return 0.0 if order.total > 50 else 5.99
    if method == "express":
        return 9.99 if order.total > 100 else 14.99
    if method == "overnight":
        return 29.99
    raise ValueError(f"Unknown method: {method}")

# GOOD: each strategy is its own object; adding one doesn't touch the others
from typing import Protocol


class ShippingStrategy(Protocol):
    def calculate(self, order: Order) -> float: ...


class StandardShipping:
    def calculate(self, order: Order) -> float:
        return 0.0 if order.total > 50 else 5.99


class ExpressShipping:
    def calculate(self, order: Order) -> float:
        return 9.99 if order.total > 100 else 14.99


class OvernightShipping:
    def calculate(self, order: Order) -> float:
        return 29.99


def calculate_shipping(order: Order, strategy: ShippingStrategy) -> float:
    return strategy.calculate(order)
```

`Protocol` is preferred here over an abstract base class when you only need structural typing
(no shared implementation) — any class with a matching `calculate` method satisfies it.

## Chain of Responsibility (replace nested validation)

```python
# BAD: one function that must know every rule
def validate(user: User) -> list[str]:
    errors = []
    if not user.email:
        errors.append("Email required")
    elif not is_valid_email(user.email):
        errors.append("Invalid email")
    if not user.name:
        errors.append("Name required")
    if user.age < 18:
        errors.append("Must be 18+")
    return errors

# GOOD: each rule is independent and composable
from abc import ABC, abstractmethod


class Validator(ABC):
    def __init__(self) -> None:
        self._next: Validator | None = None

    def set_next(self, validator: "Validator") -> "Validator":
        self._next = validator
        return validator

    def validate(self, user: User) -> str | None:
        error = self._check(user)
        if error:
            return error
        return self._next.validate(user) if self._next else None

    @abstractmethod
    def _check(self, user: User) -> str | None: ...


class EmailRequired(Validator):
    def _check(self, user: User) -> str | None:
        return None if user.email else "Email required"


class EmailFormat(Validator):
    def _check(self, user: User) -> str | None:
        if user.email and not is_valid_email(user.email):
            return "Invalid email"
        return None


class AgeValidator(Validator):
    def _check(self, user: User) -> str | None:
        return None if user.age >= 18 else "Must be 18+"


validator = EmailRequired()
validator.set_next(EmailFormat()).set_next(AgeValidator())
```

## Guard Clauses (replace nested conditionals)

```python
# BAD: deep nesting
def handle(order: Order | None) -> None:
    if order:
        if order.user:
            if order.user.is_active:
                if order.items:
                    for item in order.items:
                        if item.in_stock:
                            ship(item)

# GOOD: exit early, keep the main logic flat
def handle(order: Order | None) -> None:
    if not order or not order.user or not order.user.is_active:
        return
    _ship_available_items(order.items)


def _ship_available_items(items: list[Item]) -> None:
    for item in items:
        if item.in_stock:
            ship(item)
```

## When to reach for a pattern vs. a guard clause

- **2-3 branches, unlikely to grow** → guard clauses / plain `if` are enough, don't over-engineer.
- **Branches represent interchangeable behaviors that may grow over time** → Strategy.
- **Branches represent a sequence of independent checks** → Chain of Responsibility.
