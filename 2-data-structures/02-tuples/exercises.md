# Tuples — Exercises

Lock in immutability, unpacking, and hashability.

## Setup

```bash
mkdir -p tuples_ex && cd tuples_ex
python --version
```

```python
print((1, 2)[1])
```

```text
2
```

| Exercise | Focus | Hint |
|---|---|---|
| 1 | 1-tuple | Trailing comma |
| 2 | Swap | `a, b = b, a` |
| 3 | Unpack | `*rest` |
| 4 | Dict keys | Hashable only |
| 5 | namedtuple | `_asdict()` |

> Tuples are immutable — rebuild them instead of assigning to indexes.

---

## 1. One-tuple trap
Print `type((1))` and `type((1,))`. Fix a function that should return a 1-tuple but returns an int.

## 2. Swap without temp
Given `a = 5, b = 9`, swap in one line and print. Then rotate three vars `x, y, z` left in one line.

## 3. Starred unpack
Unpack `(10, 20, 30, 40, 50)` into `first, *middle, last`. Print all three. Ignore middle with `*_` in a second version.

## 4. Coordinate map
Build `{(0,0): "start", (1,2): "coin"}`. Look up `(1,2)`, test membership for `(9,9)`, and show why `{[1,2]: "x"}` fails with `try/except TypeError`.

## 5. Named record
Create `namedtuple("User", ["name", "age"])`, make two users, sort by `age`, print `._asdict()` for each.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Missing comma | `int` not `tuple` | Add trailing `,` |
| Bad unpack count | `ValueError` | Add `*rest` |
| `t[0] = x` | `TypeError` | Build new tuple |
| Unhashable key | `TypeError` | Use tuple, not list |
| Magic indexes | Confusing `u[1]` | Use `namedtuple` |

## Checklist

- [ ] 1-tuple vs parenthesized int is clear
- [ ] Swap and rotate in one line each
- [ ] Starred unpack works
- [ ] Dict with tuple keys runs
- [ ] `namedtuple` sorts and prints

#### [Previous](02-part.md) | [Next](../03-strings/01-part.md)
