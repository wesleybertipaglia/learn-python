# Tuples Part 2 — Dict Keys, Conventions, and Choices

Tuples hash, so they unlock dict keys and set members. Know when to pick them.

## 1. Tuples as dict keys

```python
capitals = {("fr", "paris"): 2_100_000, ("it", "rome"): 2_800_000}
print(capitals[("fr", "paris")])
coords = {(10, 20): "treasure", (0, 0): "start"}
print(coords[(10, 20)])
print(("fr", "paris") in capitals)
```

```text
2100000
treasure
True
```

```bash
python tuple_keys.py
```

| Key type | Hashable? | Usable as dict key? |
|---|---|---|
| `tuple` of str/int | Yes | Yes |
| `str`, `int` | Yes | Yes |
| `list` | No | No — `TypeError` |
| `tuple` with list inside | No | No |

> Lists crash as keys: `{[1]: 2}` raises `TypeError: unhashable type: 'list'`. Use a tuple instead.

---

## 2. The `_` convention

```python
pair = (200, "OK")
status, _ = pair
print(status)

first, *_ = (1, 2, 3, 4)
print(first)

for i, _val in enumerate(["a", "b"]):
    print(i)
```

```text
200
1
0
1
```

Use `_` for throwaway values. `*_` collects the ignored tail.

---

## 3. When tuple vs list

```python
rgb = (255, 0, 0)       # fixed record -> tuple
history = ["/", "/home"]  # growing log -> list
print(rgb, history)
```

```text
(255, 0, 0) ['/', '/home']
```

| Use tuple | Use list |
|---|---|
| Fixed size, mixed types | Variable length, same-type items |
| Dict key / set member | Needs `append`/`sort`/`pop` |
| Return `(ok, value)` | Accumulate results |
| Public constant | Internal buffer |

```python
def divide(a, b):
    if b == 0:
        return (False, None)
    return (True, a / b)

ok, value = divide(10, 2)
print(ok, value)
```

```text
True 5.0
```

---

## 4. Sorting and returning tuples

```python
pairs = [(2, "b"), (1, "a"), (2, "a")]
print(sorted(pairs))
users = [("ada", 36), ("grace", 85)]
users.sort(key=lambda u: u[1])
print(users)
```

```text
[(1, 'a'), (2, 'a'), (2, 'b')]
[('ada', 36), ('grace', 85)]
```

Tuples compare element-wise — handy for multi-key sorts.

---

## 5. Conversions

```python
t = (1, 2, 3)
lst = list(t)
print(lst, tuple(lst))
a, b, c = t
print(a + b + c)
```

```text
[1, 2, 3] (1, 2, 3)
6
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| List as dict key | `TypeError: unhashable` | Convert to `tuple(key)` |
| Tuple with list as key | Same `TypeError` | Keep keys flat |
| Forgetting `_` meaning | Unused var lint | Name it `_` or `_unused` |
| Using list for fixed record | Accidental mutation | Switch to tuple/`namedtuple` |
| Comparing mixed tuples | `TypeError` | Keep element types consistent |

## Checklist

- [ ] Use tuples as dict keys and explain hashability
- [ ] Apply `_` and `*_` for ignored values
- [ ] Choose tuple vs list with reasons
- [ ] Sort lists of tuples
- [ ] Return and unpack `(ok, value)` pairs

#### [Previous](01-part.md) | [Next](exercises.md)
