# Exercises: Functions Deep Dive

> Forward arguments, pass functions, and wrap behavior.

## Task 1: Forward

Write `logged(func, *args, **kwargs)` that prints the name and returns the result.

```python
def logged(func, *args, **kwargs):
    pass  # print + return func(*args, **kwargs)
```

```text
Expected: calling add then 7 for logged(add, 2, 5)
```

## Task 2: Sort Key

Sort `["mia", "ana", "bo"]` by length with `lambda`.

```python
names = ["mia", "ana", "bo"]
print(sorted(names, key=lambda n: len(n)))
```

```text
Expected: ['bo', 'ana', 'mia']
```

Verify quickly:

```bash
python -c "print(sorted(['mia','ana','bo'], key=lambda n: len(n)))"
```

```text
['bo', 'ana', 'mia']
```

| Exercise | Focus | Hint |
|---|---|---|
| Forward | `*args/**kwargs` | Unpack on call |
| Sort key | `lambda` | `key=len` also works |
| Timer | Decorator | Use `@wraps` |

## Task 3: Timer Decorator

Copy the `@timer` from the lesson and apply it to a slow function.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| No unpack | Tuple as single arg | `func(*args, **kwargs)` |
| Missing wraps | Wrong `__name__` | `@wraps(func)` |
| Map overuse | Hard to read | Use comprehension |

## Checklist

- [ ] Forwarder works with positional and keyword args
- [ ] Lambda sort is correct
- [ ] Decorator preserves name and returns value

#### [Previous](./02-part.md) | [Next](../03-file-io/01-part.md)
