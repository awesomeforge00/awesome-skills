# Testing (pytest)

Refactoring without tests isn't refactoring — it's editing and hoping. Add tests before you
restructure code that doesn't have any.

## 1. Basic test

```python
# order_service.py
def calculate_discount(total: float, is_member: bool) -> float:
    return total * 0.2 if is_member else 0.0

# test_order_service.py
from order_service import calculate_discount


def test_member_gets_20_percent_discount():
    assert calculate_discount(100, is_member=True) == 20.0


def test_non_member_gets_no_discount():
    assert calculate_discount(100, is_member=False) == 0.0
```

## 2. Fixtures (shared setup)

```python
# BAD: repeating setup in every test
def test_order_total():
    order = Order(items=[Item("book", 10.0)])
    assert order.total == 10.0


def test_order_discount():
    order = Order(items=[Item("book", 10.0)])
    assert order.discount == 0.0

# GOOD: a fixture builds it once per test
import pytest


@pytest.fixture
def order() -> Order:
    return Order(items=[Item("book", 10.0)])


def test_order_total(order: Order):
    assert order.total == 10.0


def test_order_discount(order: Order):
    assert order.discount == 0.0
```

## 3. Parametrized tests (replace copy-pasted test cases)

```python
import pytest


@pytest.mark.parametrize(
    "status, expected",
    [
        ("active", True),
        ("inactive", False),
        ("suspended", False),
    ],
)
def test_is_active(status: str, expected: bool):
    assert User(status=status).is_active() is expected
```

## 4. Mocking external dependencies

```python
# BAD: test hits a real network/database
def test_send_welcome_email():
    email_service.send_welcome(user)  # actually sends an email!

# GOOD: replace the dependency with a stand-in
from unittest.mock import MagicMock


def test_send_welcome_email():
    email_client = MagicMock()
    service = EmailService(client=email_client)

    service.send_welcome(user)

    email_client.send.assert_called_once_with(user.email, "Welcome!")
```

Mock things that are slow, external, or have side effects (network, filesystem, email, payment
gateways). Don't mock the code you're actually testing.

## 5. Refactor-safe testing workflow

1. **Before refactoring**: confirm tests exist and pass. If a test is missing for the part
   you're about to change, write it first — it should pass against the *current* code.
2. **During refactoring**: after each small change, re-run the tests.
3. **After refactoring**: run the full suite once more; behavior should be unchanged and all
   tests should still pass with no edits to the test file itself (unless the public API changed
   on purpose).
