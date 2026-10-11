# Lists Part 1 — Creation, Indexing, and Mutation

Lists are Python's workhorse sequence: ordered, mutable, and able to hold mixed types.

## 1. Creating lists

```python
empty = []
scores = [10, 20, 30]
mixed = [1, "two", 3.0, True]
from_range = list(range(5))
chars = list("abc")
print(empty, scores, mixed)
print(from_range, chars)
```

```text
[] [10, 20, 30] [1, 'two', 3.0, True]
[0, 1, 2, 3, 4] ['a', 'b', 'c']
```

| Constructor | Use when | Example |
|---|---|---|
| `[]` | Literal, fastest | `[1, 2]` |
| `list()` | Convert iterable | `list("hi")` |
| `list(range(n))` | Number sequence | `list(range(3))` |
| `[0] * n` | Fixed-size placeholder | `[0] * 4` |

> Never use `[[]] * 3` to make a matrix — all rows share one object. Use a comprehension instead.

---

## 2. Indexing and len

```python
names = ["ada", "grace", "linus"]
print(names[0], names[-1], len(names))
print(names[3 - 3])
```

```bash
python lists_index.py
```

```text
ada linus 3
ada
```

Negative index `-1` is the last item. Index `len(list)` raises `IndexError`.

---

## 3. Slicing

Syntax: `lst[start:stop:step]`. `stop` is excluded.

```python
nums = [0, 1, 2, 3, 4, 5]
print(nums[1:4])
print(nums[:3])
print(nums[3:])
print(nums[::-1])
print(nums[::2])
```

```text
[1, 2, 3]
[0, 1, 2]
[3, 4, 5]
[5, 4, 3, 2, 1, 0]
[0, 2, 4]
```

Slicing returns a new list. Use it to copy: `copy = nums[:]`.

---

## 4. Core mutating methods

```python
todo = ["write"]
todo.append("test")
todo.extend(["lint", "commit"])
todo.insert(1, "review")
print(todo)
todo.remove("lint")
print(todo.pop())
print(todo)
```

```text
['write', 'review', 'test', 'lint', 'commit']
commit
['write', 'review', 'test']
```

| Method | What it does | Returns |
|---|---|---|
| `append(x)` | Add one item | `None` |
| `extend(iter)` | Add each item | `None` |
| `insert(i, x)` | Insert at index | `None` |
| `remove(x)` | Delete first match | `None` |
| `pop(i=-1)` | Remove + return | item |

> `append` adds one object; `extend` iterates. `lst.append([1,2])` nests, `lst.extend([1,2])` flattens one level.

---

## 5. Aliasing vs copy

```python
a = [1, 2, 3]
b = a          # alias: same object
c = a.copy()   # shallow copy
d = a[:]       # shallow copy via slice
a.append(99)
print(a, b, c, d)
print(a is b, a is c)
```

```text
[1, 2, 3, 99] [1, 2, 3, 99] [1, 2, 3] [1, 2, 3]
True False
```

Use `copy.deepcopy()` from `copy` only when nested mutables must be independent.

```python
import copy
m = [[1], [2]]
shallow = m.copy()
deep = copy.deepcopy(m)
m[0].append(9)
print(shallow, deep)
```

```text
[[1, 9], [2]] [[1], [2]]
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `IndexError` on `lst[len(lst)]` | Crash at last index | Use `lst[-1]` or `len(lst)-1` |
| `append` vs `extend` mix-up | Nested `[..., [1,2]]` | Use `extend` for iterables |
| `remove(x)` missing value | `ValueError` | Check `x in lst` first |
| Alias mutation `b = a` | Both change unexpectedly | Use `a.copy()` or `a[:]` |
| `[[0]*3]*3` matrix | Whole column changes | Use `[[0]*3 for _ in range(3)]` |

## Checklist

- [ ] Create lists with `[]`, `list()`, `list(range())`
- [ ] Read with positive and negative indexes
- [ ] Slice with `start:stop:step` and reverse with `[::-1]`
- [ ] Use `append`, `extend`, `insert`, `remove`, `pop` correctly
- [ ] Explain aliasing vs `copy()` and demonstrate `is`

#### [Previous](../../1-fundamentals/challenge.md) | [Next](02-part.md)
