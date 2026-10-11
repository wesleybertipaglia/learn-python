# Exercises: Iterators and Generators

> Build lazy pipelines. Watch memory stay flat.

## Task 1: Manual Iterator

Use `iter()` / `next()` to print all items of `["a", "b"]`, then catch `StopIteration`.

```python
it = iter(["a", "b"])
print(next(it))
# finish it
```

```text
Expected: a, b, then StopIteration
```

## Task 2: Even Generator

Write `evens(limit)` yielding even numbers below `limit`.

```python
def evens(limit: int):
    pass  # use yield
```

```text
Expected list(evens(7)) == [0, 2, 4, 6]
```

Check laziness:

```bash
python -c "print(type(x for x in range(3)))"
```

```text
<class 'generator'>
```

| Exercise | Focus | Hint |
|---|---|---|
| Manual iter | Protocol | `try/except StopIteration` |
| Evens | `yield` | `range(0, limit, 2)` + yield |
| Sum squares | Genexpr | `sum(x*x for x in ...)` |

## Task 3: Sum of Squares

Compute `sum(x*x for x in range(100))` without building a list.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Second use empty | `[]` | Recreate generator |
| Used `[...]` not `(...)` | List built | Switch brackets |
| Infinite generator | Hang | Add stop condition |

## Checklist

- [ ] I drive an iterator manually
- [ ] My generator yields correct values
- [ ] I use a generator expression with `sum`

#### [Previous](./02-part.md) | [Next](../02-functions-deep-dive/01-part.md)
