# Generators and Laziness

Generators produce one value at a time. Memory stays flat even for million-line files.

> If the input may be huge, yield it. Never `read().split()` a file you can stream.

## `yield` and Generator Functions

A function with `yield` returns a generator, not a value. Each `next()` runs until the next `yield`.

```python
def countdown(n: int):
    while n > 0:
        yield n
        n -= 1

g = countdown(3)
print(type(g))
print(next(g))
print(next(g))
print(list(g))
```

```text
<class 'generator'>
3
2
[1]
```

After partial consumption, `list(g)` returns only the remainder. Generators are single-use iterators.

| `return` function | `yield` function |
|---|---|
| Runs to completion | Pauses at each `yield` |
| Returns one value | Returns lazy generator |
| Holds all results in memory | Holds one item at a time |

## Generator Expressions

Same idea as list comprehensions, but lazy with `(...)`.

```python
nums = [1, 2, 3, 4, 5]
squares_list = [x * x for x in nums]
squares_gen = (x * x for x in nums)
print(squares_list)
print(squares_gen)
print(sum(x * x for x in nums))
```

```text
[1, 4, 9, 16, 25]
<generator object <genexpr> at ...>
55
```

Use generator expressions inside `sum()`, `any()`, `max()` to avoid a temporary list. The address in `<generator object ...>` differs per run; that is normal.

Measure the difference:

```bash
python -c "import sys; print(sys.getsizeof([x for x in range(1000)])); print(sys.getsizeof(x for x in range(1000)))"
```

```text
8856
208
```

## Lazy File Reading Pattern

This is the killer use case. Process line by line.

```python
def read_lines(path: str):
    with open(path, encoding="utf-8") as f:
        for line in f:
            yield line.strip()

# demo without a real file: simulate with a list
def fake_lines():
    yield "ana,100"
    yield "bo,200"

total = sum(int(line.split(",")[1]) for line in fake_lines())
print(total)
```

```text
300
```

Real usage:

```python
# for line in read_lines("big.csv"):
#     process(line)  # constant memory, works for 10 GB files
```

Never do `open(...).read().splitlines()` on large files. Stream them.

| Pattern | Memory | Use |
|---|---|---|
| `Path.read_text().splitlines()` | O(n) | Small files only |
| `for line in f:` / `yield` | O(1) | Large files, logs, CSV |
| List comprehension | O(n) | Need all results twice |
| Generator expression | O(1) | Single pass + `sum`/`any` |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Reusing a generator | Second `list(g)` is `[]` | Recreate it or store a list |
| `return value` inside generator | `StopIteration: value`, confusing | Use bare `return` to stop early |
| Forgetting `yield` runs lazily | Nothing happens until iterated | Call `list()` / loop to execute |
| Eager list on big file | `MemoryError` | Yield lines, use generator expression |
| Printing generator directly | `<generator object ...>` | Iterate it: `list(g)` or `for x in g` |

## Checklist

- [ ] I can write a generator function with `yield`
- [ ] I know generators are single-use
- [ ] I can rewrite `[f(x) for x in xs]` as `(f(x) for x in xs)`
- [ ] I can stream a file line by line with constant memory
- [ ] I pass generators to `sum`/`any`/`max` without a list

#### [Previous](./01-part.md) | [Next](./exercises.md)
