# Inheritance: Reuse with super()

Inheritance models is-a. A `SavingsAccount` is an `Account` with extra behavior.

> Inherit to reuse behavior. Override to specialize it. Call `super()` to keep the parent working.

## Basic Inheritance

```python
class Animal:
    def __init__(self, name: str):
        self.name = name

    def speak(self) -> str:
        return "..."

class Dog(Animal):
    def speak(self) -> str:
        return f"{self.name} says woof!"

class Cat(Animal):
    def speak(self) -> str:
        return f"{self.name} says meow!"

for a in [Dog("Rex"), Cat("Mia")]:
    print(a.speak())
```

```text
Rex says woof!
Mia says meow!
```

`Dog(Animal)` means Dog inherits from Animal. It gets `__init__` and any method it does not override.

Inspect the hierarchy:

```bash
python -c "class A: pass
class B(A): pass
print(B.__mro__)"
```

```text
(<class 'B'>, <class 'A'>, <class 'object'>)
```

## `super()` and Method Override

Use `super()` to extend parent behavior instead of replacing it.

```python
class Account:
    def __init__(self, owner: str, balance: float = 0.0):
        self.owner = owner
        self.balance = balance

    def fee(self) -> float:
        return 0.0

    def __str__(self) -> str:
        return f"{self.owner}: {self.balance:.2f}"

class SavingsAccount(Account):
    def __init__(self, owner: str, balance: float = 0.0, rate: float = 0.02):
        super().__init__(owner, balance)
        self.rate = rate

    def fee(self) -> float:
        return 1.0  # override: savings has a monthly fee

    def add_interest(self) -> None:
        self.balance += self.balance * self.rate

s = SavingsAccount("Ana", 1000.0)
s.add_interest()
print(s)
print(s.fee())
```

```text
Ana: 1020.00
1.0
```

Rules for `super()`:

- Call `super().__init__(...)` first in the child `__init__`.
- Override by redefining the method with the same name.
- Call `super().method()` inside the override when you want parent logic plus extra.

| Pattern | Code | When |
|---|---|---|
| Inherit init | `super().__init__(owner, balance)` | Child adds attributes |
| Full override | `def fee(self): return 1.0` | Behavior differs |
| Extend | `super().fee() + 1.0` | Parent logic + extra |

Extending keeps the parent in the loop:

```python
class CheckingAccount(Account):
    def fee(self) -> float:
        return super().fee() + 2.0

print(CheckingAccount("Bo", 50.0).fee())
```

```text
2.0
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Forgetting `super().__init__` | Missing attributes, `AttributeError` | Call it first in child `__init__` |
| Wrong override signature | `TypeError` or silent mismatch | Copy parent signature exactly |
| Duplicating parent logic | Two places to fix bugs | Call `super().method()` and add delta |
| Mutable default in `__init__` | Shared list across instances | Use `None` then create new list |
| Overriding `__init__` without new args | Pointless override | Skip it; parent `__init__` is inherited |

## Checklist

- [ ] I can write a subclass that calls `super().__init__`
- [ ] I can override a method and optionally extend it
- [ ] I know the difference between replace and extend overrides
- [ ] My hierarchies stay 1-2 levels deep
- [ ] Child adds state without breaking parent construction

#### [Previous](../03-encapsulation/exercises.md) | [Next](./02-part.md)
