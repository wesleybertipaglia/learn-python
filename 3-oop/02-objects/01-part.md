# Objects: Instances and Identity

Every time you call a class, you build a fresh object. Understanding construction, identity, and equality keeps you out of aliasing traps.

> A class is the cookie cutter. Objects are the cookies. Each cookie has its own crumbs.

## Construction: How an Instance Is Born

Calling `Dog(...)` creates the object, then runs `__init__` to fill it in.

```python
class Dog:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

d = Dog("Rex", 3)
print(d.name, d.age)
print(type(d).__name__)
```

```text
Rex 3
Dog
```

| Step | What happens |
|---|---|
| `Dog("Rex", 3)` | Python allocates a new empty object |
| `__init__(self, ...)` | Your code sets attributes on `self` |
| Return | You get a ready-to-use instance |

Always set every attribute inside `__init__`. An object should be valid the moment it exists.

Confirm construction order in the shell:

```bash
python -c "class A:
    def __init__(self): print('init runs')
A(); print('object ready')"
```

```text
init runs
object ready
```

## Identity vs Equality: `is` vs `==`

`is` asks "same object in memory?". `==` asks "same value?". They are different questions.

```python
class Point:
    def __init__(self, x: float, y: float):
        self.x = x
        self.y = y

    def __eq__(self, other: object) -> bool:
        if not isinstance(other, Point):
            return NotImplemented
        return self.x == other.x and self.y == other.y

a = Point(2, 5)
b = Point(2, 5)
c = a
print(a == b)
print(a is b)
print(a is c)
```

```text
True
False
True
```

| Operator | Meaning | Default without `__eq__` |
|---|---|---|
| `is` | Same object identity | Identity comparison |
| `==` | Equal value | Falls back to identity |
| `is not` / `!=` | Negations | Same logic inverted |

Without `__eq__`, two points with identical coordinates compare unequal. Dataclasses generate `__eq__` for you. Manual classes need it written out.

> Use `is` only for singletons like `None`. Use `==` for every value comparison.

## Mutable State and Aliasing

Variables hold references, not copies. Two names can point at one object.

```python
class Cart:
    def __init__(self):
        self.items: list[str] = []

a = Cart()
b = a  # alias, not a copy
b.items.append("apple")
print(a.items)
print(a is b)
```

```text
['apple']
True
```

Copy deliberately when you need independence:

```python
import copy

a = Cart()
a.items.append("apple")
shallow = copy.copy(a)
deep = copy.deepcopy(a)
print(shallow.items, deep.items)
print(shallow is a, deep is a)
```

```text
['apple'] ['apple']
False False
```

| Situation | What to do |
|---|---|
| Pass object into a function | Remember the function can mutate it |
| Need an independent object | `copy.deepcopy(obj)` |
| `None` default then build list | `def __init__(self, items=None)` pattern |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Using `is` for values | `Point(2,5) is Point(2,5)` is `False` | Use `==` with `__eq__` defined |
| Missing `__eq__` | Equal objects compare `False` | Define `__eq__` or use `@dataclass` |
| Accidental aliasing | Mutating `b` changes `a` | Copy explicitly with `copy` module |
| Half-initialized object | `AttributeError` later | Set all attributes in `__init__` |
| `==` without `NotImplemented` guard | `TypeError` on mixed types | Return `NotImplemented` for unknown types |

## Checklist

- [ ] I can describe the two steps of construction
- [ ] I use `is` only for `None` and singletons
- [ ] I can write `__eq__` for a small class
- [ ] I can explain aliasing with a diagram or example
- [ ] I know when to reach for `copy.deepcopy`

#### [Previous](../01-classes/exercises.md) | [Next](./02-part.md)
