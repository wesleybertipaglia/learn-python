# Methods, String Representation, and Dataclasses

Methods give objects behavior. `__str__` and `__repr__` give them readable faces. Dataclasses remove boilerplate when a class mostly holds data.

> Start with a plain class. Reach for `@dataclass` when `__init__` plus `__repr__` is most of the file.

## Methods in Depth

A method is a function bound to an object. It reads and changes `self`.

```python
class BankAccount:
    def __init__(self, owner: str, balance: float = 0.0):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount: float) -> None:
        if amount <= 0:
            raise ValueError("deposit must be positive")
        self.balance += amount

    def withdraw(self, amount: float) -> None:
        if amount <= 0:
            raise ValueError("withdraw must be positive")
        if amount > self.balance:
            raise ValueError("insufficient funds")
        self.balance -= amount

acc = BankAccount("Ana", 100.0)
acc.deposit(50.0)
acc.withdraw(30.0)
print(acc.owner, acc.balance)
```

```text
Ana 120.0
```

| Guideline | Reason |
|---|---|
| Methods validate their input | Object never enters a bad state |
| Methods mutate `self`, then return `None` | Caller sees the change on the object |
| One method, one action | `deposit` does not also print statements |

## `__str__` and `__repr__`

`print(obj)` calls `__str__`. The REPL echo calls `__repr__`. Define both.

```python
class Point:
    def __init__(self, x: float, y: float):
        self.x = x
        self.y = y

    def __str__(self) -> str:
        return f"({self.x}, {self.y})"

    def __repr__(self) -> str:
        return f"Point(x={self.x!r}, y={self.y!r})"

p = Point(2, 5)
print(p)
print(repr(p))
```

```text
(2, 5)
Point(x=2, y=5)
```

Convention:

- `__str__` is for users: short, readable.
- `__repr__` is for developers: unambiguous, ideally copy-pasteable.

If you define only one, define `__repr__`. Python falls back to it when `__str__` is missing.

Check both render paths:

```bash
python -c "class P:
    def __repr__(self): return 'P()'
print(P())"
```

```text
P()
```

## Classes for Plain Data: `@dataclass` Basics

If your class is mostly `__init__` plus `__repr__`, it wants to be a dataclass.

```python
from dataclasses import dataclass

@dataclass
class Point:
    x: float
    y: float

p = Point(2, 5)
print(p)
print(p == Point(2, 5))
```

```text
Point(x=2, y=5)
True
```

You get `__init__`, `__repr__`, and `__eq__` for free. Compare with ~15 lines of manual code.

| Feature | Manual class | `@dataclass` |
|---|---|---|
| `__init__` | Write it | Generated |
| `__repr__` | Write it | Generated |
| `__eq__` | Write it | Generated |
| Custom behavior | Any method | Add methods normally |

Dataclasses accept methods and defaults too:

```python
from dataclasses import dataclass, field

@dataclass
class Account:
    owner: str
    balance: float = 0.0
    tags: list[str] = field(default_factory=list)

    def deposit(self, amount: float) -> None:
        if amount <= 0:
            raise ValueError("deposit must be positive")
        self.balance += amount

a = Account("Ana")
a.deposit(20.0)
print(a)
```

```text
Account(owner='Ana', balance=20.0, tags=[])
```

Always use `field(default_factory=list)` for mutable defaults. Never write `tags: list = []`.

Verify dataclass support on your interpreter:

```bash
python --version && python -c "import dataclasses; print('dataclasses OK')"
```

```text
Python 3.12.3
dataclasses OK
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `__str__` returns non-string | `TypeError: __str__ returned non-string` | Always `return f"..."` |
| No `__repr__` defined | `<__main__.Point object at 0x...>` | Define `__repr__` with all fields |
| Mutable default `[]` in dataclass | Shared list across instances | `field(default_factory=list)` |
| No type hints in dataclass | Fields missing or confusing | Always annotate: `x: float` |
| Using dataclass for heavy behavior | Bloated model | Dataclass for data, methods for small logic |

## Checklist

- [ ] I can write methods that validate and mutate `self`
- [ ] I know when `__str__` vs `__repr__` is used
- [ ] My classes print cleanly with `print()` and in the REPL
- [ ] I can convert a data-holder class to `@dataclass`
- [ ] I use `field(default_factory=...)` for lists and dicts

#### [Previous](./01-part.md) | [Next](./exercises.md)
