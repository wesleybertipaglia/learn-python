# Exercises: Objects

> Practice construction, `is` vs `==`, and shared vs per-object state.

## Task 1: Identity vs Equality

Create two `Point(2, 5)` objects. Show `==` is `True` (with `__eq__`) while `is` is `False`.

```python
a = None  # Point(2, 5)
b = None  # Point(2, 5)
```

```text
Expected: a == b True, a is b False, c = a then a is c True
```

## Task 2: Class Attribute

Give `Account` a class attribute `currency = "EUR"`. Shadow it on one instance and show the other is unaffected.

```python
acc = None  # Account("Ana"); set acc.currency = "USD" on one only
```

```text
Expected: USD EUR
```

Check your file compiles:

```bash
python -m py_compile solution.py && echo OK
```

```text
OK
```

| Exercise | Focus | Hint |
|---|---|---|
| Identity | `is` vs `==` | Define `__eq__`, keep `is` for `None` |
| Currency | Class attributes | Assign on instance shadows only |
| Garage | Composition | `Garage` holds a `Car`, delegates `start` |

## Task 3: Garage Composition

Write `Garage` holding a `Car` (which holds an `Engine`). `Garage.open()` starts the car.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `is` for values | Always `False` for distinct objects | Use `==` |
| Mutable class list | Shared across instances | Init list in `__init__` |
| Alias instead of copy | Two names mutate one object | `copy.deepcopy` |

## Checklist

- [ ] `__eq__` distinguishes value from identity
- [ ] Class attribute shadowing behaves as expected
- [ ] Composition delegates without inheritance

#### [Previous](./02-part.md) | [Next](../03-encapsulation/01-part.md)
