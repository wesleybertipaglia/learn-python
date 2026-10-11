# Exercises: Polymorphism

> One interface, many classes. Let the method call do the dispatching.

## Task 1: Shapes Area

Write `Circle` and `Square` with `.area()`, plus `total_area(shapes)` using only duck typing.

```python
total = None  # total_area([Circle(1.0), Square(2.0)])
```

```text
Expected: about 7.14 (pi + 4.0)
```

## Task 2: Playlist Add

Implement `Playlist.__len__` and `__add__` so `len(a + b)` equals the combined count.

```python
combined = None  # Playlist(["a"]) + Playlist(["b", "c"])
```

```text
Expected len: 3
```

Check quickly:

```bash
python -m py_compile solution.py && echo OK
```

```text
OK
```

| Exercise | Focus | Hint |
|---|---|---|
| Shapes | Duck typing | Just call `s.area()` |
| Playlist | Dunder overload | `__add__` returns new object |
| Protocol | `HasArea` | One method, structural match |

## Task 3: HasArea Protocol

Define `HasArea` with `area() -> float` and annotate `total_area`.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `isinstance` chain | Brittle dispatch | Call the shared method |
| Mutating `__add__` | Lost data | Return new instance |
| Missing `__eq__` guard | `AttributeError` | Return `NotImplemented` |

## Checklist

- [ ] Shapes share one interface with no base class
- [ ] Playlist `+` and `len()` behave natively
- [ ] Protocol annotates the duck type

#### [Previous](./02-part.md) | [Next](../06-abstraction/01-part.md)
