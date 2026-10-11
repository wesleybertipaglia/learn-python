# Sets Part 1 — Creation, Uniqueness, and Speed

Sets store unique hashables with O(1) membership. Use them to dedup and test fast.

## 1. Creating sets

```python
empty = set()  # NOT {}
nums = {1, 2, 2, 3}
from_list = set([1, 2, 2, 3])
from_str = set("abca")
print(empty, nums, from_list, from_str)
print(type({}), type(set()))
```

```text
set() {1, 2, 3} {1, 2, 3} {'a', 'b', 'c'}
<class 'dict'> <class 'set'>
```

| Form | Result |
|---|---|
| `set()` | Empty set |
| `{1, 2}` | Populated set |
| `set(iter)` | Deduped from iterable |
| `{}` | Empty **dict**, not set |

> `{}` is a dict. Memorize it — this bites everyone once.

---

## 2. Uniqueness guarantee

```python
vals = [1, 2, 2, 3, 3, 3]
print(set(vals))
print(len(set(vals)), len(vals))
names = {"ada", "ada", "grace"}
print(names)
```

```bash
python set_unique.py
```

```text
{1, 2, 3}
3 6
{'ada', 'grace'}
```

Duplicates vanish silently. Order is not guaranteed — never index a set.

---

## 3. add / discard / remove

```python
s = {1, 2}
s.add(3)
s.add(2)
print(s)
s.discard(99)  # safe
print(s)
s.remove(1)
print(s)
try:
    s.remove(99)
except KeyError as e:
    print("KeyError:", e)
```

```text
{1, 2, 3}
{1, 2, 3}
{2, 3}
KeyError: 99
```

| Method | Missing element |
|---|---|
| `discard(x)` | Ignored |
| `remove(x)` | Raises `KeyError` |
| `pop()` | Removes arbitrary item |

Prefer `discard` when absence is normal.

---

## 4. Membership speed

```python
big_list = list(range(100_000))
big_set = set(big_list)
import time
t0 = time.perf_counter()
print(99_999 in big_list)
t1 = time.perf_counter()
print(99_999 in big_set)
t2 = time.perf_counter()
print(f"list: {t1-t0:.4f}s set: {t2-t1:.6f}s")
```

```text
True
True
list: 0.0011s set: 0.000001s
```

List `in` is O(n), set `in` is O(1). For repeated lookups, convert once to a set.

---

## 5. Set elements must hash

```python
print(hash("ada"), hash((1, 2)))
try:
    {[1, 2]}
except TypeError as e:
    print("TypeError:", e)
```

```text
-3001726922425978627 3713081631934410656
TypeError: unhashable type: 'list'
```

> Only hashables go in sets. Convert lists to tuples first: `{(tuple(x)) for x in rows}`.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `{}` for empty set | Gets a dict | Use `set()` |
| Indexing `s[0]` | `TypeError` | Convert: `list(s)[0]` or iterate |
| `remove` missing | `KeyError` | Use `discard` |
| Assuming order | Flaky tests | `sorted(s)` for stable output |
| Unhashable member | `TypeError` | Use tuple/frozenset |

## Checklist

- [ ] Create sets with `set()` and `{...}`
- [ ] Explain why `{}` is a dict
- [ ] Use `add`, `discard` vs `remove`
- [ ] Show O(1) membership vs list
- [ ] Enforce hashable members only

#### [Previous](../04-dicts/exercises.md) | [Next](02-part.md)
