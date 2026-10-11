# Exercises: Inheritance

> Practice `super()`, overriding, type checks, and composition choices.

## Task 1: Savings Account

Write `SavingsAccount(Account)` with `rate` and `add_interest()`. Use `super().__init__`.

```python
s = None  # SavingsAccount("Ana", 1000.0, rate=0.05) then add_interest()
```

```text
Expected balance: 1050.0
```

## Task 2: Fee Override

Override `fee()` in the child: parent returns `0.0`, child returns `1.0`. Extend with `super()` in a second subclass.

```python
child_fee = None  # SavingsAccount("Bo").fee()
```

```text
Expected: 1.0
```

Quick check:

```bash
python -m py_compile solution.py && echo OK
```

```text
OK
```

| Exercise | Focus | Hint |
|---|---|---|
| Savings | `super()`, override | Call parent init first |
| Fee | Replace vs extend | `super().fee() + extra` |
| Garage | Composition | Hold, do not inherit, the engine |

## Task 3: Engine Composition

Refactor a `Car(Engine)` subclass attempt into `Car` holding an `Engine`.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Missing `super().__init__` | `AttributeError: owner` | Call it first |
| Wrong override signature | `TypeError` | Copy parent signature |
| Inheritance for has-a | Awkward hierarchy | Compose instead |

## Checklist

- [ ] Subclass initializes parent correctly
- [ ] Override and extend both work
- [ ] Composition replaces the wrong inheritance

#### [Previous](./02-part.md) | [Next](../05-polymorphism/01-part.md)
