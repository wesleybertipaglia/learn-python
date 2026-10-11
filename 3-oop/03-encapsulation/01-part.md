# Encapsulation: Conventions and Properties

Python encapsulation is by convention, not by force. A leading underscore means hands off, and `@property` adds guardrails where they pay.

> Use `_` for internal, `@property` for controlled access, methods for actions. Keep attributes public unless you have a reason.

## The `_single_underscore` Convention

| Name | Meaning | Enforced? |
|---|---|---|
| `balance` | Public, part of the API | No |
| `_balance` | Internal, do not touch directly | No, convention only |
| `__balance` | Name-mangled, avoid it | Partly, confusing |

Prefer `_balance` over `__balance`. Double underscore triggers name mangling and surprises subclasses. You almost never need it.

```python
class Account:
    def __init__(self, owner: str, balance: float = 0.0):
        self.owner = owner
        self._balance = balance

acc = Account("Ana", 100.0)
print(acc.owner)
print(acc._balance)
```

```text
Ana
100.0
```

`_balance` is still accessible. The underscore is a signal to other developers, not a lock.

Verify style with:

```bash
python -m py_compile account.py && echo "compiles OK"
```

```text
compiles OK
```

## `__` Name Mangling and Why You Rarely Need It

Two leading underscores rewrite the attribute name to `_ClassName__attr`. It was designed to avoid collisions in subclasses, not to make things private.

```python
class Base:
    def __init__(self):
        self.__value = 1

b = Base()
print(b._Base__value)
print([n for n in vars(b) if "value" in n])
```

```text
1
['_Base__value']
```

| Fact | Implication |
|---|---|
| `self.__x` becomes `self._Class__x` | Subclass `self.__x` is a different name |
| Still reachable as `_Class__x` | Not real privacy, just obscurity |
| Breaks subclass overrides silently | Two attributes where you expected one |
| Debuggers show mangled names | Harder to inspect |

Rule: use `_value`. Reach for `__value` only to dodge a proven name clash in a deep hierarchy — then document why.

## `@property` Getters and Setters

Use `@property` when reading or writing needs logic, like validation.

```python
class Account:
    def __init__(self, owner: str, balance: float = 0.0):
        self.owner = owner
        self._balance = 0.0
        self.balance = balance  # goes through setter

    @property
    def balance(self) -> float:
        return self._balance

    @balance.setter
    def balance(self, value: float):
        if value < 0:
            raise ValueError("balance cannot be negative")
        self._balance = value

acc = Account("Ana", 100.0)
print(acc.balance)
acc.balance = 150.0
print(acc.balance)
```

```text
100.0
150.0
```

Start with plain attributes. Convert to `@property` later without changing callers. That is the point: `acc.balance` works in both versions.

Trigger the guard to see it fire:

```bash
python -c "print('try acc.balance = -5 and read the ValueError')"
```

```text
ValueError: balance cannot be negative
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Recursive property (`self.balance` inside getter) | `RecursionError` | Store in `self._balance`, expose as `balance` |
| Setter bypassed in `__init__` | Invalid state accepted | Assign `self.balance = balance` |
| Using `__balance` then subclass fails | `AttributeError: _Child__balance` | Use single `_balance` |
| Treating `_x` as enforced privacy | Surprise when external code touches it | Document the contract; validate in setter |
| Validation only in one method | Inconsistent state | Centralize in property setter |

## Checklist

- [ ] I use `_name` for internal attributes
- [ ] I can explain name mangling and why I avoid it
- [ ] I can write a validated `@property` with getter and setter
- [ ] `__init__` routes through the setter
- [ ] Callers use attribute syntax with logic underneath

#### [Previous](../02-objects/exercises.md) | [Next](./02-part.md)
