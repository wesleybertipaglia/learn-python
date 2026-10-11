# Instance vs Class Attributes, Methods, Lifecycle

Objects carry their own state. Classes carry shared defaults. Methods glue the two together, and composition lets objects collaborate.

> Instance state answers "what is different about this object?". Class state answers "what is true for all of them?".

## Instance vs Class Attributes

Instance attributes live on `self`. Class attributes live on the class and are shared.

```python
class Account:
    currency = "EUR"  # class attribute, shared default

    def __init__(self, owner: str, balance: float = 0.0):
        self.owner = owner      # instance attribute
        self.balance = balance  # instance attribute

a = Account("Ana")
b = Account("Bo")
print(a.currency, b.currency)
print(Account.currency)
a.currency = "USD"  # shadows on ONE instance only
print(a.currency, b.currency)
```

```text
EUR EUR
EUR
USD EUR
```

Rules:

- Put constants and defaults on the class (`currency`, `interest_rate`).
- Put per-object state in `__init__` (`owner`, `balance`).
- Never use a mutable class attribute (`list`, `dict`) for per-object state.

| Location | Lookup | Mutation effect |
|---|---|---|
| `self.x` | Instance first, then class | Per-object |
| `Class.x` | Class (and bases) | Visible to all readers |
| Mutable class attr | Shared object | All instances see the change |

Verify the shadowing rule:

```bash
python -c "class A:
    x = 1
a = A(); b = A(); a.x = 2; print(a.x, b.x, A.x)"
```

```text
2 1 1
```

## Bound Methods and the Object Lifecycle

`obj.method` is a bound method: Python already filled in `self`.

```python
class Greeter:
    def __init__(self, name: str):
        self.name = name

    def hello(self) -> str:
        return f"hi, {self.name}"

g = Greeter("Ana")
bound = g.hello
print(bound())
print(Greeter.hello(g))
```

```text
hi, Ana
hi, Ana
```

Objects are born in `__init__` and die when no reference remains. You rarely need `__del__`; use context managers for files and locks instead.

```python
class Session:
    def __init__(self, user: str):
        self.user = user
        print(f"open {self.user}")

    def close(self) -> None:
        print(f"close {self.user}")

s = Session("ana")
s.close()
del s
print("done")
```

```text
open ana
close ana
done
```

| Concept | Meaning |
|---|---|
| Bound method | `g.hello` remembers `g` as `self` |
| Unbound access | `Greeter.hello(g)` passes instance manually |
| Lifecycle | `__init__` sets up; garbage collector tears down |
| Cleanup | Prefer `close()` or `with` over `__del__` |

## Composition Preview: Has-A

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

A `Library` holding `Book` objects is the same idea at larger scale: small parts, wired together, each easy to test alone.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Mutable class attribute shared | All instances share one list | Initialize mutable state in `__init__` |
| Assigning via instance expecting global change | Only one object changes | Assign on the class: `Account.currency = "USD"` |
| Holding `__del__` for cleanup | Cleanup never runs on time | Explicit `close()` or context manager |
| Calling unbound method wrong | `TypeError: missing self` | Call `obj.method()` in normal code |
| Deep-copying composed graphs naively | Shared parts duplicated | Design ownership: who owns the `Engine`? |

## Checklist

- [ ] I can explain instance vs class attribute lookup
- [ ] I never store per-object lists as class attributes
- [ ] I can explain what a bound method captures
- [ ] I can describe the object lifecycle without `__del__`
- [ ] I can sketch a has-a diagram for `Car`/`Engine`

#### [Previous](./01-part.md) | [Next](./exercises.md)
