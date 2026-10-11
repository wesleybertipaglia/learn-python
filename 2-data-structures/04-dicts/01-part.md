# Dicts Part 1 — Creation, Access, and Views

Dicts map hashable keys to values. They are ordered by insertion since Python 3.7.

## 1. Creating dicts

```python
empty = {}
user = {"name": "ada", "age": 36}
from_pairs = dict([("a", 1), ("b", 2)])
from_kwargs = dict(name="grace", age=85)
print(empty, user, from_pairs, from_kwargs)
```

```text
{} {'name': 'ada', 'age': 36} {'a': 1, 'b': 2} {'name': 'grace', 'age': 85}
```

| Style | Best for |
|---|---|
| `{"k": v}` | Literals, most code |
| `dict(pairs)` | Converting pair lists |
| `dict(k=v)` | String keys, quick build |
| `{k: v for ...}` | Computed (see comprehensions) |

> Keys must be hashable: `str`, `int`, `tuple` work; `list` and `dict` do not.

---

## 2. Access with [] vs .get()

```python
user = {"name": "ada", "age": 36}
print(user["name"])
print(user.get("age"))
print(user.get("city"))
print(user.get("city", "unknown"))
try:
    print(user["city"])
except KeyError as e:
    print("KeyError:", e)
```

```bash
python dict_get.py
```

```text
ada
36
None
unknown
KeyError: 'city'
```

Rule: `[]` when key must exist, `.get()` with default when optional.

---

## 3. keys / values / items

```python
user = {"name": "ada", "age": 36, "city": "london"}
print(list(user.keys()))
print(list(user.values()))
print(list(user.items()))
for k, v in user.items():
    print(f"{k}={v}")
```

```text
['name', 'age', 'city']
['ada', 36, 'london']
[('name', 'ada'), ('age', 36), ('city', 'london')]
name=ada
age=36
city=london
```

Views update live — `user["x"] = 1` appears in an existing `keys()` view.

---

## 4. Membership with in

```python
user = {"name": "ada", "age": 36}
print("name" in user, "city" in user)
print("city" not in user)
if "city" not in user:
    user["city"] = "london"
print(user)
```

```text
True False
True
{'name': 'ada', 'age': 36, 'city': 'london'}
```

`in` checks keys only. For values use `v in d.values()`.

---

## 5. Idiomatic loops

```python
scores = {"ada": 95, "grace": 99}
for name in scores:
    print(name, scores[name])
for name, score in scores.items():
    print(f"{name}: {score}")
```

```text
ada 95
grace 99
ada: 95
grace: 99
```

Prefer `.items()` with unpacking over indexing inside the loop.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `d["missing"]` | `KeyError` | Use `.get()` or `in` check |
| Mutable key `d[[1]]` | `TypeError: unhashable` | Use tuple |
| `d.get(k)` returns `None` | `None` propagates | Pass default: `.get(k, 0)` |
| Iterating + deleting | `RuntimeError` | Loop over `list(d)` |
| Assuming order in <3.7 | Flaky old code | Require 3.12+, order guaranteed |

## Checklist

- [ ] Create dicts 3 ways
- [ ] Choose `[]` vs `.get(default)` correctly
- [ ] Loop with `.keys()`, `.values()`, `.items()`
- [ ] Test membership with `in` on keys
- [ ] Unpack `for k, v in d.items()`

#### [Previous](../03-strings/exercises.md) | [Next](02-part.md)
