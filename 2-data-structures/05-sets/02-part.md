# Sets Part 2 — Operations, Dedup, and Choices

Combine sets with math-like operators and pick set vs list on purpose.

## 1. Union, intersection, difference

```python
a = {1, 2, 3}
b = {3, 4, 5}
print(a | b)  # union
print(a & b)  # intersection
print(a - b)  # difference
print(a ^ b)  # symmetric difference
print(a.union(b), a.intersection(b))
```

```text
{1, 2, 3, 4, 5}
{3}
{1, 2}
{1, 2, 4, 5}
{1, 2, 3, 4, 5} {3}
```

| Op | Method | Meaning |
|---|---|---|
| `\|` | `union` | In either |
| `&` | `intersection` | In both |
| `-` | `difference` | In `a` only |
| `^` | `symmetric_difference` | In exactly one |

```bash
python set_ops.py
```

> Methods accept any iterable; operators require sets on both sides.

---

## 2. Subset and disjoint tests

```python
print({1, 2} <= {1, 2, 3})
print({1, 2}.issubset({1, 2, 3}))
print({1, 9}.isdisjoint({2, 3}))
print({1, 2, 3} >= {2})
```

```text
True
True
True
True
```

---

## 3. Dedup pattern

```python
raw = ["ada", "grace", "ada", "linus", "grace"]
unique = list(dict.fromkeys(raw))  # keeps order
print(unique)
print(sorted(set(raw)))  # sorted, loses input order
seen = set()
deduped = []
for name in raw:
    if name not in seen:
        seen.add(name)
        deduped.append(name)
print(deduped)
```

```text
['ada', 'grace', 'linus']
['ada', 'grace', 'linus']
['ada', 'grace', 'linus']
```

| Need | Use |
|---|---|
| Unique + keep order | `dict.fromkeys` |
| Unique + sorted | `sorted(set(...))` |
| Unique + filter logic | `seen` set loop |

---

## 4. When set vs list

```python
visitors = ["a", "b", "a"]
print(set(visitors))  # uniqueness wanted -> set
queue = ["first", "second"]  # order matters -> list
print(queue[0])
```

```text
{'a', 'b'}
first
```

| Use set | Use list |
|---|---|
| Membership tests | Order/index matters |
| Dedup | Duplicates allowed |
| Math ops (∩/∪) | Stack/queue behavior |
| Small unique tags | Sequence of events |

---

## 5. frozenset preview

```python
fs = frozenset([1, 2, 3])
print(fs, len(fs), 2 in fs)
d = {fs: "immutable key"}
print(d[fs])
try:
    fs.add(4)
except AttributeError as e:
    print("AttributeError:", e)
```

```text
frozenset({1, 2, 3}) 3 True
immutable key
AttributeError: 'frozenset' object has no attribute 'add'
```

Use `frozenset` as a dict key or set member when you need an immutable set.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `set \| list` | `TypeError` | Convert: `s \| set(lst)` or `s.union(lst)` |
| Relying on order | Random-looking output | `sorted(s)` |
| `pop()` assumed first | Arbitrary item gone | `pop()` is arbitrary; use list for order |
| Dedup loses order | Wrong sequence | `dict.fromkeys` instead |
| Mutating during iteration | `RuntimeError` | Iterate `s.copy()` |

## Checklist

- [ ] Apply `\|`, `&`, `-`, `^` correctly
- [ ] Test subsets with `<=` / `issubset`
- [ ] Dedup with order preserved
- [ ] Choose set vs list with reasons
- [ ] Use `frozenset` as immutable key

#### [Previous](01-part.md) | [Next](exercises.md)
