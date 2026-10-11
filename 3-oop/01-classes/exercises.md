# Exercises: Classes

> Write small classes with clean `__init__`, methods, and string representation.
## Task 1: Book Class

Write a `Book` with `title`, `author`, `pages`. Add `__str__` like `"1984 by Orwell (328 pages)"`.

```python
class Book:
    def __init__(self, title: str, author: str, pages: int):
        self.title = title
        self.author = author
        self.pages = pages

    def __str__(self) -> str:
        return f"{self.title} by {self.author} ({self.pages} pages)"
```

```text
Expected: 1984 by Orwell (328 pages)
```

## Task 2: Counter

Write a `Counter` starting at 0 with `increment()`, `reset()`, and `__repr__`.

```python
counter = None  # Counter(), increment() twice, then reset()
```

```text
Expected: count goes 0 -> 1 -> 2 -> 0
```

Check syntax quickly:

```bash
python -m py_compile solution.py && echo OK
```

```text
OK
```

| Exercise | Focus | Hint |
|---|---|---|
| Book | `__init__`, `__str__` | Store all three args on `self` |
| Counter | Methods + state | `increment` adds 1, `reset` sets 0 |
| Point | `@dataclass` | Annotate fields, free `__eq__` |

## Task 3: Dataclass Point

Convert manual `Point(x, y)` to a `@dataclass` and verify `==` works.
---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Missing `self` | `TypeError` on call | Add `self` first |
| No `__str__` | `<__main__.Book object at ...>` | Define `__str__` |
| Mutable default `[]` | Shared state | Use `field(default_factory=list)` |

## Checklist

- [ ] Book prints in the required format
- [ ] Counter increments and resets correctly
- [ ] Dataclass Point compares equal with `==`

#### [Previous](./02-part.md) | [Next](../02-objects/01-part.md)
