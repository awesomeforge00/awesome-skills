# Code Smells (Python)

Short, focused fixes for the most common readability problems. Each shows a bad version and a good version.

## 1. Long Function

```python
# BAD: one function doing everything
def process_order(order_id):
    order = db.get_order(order_id)
    if not order or order.total <= 0:
        raise ValueError("invalid order")
    price = order.total * (1 - order.discount)
    db.update_inventory(order.items)
    shipment = shipping.create(order)
    email.send(order.customer_email, "Order shipped", str(shipment))
    return shipment

# GOOD: broken into named steps
def process_order(order_id: str) -> Shipment:
    order = _fetch_valid_order(order_id)
    price = _apply_discount(order)
    inventory.update(order.items)
    shipment = shipping.create(order)
    _notify_customer(order, shipment)
    return shipment


def _fetch_valid_order(order_id: str) -> Order:
    order = db.get_order(order_id)
    if not order or order.total <= 0:
        raise ValueError("invalid order")
    return order


def _apply_discount(order: Order) -> float:
    return order.total * (1 - order.discount)


def _notify_customer(order: Order, shipment: Shipment) -> None:
    email.send(order.customer_email, "Order shipped", str(shipment))
```

Rule of thumb: if you need a comment to mark a "section" of a function, that section is a candidate for its own function.

## 2. Large Class ("God Object")

```python
# BAD: one class doing unrelated jobs
class UserManager:
    def create_user(self, data): ...
    def update_user(self, user_id, data): ...
    def send_welcome_email(self, user): ...
    def generate_activity_report(self, user): ...
    def charge_subscription(self, user, amount): ...

# GOOD: one responsibility per class
class UserService:
    def create(self, data: dict) -> User: ...
    def update(self, user_id: str, data: dict) -> User: ...


class EmailService:
    def send_welcome(self, user: User) -> None: ...


class ReportService:
    def generate_activity_report(self, user: User) -> Report: ...


class BillingService:
    def charge_subscription(self, user: User, amount: float) -> None: ...
```

## 3. Feature Envy

A method that mostly uses *another* object's data belongs on that object instead.

```python
# BAD: Order reaches into User's data to compute a discount
class Order:
    def calculate_discount(self, user: "User") -> float:
        if user.membership_level == "gold":
            return self.total * 0.2
        if user.account_age_days > 365:
            return self.total * 0.1
        return 0.0

# GOOD: User knows its own discount rate
class User:
    def discount_rate(self) -> float:
        if self.membership_level == "gold":
            return 0.2
        if self.account_age_days > 365:
            return 0.1
        return 0.0


class Order:
    def calculate_discount(self, user: "User") -> float:
        return self.total * user.discount_rate()
```

## 4. Primitive Obsession

Passing raw strings/numbers for domain concepts (emails, money, phone numbers) makes invalid values easy to create by accident.

```python
# BAD: a plain string can be anything
def send_email(to: str, subject: str, body: str) -> None: ...

send_email("not-an-email", "Hi", "...")  # no error until it hits the mail server

# GOOD: a small type that validates itself
import re
from dataclasses import dataclass

EMAIL_RE = re.compile(r"^[^\s@]+@[^\s@]+\.[^\s@]+$")


@dataclass(frozen=True)
class Email:
    value: str

    def __post_init__(self) -> None:
        if not EMAIL_RE.match(self.value):
            raise ValueError(f"Invalid email: {self.value}")


def send_email(to: Email, subject: str, body: str) -> None: ...

send_email(Email("user@example.com"), "Hi", "...")
```

## 5. Magic Numbers/Strings

```python
# BAD: unexplained literal values
if user.status == 2:
    ...
discount = total * 0.15
time.sleep(86400)

# GOOD: named constants
from enum import IntEnum

class UserStatus(IntEnum):
    ACTIVE = 1
    INACTIVE = 2
    SUSPENDED = 3

PREMIUM_DISCOUNT_RATE = 0.15
ONE_DAY_SECONDS = 24 * 60 * 60

if user.status == UserStatus.INACTIVE:
    ...
discount = total * PREMIUM_DISCOUNT_RATE
time.sleep(ONE_DAY_SECONDS)
```

## 6. Inappropriate Intimacy

Don't reach through an object to get to *its* internals — ask the object to do the work ("Law of Demeter").

```python
# BAD: reaching deep into another object's structure
class OrderProcessor:
    def process(self, order: Order) -> None:
        street = order.user.profile.address.street  # too intimate
        order.repository.connection.commit()          # breaks encapsulation

# GOOD: ask, don't reach
class OrderProcessor:
    def process(self, order: Order) -> None:
        street = order.shipping_street()
        order.save()
```
