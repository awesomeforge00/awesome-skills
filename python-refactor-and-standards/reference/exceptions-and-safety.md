# Exceptions & Safety (Python)

## 1. Exception handling

```python
# BAD: swallows everything, including bugs
try:
    result = process_order(order_id)
except Exception:
    pass

# GOOD: catch what you expect, preserve the cause, let the rest propagate
try:
    result = process_order(order_id)
except OrderNotFoundError as exc:
    logger.warning("Order not found: %s", order_id)
    raise ServiceError("Order processing failed") from exc
```

Never use a bare `except:` — it also catches `KeyboardInterrupt` and `SystemExit`.

## 2. SQL injection safety

```python
# BAD: an attacker can inject SQL through order_id
cursor.execute(f"SELECT * FROM orders WHERE id = {order_id}")

# GOOD: let the driver handle escaping
cursor.execute("SELECT * FROM orders WHERE id = %s", (order_id,))
```

Same idea applies to any query builder (ORM `.raw()`/`.filter()` calls) — never format
user input directly into a query string.

## 3. Secrets and randomness

```python
# BAD: random is predictable, not safe for secrets
import random
token = str(random.randint(100000, 999999))

# GOOD: secrets is designed for security-sensitive values
import secrets
token = secrets.token_urlsafe(32)
```

Use `random`/`numpy.random` for simulations, shuffling test data, etc. Use `secrets` for
anything that must not be guessable: tokens, password reset codes, API keys.

## 4. Untrusted input / unsafe deserialization

```python
# BAD: pickle executes arbitrary code embedded in the data
import pickle
data = pickle.loads(request.body)  # never do this with data from a network request

# GOOD: use a data-only format for anything from outside your process
import json
data = json.loads(request.body)
```

The same caution applies to `eval()`, `exec()`, and `yaml.load()` (use `yaml.safe_load()` instead)
on any input that didn't come from a trusted, internal source.

## 5. Shell commands

```python
# BAD: shell=True + untrusted input allows command injection
import subprocess
subprocess.run(f"ls {user_supplied_path}", shell=True)

# GOOD: pass arguments as a list, no shell involved
subprocess.run(["ls", user_supplied_path])
```

## 6. Input validation at boundaries

Validate data as soon as it enters your system (API request body, file upload, CLI arg) —
not deep inside business logic where it's easy to forget a check.

```python
# GOOD: validate once, at the edge
from dataclasses import dataclass


@dataclass(frozen=True)
class CreateOrderRequest:
    customer_id: str
    quantity: int

    def __post_init__(self) -> None:
        if self.quantity <= 0:
            raise ValueError("quantity must be positive")
```

Everything past this point in the code can trust that `quantity` is valid.
