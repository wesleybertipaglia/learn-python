# Exercises: Encapsulation

> Hide internals with `_`, guard writes with `@property`, and keep invariants green.

## Task 1: Validated Balance

Extend `BankAccount` from the lesson. The `balance` setter must reject negatives.

```python
from bank import BankAccount

acc = BankAccount("Bo", 50.0)
acc.balance = -10  # must raise
```

```text
Expected: ValueError: balance cannot be negative
```

## Task 2: Deposit Guard

`deposit(0)` and `deposit(-5)` both raise `ValueError`. A good deposit updates the balance.

```python
acc = None  # BankAccount("Bo", 50.0), then try bad deposits
```

```text
Expected: ValueError: deposit must be positive
```

Check syntax quickly:

```bash
python -m py_compile solution.py && echo OK
```

```text
OK
```

| Exercise | Focus | Hint |
|---|---|---|
| Balance | `@property` setter | Raise `ValueError` when `< 0` |
| Deposit | Method validation | Guard `<= 0` before mutating |
| Email | Property + invariant | Setter validates format, stores `_email` |

## Task 3: Validated Email

Write a `User` class with an `email` property that requires `"@"` in the value.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Setter recursion | `RecursionError` | Use `self._balance` inside |
| Bypassed setter in `__init__` | Negative accepted | Assign `self.balance = ...` |
| Double-underscore field | Subclass `AttributeError` | Use single `_` prefix |

## Checklist

- [ ] Negative balance raises `ValueError`
- [ ] Zero and negative deposits are rejected
- [ ] Email setter keeps the object valid

#### [Previous](./02-part.md) | [Next](../04-inheritance/01-part.md)
