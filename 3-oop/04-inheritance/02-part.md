# MRO, Type Checks, and Composition

Single inheritance gets you far. Method resolution order, `isinstance`, and composition cover the rest.

> Prefer composition when in doubt. Inherit for is-a, compose for has-a.

## `isinstance` and `issubclass`

`isinstance` checks an object. `issubclass` checks a class.

```python
class Account:
    pass

class SavingsAccount(Account):
    pass

s = SavingsAccount()
print(isinstance(s, SavingsAccount))
print(isinstance(s, Account))
print(issubclass(SavingsAccount, Account))
print(issubclass(Account, SavingsAccount))
```

```text
True
True
True
False
```

A child instance counts as its parent type. That is what makes substitution work: anywhere an `Account` fits, a `SavingsAccount` fits.

| Check | Answers | Use for |
|---|---|---|
| `isinstance(obj, Cls)` | Is this object a `Cls` (or child)? | Validation at boundaries |
| `issubclass(Child, Parent)` | Is `Child` derived from `Parent`? | Plugin and test discovery |
| `type(x) is Cls` | Exact type, no children | Rare; blocks substitution |

## MRO Intro

The method resolution order is the chain Python walks to find a method. Read it with `__mro__`.

```python
class A:
    def who(self) -> str:
        return "A"

class B(A):
    def who(self) -> str:
        return "B"

class C(B):
    pass

print([c.__name__ for c in C.__mro__])
print(C().who())
```

```text
['C', 'B', 'A', 'object']
B
```

Python checks `C`, then `B`, then `A`, then `object`. The first `who` wins.

Inspect any hierarchy from the shell:

```bash
python -c "class A: pass
class B(A): pass
class C(B): pass
print([c.__name__ for c in C.__mro__])"
```

```text
['C', 'B', 'A', 'object']
```

Keep hierarchies shallow (1-2 levels). Deep chains make the MRO hard to predict and harder to test.

## Composition vs Inheritance

Composition means an object holds other objects instead of inheriting from them.

```python
class Engine:
    def start(self) -> str:
        return "engine on"

class Car:
    def __init__(self, engine: Engine):
        self.engine = engine  # has-a, not is-a

    def start(self) -> str:
        return self.engine.start()

print(Car(Engine()).start())
```

```text
engine on
```

| Question | If yes, use |
|---|---|
| Is `SavingsAccount` an `Account`? | Inheritance |
| Does `Car` have an `Engine`? | Composition |
| Do you need 3+ levels of inheritance? | Composition, flatten it |

Deep hierarchies become rigid. Shallow hierarchies plus composition stay flexible.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Deep hierarchy (4+ levels) | Fragile changes, hard tests | Replace middle layers with composition |
| `isinstance` for all logic | Long if/elif chain | Rely on shared method names instead |
| `type(x) == Cls` blocking children | Subclass rejected wrongly | Use `isinstance(x, Cls)` |
| Forgetting `super().__init__` | `AttributeError` on parent fields | Call it first |
| Inheritance for code reuse only | Wrong is-a relationship | Extract helper class and compose |

## Checklist

- [ ] I know what `isinstance` returns for parent vs child
- [ ] I can read `__mro__` for a 3-level chain
- [ ] I can rewrite a bad inheritance chain as composition
- [ ] I choose inheritance only for true is-a links
- [ ] My hierarchies stay 1-2 levels deep

#### [Previous](./01-part.md) | [Next](./exercises.md)
