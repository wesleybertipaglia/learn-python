# Iterators and the For Loop

A `for` loop is syntactic sugar over the iterator protocol. Learn the protocol and lazy behavior clicks.

> Every `for` calls `iter()` once, then `next()` until `StopIteration`. That is the whole trick.

## Iterable vs Iterator

| Term | Has `__iter__` | Has `__next__` | Example |
|---|---|---|---|
| Iterable | Yes | No | `list`, `str`, `range` |
| Iterator | Yes | Yes | `iter([1, 2])`, generator |
| Exhausted iterator | Yes | Raises `StopIteration` | Used-up generator |

```python
nums = [10, 20, 30]  # iterable
it = iter(nums)       # iterator
print(next(it))
print(next(it))
print(next(it))
```

```text
10
20
30
```

One more `next(it)` raises `StopIteration`. A `for` loop catches that and stops cleanly.

```python
it = iter([1, 2])
for x in it:
    print(x)
print(list(it))  # already consumed -> []
```

```text
1
2
[]
```

Iterators are single-use. Iterables like `list` make a fresh iterator each loop.

## How `for` Really Works

This:

```python
for x in [1, 2, 3]:
    print(x)
```

is roughly this:

```python
it = iter([1, 2, 3])
while True:
    try:
        x = next(it)
    except StopIteration:
        break
    print(x)
```

```text
1
2
3
```

Prove it in the shell:

```bash
python -c "it = iter([1, 2]); print(next(it)); print(list(it))"
```

```text
1
[2]
```

Partial consumption matters: if you `next()` once before the loop, the loop starts from the second item.

## `range` and `enumerate` as Iterables

`range` is lazy. It does not build a list.

```python
r = range(1_000_000)
print(type(r))
print(len(r))
print(r[5])
print(list(range(3)))
```

```text
<class 'range'>
1000000
5
[0, 1, 2]
```

`enumerate` pairs index with value. Never manage a manual counter.

```python
names = ["ana", "bo", "mia"]
for i, name in enumerate(names, start=1):
    print(i, name)
```

```text
1 ana
2 bo
3 mia
```

| Helper | Returns | Use when |
|---|---|---|
| `range(n)` | Lazy ints | Repeat N times, index |
| `enumerate(xs)` | `(index, value)` | Need position |
| `zip(a, b)` | Pairs | Walk two lists together |
| `iter(xs)` | Iterator | Manual control |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Reusing an iterator | Second loop prints nothing | Loop over the list, or call `iter()` again |
| Manual index with `range(len(x))` | Off-by-one, ugly | Use `enumerate(x)` |
| Infinite `while` on iterator | Hangs | Catch `StopIteration` or use `for` |
| `zip` silently truncates | Missing tail items | Use `itertools.zip_longest` if needed |
| Assuming `range` is a list | Confusion in REPL | It is lazy; call `list()` only to inspect |

## Checklist

- [ ] I can explain iterable vs iterator
- [ ] I can manually drive `iter()` / `next()` to exhaustion
- [ ] I know why a second loop over an iterator is empty
- [ ] I use `enumerate` instead of manual counters
- [ ] I know `range` is lazy, not a list

#### [Previous](../../3-oop/challenge.md) | [Next](./02-part.md)
