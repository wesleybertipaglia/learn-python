# Classes and Objects

A class is a blueprint. An object is a thing built from it. If you can make one `BankAccount`, you can make a thousand.

> A class groups data and the functions that operate on that data into one reusable type.

## Why Classes

Dicts can store data, but they do not enforce shape or behavior. Classes do both.

| Approach | Shape enforced | Behavior attached | Example |
|---|---|---|---|
| `dict` | No | No | `{"owner": "Ana", "balance": 100}` |
| `class` | Yes via `__init__` | Yes via methods | `Account("Ana", 100)` |
| `tuple` | Fixed order | No | `("Ana", 100)` |

Use a class when several functions all take the same dict as first argument. That dict wants to be an object.

## Class vs Instance

The class is the definition. Instances are individual objects created from it.

```python
class Dog:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def bark(self) -> str:
        return f"{self.name} says woof!"

a = Dog("Rex", 3)
b = Dog("Mia", 1)
print(a.name, b.name)
print(type(a))
print(isinstance(a, Dog))
```

```text
Rex Mia
<class '__main__.Dog'>
True
```

| Term | What it is | Example |
|---|---|---|
| Class | Blueprint, shared definition | `Dog` |
| Instance | One concrete object | `Dog("Rex", 3)` |
| Type check | Ask what an object is | `type(a)`, `isinstance(a, Dog)` |

Each instance carries its own data. Changing `a.name` never touches `b.name`.

## Your First Class in Detail

```python
class Dog:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def bark(self) -> str:
        return f"{self.name} says woof!"

d = Dog("Rex", 3)
print(d.name)
print(d.age)
print(d.bark())
```

```text
Rex
3
Rex says woof!
```

Three ideas in 10 lines:

1. `class Dog:` defines a new type.
2. `__init__` runs automatically when you call `Dog(...)`. It sets up the object.
3. `self` is the object itself. `self.name = name` stores an attribute on it.

Check it in the shell:

```bash
python -c "print('run python, then paste the Dog class and try Dog(\"Rex\", 3)')"
```

```text
run python, then paste the Dog class and try Dog("Rex", 3)
```

Then in a REPL you would see:

```text
>>> d = Dog("Rex", 3)
>>> type(d)
<class '__main__.Dog'>
```

## Attributes and Methods

Attributes are variables on an object. Methods are functions on an object.

```python
class Counter:
    def __init__(self):
        self.count = 0

    def increment(self):
        self.count += 1

    def reset(self):
        self.count = 0

c = Counter()
c.increment()
c.increment()
print(c.count)
c.reset()
print(c.count)
```

```text
2
0
```

Rules:

- Always set all attributes inside `__init__`. Do not add new ones later.
- Every method takes `self` as first parameter. Python passes it automatically.
- Call methods with `obj.method()`, never `Class.method(obj)` in normal code.

| Concept | Where it lives | Access |
|---|---|---|
| Attribute | `self.count` | `c.count` |
| Method | `def increment(self)` | `c.increment()` |
| Constructor | `def __init__(self, ...)` | `Counter()` |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Missing `self` in method | `TypeError: bark() takes 0 positional arguments` | `def bark(self):` |
| Forgetting `self.` in `__init__` | Attribute lost, `AttributeError` later | `self.name = name` |
| Attributes added outside `__init__` | Objects with different shapes | Set everything in `__init__` |
| Calling `Dog.bark()` without instance | Missing argument error | Use `d.bark()` |
| Confusing class with instance | Shared state surprises | Keep per-object data on `self` |

## Checklist

- [ ] I can explain class vs instance in one sentence
- [ ] I can explain what `self` is without hesitation
- [ ] I set all attributes inside `__init__`
- [ ] I can write a method that reads and updates attributes
- [ ] My test class creates two independent instances

#### [Previous](../../2-data-structures/challenge.md) | [Next](./02-part.md)
