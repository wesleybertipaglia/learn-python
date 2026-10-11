# Dunder Overloading and Protocols

Python uses double-underscore methods as hooks. Overload them to make your objects feel native, and use `typing.Protocol` to write down duck-type contracts.

> Teach your class the language Python already speaks: `len()`, `==`, `+`, iteration.

## Overloading `__len__`, `__eq__`, `__add__`

```python
class Playlist:
    def __init__(self, songs: list[str]):
        self.songs = list(songs)

    def __len__(self) -> int:
        return len(self.songs)

    def __eq__(self, other: object) -> bool:
        if not isinstance(other, Playlist):
            return NotImplemented
        return self.songs == other.songs

    def __add__(self, other: "Playlist") -> "Playlist":
        if not isinstance(other, Playlist):
            return NotImplemented
        return Playlist(self.songs + other.songs)

a = Playlist(["a", "b"])
b = Playlist(["c"])
print(len(a))
print(a == Playlist(["a", "b"]))
print((a + b).songs)
```

```text
2
True
['a', 'b', 'c']
```

| Dunder | Called by | Must return |
|---|---|---|
| `__len__` | `len(obj)` | Non-negative `int` |
| `__eq__` | `==`, `!=` | `bool` or `NotImplemented` |
| `__add__` | `obj + other` | New object (do not mutate) |

Keep overloads unsurprising: `len()` is cheap, `==` is symmetric, `+` builds something new.

Confirm the operators route correctly:

```bash
python -c "print('len() -> __len__, == -> __eq__, + -> __add__')"
```

```text
len() -> __len__, == -> __eq__, + -> __add__
```

## `typing.Protocol`: Writing Down Duck Types

A `Protocol` names the methods a duck must have. Static checkers verify it; runtime code still just calls the method.

```python
from typing import Protocol

class HasArea(Protocol):
    def area(self) -> float: ...

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

def total_area(shapes: list[HasArea]) -> float:
    return sum(s.area() for s in shapes)

print(total_area([Circle(1.0), Square(2.0)]))
```

```text
5.14159
```

`Circle` never inherits `HasArea`. It just matches structurally. That is static duck typing.

| Tool | Checks when | Cost |
|---|---|---|
| Duck typing alone | Runtime, on call | Zero annotations |
| `Protocol` | Type-check time (`mypy`, IDE) | One small class |
| Abstract base class | Runtime + instantiation block | Heavier, see next topic |

> Use `Protocol` for flexible interfaces, ABCs for mandatory hierarchies with shared code.

## Putting It Together

Overloaded operators plus a protocol make a small class feel built in.

```python
from typing import Protocol

class HasSongs(Protocol):
    def __len__(self) -> int: ...

class Playlist:
    def __init__(self, songs: list[str]):
        self.songs = list(songs)

    def __len__(self) -> int:
        return len(self.songs)

def describe(pl: HasSongs) -> str:
    return f"playlist with {len(pl)} songs"

print(describe(Playlist(["a", "b"])))
```

```text
playlist with 2 songs
```

The protocol guarantees `len()` works. The dunder delivers it.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `__eq__` without type guard | `AttributeError` on other types | Return `NotImplemented` for unknown types |
| `__add__` mutating `self` | Caller data corrupted | Return a new instance |
| `__len__` returning non-int | `TypeError: __len__ should return int` | Return `len(self.items)` |
| Protocol with missing method | Checker error, runtime `AttributeError` | Implement every protocol member |
| Overloading too many dunders | Confusing class | Overload only what callers need |

## Checklist

- [ ] I can overload `__len__`, `__eq__`, `__add__` correctly
- [ ] `+` returns a new object without mutating inputs
- [ ] `__eq__` returns `NotImplemented` for foreign types
- [ ] I can write a `Protocol` for a one-method interface
- [ ] I know when to pick `Protocol` over inheritance

#### [Previous](./01-part.md) | [Next](./exercises.md)
