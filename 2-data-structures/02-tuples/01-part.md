# Tuples Part 1 — Immutability and Unpacking

Tuples are immutable, ordered sequences. Use them for fixed records and safe unpacking.

## 1. Creating tuples

```python
empty = ()
one = (1,)          # trailing comma matters
pair = (1, 2)
no_parens = 1, 2    # packing without parens
from_list = tuple([1, 2, 3])
print(empty, one, pair, no_parens, from_list)
print(len(pair), pair[0], pair[-1])
```

```text
() (1,) (1, 2) (1, 2) (1, 2, 3)
2 1 2
```

| Form | Meaning |
|---|---|
| `()` | Empty tuple |
| `(1,)` | One-element tuple |
| `(1, 2)` | Normal tuple |
| `1, 2` | Packed, same as `(1, 2)` |
| `tuple(it)` | Convert iterable |

> `(1)` is just `1` in parens. Write `(1,)` for a real 1-tuple.

---

## 2. Immutability

```python
point = (10, 20)
print(point[0])
try:
    point[0] = 99
except TypeError as e:
    print("TypeError:", e)
```

```bash
python tuple_immutable.py
```

```text
10
TypeError: 'tuple' object does not support item assignment
```

You can rebind the name (`point = (1, 2)`) but cannot mutate items. Nested mutables inside are still mutable — avoid that.

---

## 3. Packing and unpacking

```python
name, age, city = ("ada", 36, "london")
print(name, age, city)
first, *rest = (1, 2, 3, 4)
print(first, rest)
a, b = (10, 20)
print(a, b)
```

```text
ada 36 london
1 [2, 3, 4]
10 20
```

Count must match unless you use `*rest`.

---

## 4. Swap idiom and named access

```python
a, b = 1, 2
a, b = b, a
print(a, b)

from collections import namedtuple
Point = namedtuple("Point", ["x", "y"])
p = Point(3, 4)
print(p, p.x, p.y)
print(p[0], p._asdict())
```

```text
2 1
Point(x=3, y=4) 3 4
3 {'x': 3, 'y': 4}
```

Prefer `namedtuple` over magic indexes when a tuple has meaning.

---

## 5. Tuple methods

```python
t = (1, 2, 2, 3)
print(t.count(2), t.index(3))
print(2 in t, 9 in t)
```

```text
2 3
True False
```

Only `count` and `index` — tuples stay small and simple.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `(1)` not a tuple | `type` is `int` | Write `(1,)` |
| Unpack arity mismatch | `ValueError` | Match count or use `*rest` |
| Assign to `t[0]` | `TypeError` | Rebuild tuple or use list |
| Mutable inside tuple | Hidden mutation | Keep tuples flat/immutable |
| Overusing indexes `p[0]` | Unreadable | Use `namedtuple` |

## Checklist

- [ ] Create empty, 1-tuple, packed forms
- [ ] Explain immutability and show `TypeError`
- [ ] Unpack with exact and starred targets
- [ ] Swap with `a, b = b, a`
- [ ] Access by name with `namedtuple`

#### [Previous](../01-lists/exercises.md) | [Next](02-part.md)
