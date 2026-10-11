# Polymorphism: One Interface, Many Classes

Polymorphism means different classes answer the same message. Python leans on duck typing: if it has `area()`, call `area()`.

> Do not ask what it is. Ask what it can do.

## Duck Typing and EAFP

Duck typing: "if it walks like a duck and quacks like a duck, it is a duck for our purposes." EAFP: easier to ask forgiveness than permission — try, then catch `AttributeError` or `TypeError`.

```python
class Circle:
    def __init__(self, r: float):
        self.r = r

    def area(self) -> float:
        return 3.14159 * self.r * self.r

class Square:
    def __init__(self, side: float):
        self.side = side

    def area(self) -> float:
        return self.side * self.side

def total_area(shapes) -> float:
    return sum(s.area() for s in shapes)

print(total_area([Circle(1.0), Square(2.0)]))
```

```text
5.14159
```

No shared base class. No `isinstance` chain. The loop just calls `.area()`.

| Style | Code | Verdict |
|---|---|---|
| Duck typing (good) | `s.area()` | Works for any shape with `area` |
| LBYL with `isinstance` chain | `if isinstance(s, Circle)... elif ...` | Brittle, edit for every new shape |
| EAFP guard | `try: s.area() except AttributeError` | Good at plugin boundaries |

EAFP at a boundary looks like this:

```python
def describe(obj) -> str:
    try:
        return f"area={obj.area():.2f}"
    except AttributeError:
        return "no area available"

print(describe(Square(2.0)))
print(describe(42))
```

```text
area=4.00
no area available
```

## One Interface, Many Shapes

Add a third shape without touching `total_area`. That is the payoff.

```python
class Circle:
    def __init__(self, r: float):
        self.r = r

    def area(self) -> float:
        return 3.14159 * self.r * self.r

class Square:
    def __init__(self, side: float):
        self.side = side

    def area(self) -> float:
        return self.side * self.side

class Triangle:
    def __init__(self, base: float, height: float):
        self.base = base
        self.height = height

    def area(self) -> float:
        return 0.5 * self.base * self.height

shapes = [Circle(1.0), Square(2.0), Triangle(3.0, 4.0)]
for s in shapes:
    print(f"{type(s).__name__}: {s.area():.2f}")
```

```text
Circle: 3.14
Square: 4.00
Triangle: 6.00
```

Count the interface, not the classes:

```bash
python -c "print('shapes with area(): Circle, Square, Triangle = 3 classes, 1 interface')"
```

```text
shapes with area(): Circle, Square, Triangle = 3 classes, 1 interface
```

| Rule | Why |
|---|---|
| Same method name + signature | Callers stay uniform |
| Return compatible types | `sum()` keeps working |
| Document the interface | New classes know the contract |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `isinstance` chain for dispatch | Every new class edits old code | Call the shared method directly |
| Inconsistent signatures | `TypeError` on some classes | Keep `area(self)` identical everywhere |
| Mixed return types | `sum()` crashes | All return `float` |
| Catching too broadly | Real bugs hidden | Catch `AttributeError` only at boundary |
| Assuming inheritance is required | Unneeded base class | Duck typing needs only the method |

## Checklist

- [ ] I can explain duck typing in one sentence
- [ ] I can contrast EAFP with look-before-you-leap
- [ ] `total_area` works with a new shape untouched
- [ ] Boundary code handles missing methods gracefully
- [ ] My interfaces use identical names and signatures

#### [Previous](../04-inheritance/exercises.md) | [Next](./02-part.md)
