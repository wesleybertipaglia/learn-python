# Loops — enumerate, zip, for-else, Nested Loops

Part 1 loops count and repeat. This part loops over real data like a Python dev.

## `enumerate()`: Index + Value Together

The beginner version tracks the index by hand. Do not:

```python
names = ["ada", "grace", "linus"]

# Clunky — manual counter
i = 0
for name in names:
    print(i, name)
    i += 1
```

```python
# Pythonic — enumerate does it for you
for i, name in enumerate(names):
    print(i, name)
```

```text
0 ada
1 grace
2 linus
```

Need 1-based numbering for display? `enumerate(names, start=1)`. The data stays 0-based; only the label changes.

> If you catch yourself writing `for i in range(len(items))`, stop — you almost always want `enumerate(items)` instead. It is shorter and impossible to get the bounds wrong.

---

## `zip()`: Walk Two Lists in Lockstep

```python
names = ["ada", "grace", "linus"]
scores = [95, 88, 72]

for name, score in zip(names, scores):
    print(f"{name}: {score}")
```

```text
ada: 95
grace: 88
linus: 72
```

| Function | Gives you | Stops when |
|---|---|---|
| `enumerate(items)` | `(index, item)` pairs | Items run out |
| `zip(a, b)` | `(item_a, item_b)` pairs | **Shorter** list runs out |

```python
print(list(zip([1, 2, 3], ["a", "b"])))  # third element dropped!
```

```text
[(1, 'a'), (2, 'b')]
```

`zip` silently truncates to the shorter input. If the lists should be equal length, assert it or the missing data fails silently — one of `zip`'s few sharp edges.

---

## `for-else`: The Feature Nobody Expects

`else` on a loop runs **only if the loop finished without `break`**:

```python
for n in [4, 6, 8]:
    if n % 2 == 1:
        print(f"found odd: {n}")
        break
else:
    print("no odd numbers found")
```

```text
no odd numbers found
```

```python
for n in [4, 7, 8]:
    if n % 2 == 1:
        print(f"found odd: {n}")
        break
else:
    print("no odd numbers found")
```

```text
found odd: 7
```

Use it for searches: `break` = found it, `else` = not there. Same works on `while-else`. Confusing at first, genuinely useful for "find or report missing" logic.

---

## Nested Loops: One Loop Per Dimension

```python
for row in range(1, 4):
    for col in range(1, 4):
        print(f"{row}x{col}={row * col}", end="  ")
    print()
```

```text
1x1=1  1x2=2  1x3=3
2x1=2  2x2=4  2x3=6
3x1=3  3x2=6  3x3=9
```

The inner loop runs **to completion for every single outer iteration**. Cost multiplies: 3 rows × 3 cols = 9 prints. Keep nesting shallow — two levels is normal, three is a smell, four means rethink the data structure.

`break` only exits its own level:

```python
for row in range(3):
    for col in range(3):
        if col == 1:
            break
        print(row, col)
```

```text
0 0
1 0
2 0
```

To exit both levels, put the loops in a function and `return` (lesson 07).

---

## Bugs You Will Write

| Bug | Symptom | Fix |
|---|---|---|
| `range(len(x))` + `x[i]` everywhere | Works but noisy, easy to mis-index | Use `enumerate(x)` |
| `zip` drops data silently | Missing rows, no error | Verify equal lengths first |
| `else` always/never runs | Misunderstood `for-else` | `else` = "no `break` happened" |
| 3+ nested loops | Slow, unreadable | Extract inner loop into a function |

---

## Checklist

- [ ] I use `enumerate()` instead of `range(len(...))`
- [ ] I can `zip()` two lists and know it truncates to the shorter
- [ ] I can explain when `for-else`'s `else` runs
- [ ] My nested loops are at most 2 deep, and `break` scope does not surprise me

---
#### [Previous](01-part.md) | [Next](exercises.md)
