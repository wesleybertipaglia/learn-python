# Flexible Functions

`*args` and `**kwargs` make functions accept anything. First-class functions let you pass behavior. Comprehensions usually beat `map`/`filter`.

> Accept explicit arguments by default. Reach for `*args`/`**kwargs` only at boundaries like wrappers and plugins.

## `*args` and `**kwargs` Deep

```python
def report(*args: float, **kwargs: str) -> None:
    print("args:", args)
    print("kwargs:", kwargs)

report(1, 2, 3, name="ana", city="berlin")
```

```text
args: (1, 2, 3)
kwargs: {'name': 'ana', 'city': 'berlin'}
```

Unpacking works both ways with `*` and `**`:

```python
def add(a: int, b: int) -> int:
    return a + b

nums = [3, 4]
opts = {"a": 3, "b": 4}
print(add(*nums))
print(add(**opts))
```

```text
7
7
```

Forwarding pattern for wrappers:

```python
def logged(func, *args, **kwargs):
    print(f"calling {func.__name__}")
    return func(*args, **kwargs)

print(logged(add, 2, 5))
print(logged(add, a=1, b=9))
```

```text
calling add
7
calling add
10
```

Inspect signatures while learning:

```bash
python -c "import inspect; print(inspect.signature(print))"
```

```text
(*args, sep=' ', end='\n', file=None, flush=False)
```

| Syntax | In definition | In call |
|---|---|---|
| `*args` | Collect positionals into tuple | Unpack iterable |
| `**kwargs` | Collect keywords into dict | Unpack mapping |
| `def f(a, *args, key=None)` | Mixed, explicit first | `f(1, 2, 3, key="x")` |

## First-Class Functions and `lambda`

Functions are values. Store them, pass them, return them.

```python
def shout(text: str) -> str:
    return text.upper()

def whisper(text: str) -> str:
    return text.lower()

styles = {"loud": shout, "quiet": whisper}
print(styles["loud"]("hello"))
print(styles["quiet"]("HELLO"))
```

```text
HELLO
hello
```

`lambda` is a one-expression function. Use it for tiny keys, not logic.

```python
names = ["mia", "ana", "bo"]
print(sorted(names))
print(sorted(names, key=lambda n: len(n)))
```

```text
['ana', 'bo', 'mia']
['bo', 'ana', 'mia']
```

Name a `def` when the lambda grows past one line or needs reuse.

## `map`/`filter` vs Comprehensions

Prefer comprehensions. They read better and are faster to debug.

```python
nums = [1, 2, 3, 4, 5]

doubled_map = list(map(lambda x: x * 2, nums))
doubled_comp = [x * 2 for x in nums]
evens_comp = [x for x in nums if x % 2 == 0]
evens_filter = list(filter(lambda x: x % 2 == 0, nums))

print(doubled_comp)
print(evens_comp)
print(doubled_map == doubled_comp)
```

```text
[2, 4, 6, 8, 10]
[2, 4]
True
```

| Task | Pythonic | Avoid |
|---|---|---|
| Transform | `[f(x) for x in xs]` | `map(lambda ...)` |
| Filter | `[x for x in xs if cond]` | `filter(lambda ...)` |
| Transform + filter | `[f(x) for x in xs if cond]` | `map` + `filter` chain |

Keep `map`/`filter` only when a named function already exists: `list(map(str.strip, lines))`.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Mutable default `def f(x=[])` | Shared list | `def f(x=None)` then `x = [] if x is None` |
| Positional after keyword | `SyntaxError` | `f(1, b=2)`, never `f(a=1, 2)` |
| Big `lambda` with statements | `SyntaxError` | Use `def` |
| Forgetting `*` when forwarding | Tuple passed as one arg | `func(*args, **kwargs)` |
| Overusing `**kwargs` everywhere | Lost autocompletion, silent typos | Explicit params + `**kwargs` only at edges |

## Checklist

- [ ] I can collect and forward `*args` / `**kwargs`
- [ ] I can unpack a list and dict into a call
- [ ] I can pass functions as values
- [ ] I use `lambda` only for tiny keys
- [ ] I default to comprehensions over `map`/`filter`

#### [Previous](../01-iterators-generators/exercises.md) | [Next](./02-part.md)
