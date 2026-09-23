# Docstrings & Logging (Python)

## 1. Docstrings (Google style)

```python
# BAD: no docstring, or one that just repeats the function name
def calculate_discount(user, total):
    """Calculates discount."""
    ...

# GOOD: explains parameters, return value, and anything non-obvious
def calculate_discount(user: User, total: float) -> float:
    """Calculate the discount amount for an order.

    Args:
        user: The customer placing the order. Membership level and account
            age affect the discount rate.
        total: The order total before discount, must be non-negative.

    Returns:
        The discount amount in the same currency as `total`.

    Raises:
        ValueError: If `total` is negative.
    """
    ...
```

Rules of thumb:
- Document *why* something is done a certain way, not what the code already shows line by line.
- Skip the docstring on tiny, self-explanatory private helpers (e.g. `_is_valid(x)`).
- Keep the first line a one-sentence summary — some tools (IDEs, `help()`) only show that line.

## 2. Logging

```python
# BAD: f-string logging always builds the string, even if the log level is disabled
logger.info(f"Processing order {order_id} for user {user_id}")

# GOOD: parameterized logging — the string is only built if the message is actually logged
logger.info("Processing order %s for user %s", order_id, user_id)
```

```python
# BAD: logging a caught exception without the traceback
try:
    process_order(order_id)
except OrderError as exc:
    logger.error("Order failed: %s", exc)

# GOOD: use logger.exception inside an except block to include the traceback
try:
    process_order(order_id)
except OrderError:
    logger.exception("Order failed for order_id=%s", order_id)
```

## 3. Log levels — when to use which

| Level | Use for |
| --- | --- |
| `DEBUG` | Details only useful while actively debugging |
| `INFO` | Normal events worth recording (order created, job started) |
| `WARNING` | Something unexpected but recoverable |
| `ERROR` | An operation failed, but the app keeps running |
| `CRITICAL` | The app itself can't continue |

Avoid logging secrets, passwords, or full personal data (emails, tokens) at any level.
