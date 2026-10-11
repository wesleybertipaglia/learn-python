# Dicts Part 2 — Update, Merge, Delete, and Records

Mutate safely, merge with `|`, delete with `pop`/`del`, and model rows as dicts.

## 1. update() and merge with |

```python
a = {"x": 1, "y": 2}
b = {"y": 20, "z": 30}
merged = a | b
print(merged, a)
a.update(b)
print(a)
c = {**a, "w": 0}
print(c)
```

```text
{'x': 1, 'y': 20, 'z': 30} {'x': 1, 'y': 2}
{'x': 1, 'y': 20, 'z': 30}
{'x': 1, 'y': 20, 'z': 30, 'w': 0}
```

| Form | Mutates? | Version |
|---|---|---|
| `a.update(b)` | Yes, in place | All 3.x |
| `a \| b` | No, new dict | 3.9+ |
| `{**a, **b}` | No, new dict | 3.5+ |

> `a | b`: right side wins on conflicts. Prefer it for pure merges.

---

## 2. pop / del / clear

```python
d = {"a": 1, "b": 2, "c": 3}
print(d.pop("b"))
print(d.pop("missing", None))
del d["a"]
print(d)
d.clear()
print(d)
```

```bash
python dict_delete.py
```

```text
2
None
{'c': 3}
{}
```

Use `pop(k, default)` when absence is fine; `del` when it must exist.

---

## 3. Dict of records pattern

```python
users = [
    {"id": 1, "name": "ada", "role": "admin"},
    {"id": 2, "name": "grace", "role": "user"},
]
by_id = {u["id"]: u for u in users}
print(by_id[1]["name"])
for u in users:
    print(f"{u['id']}: {u['name']} ({u['role']})")
```

```text
ada
1: ada (admin)
2: grace (user)
```

Store rows as a list of dicts; index with `{row["id"]: row}` for O(1) lookup.

---

## 4. KeyError handling

```python
config = {"host": "localhost"}

try:
    port = config["port"]
except KeyError:
    port = 8000
print(port)

port2 = config.get("port", 8000)
print(port2)

from collections import defaultdict
counts = defaultdict(int)
for w in ["a", "b", "a"]:
    counts[w] += 1
print(dict(counts))
```

```text
8000
8000
{'a': 2, 'b': 1}
```

| Strategy | Code |
|---|---|
| Default value | `d.get(k, default)` |
| Try/except | `try: d[k] except KeyError:` |
| Pre-check | `if k in d:` |
| Auto-vivify | `defaultdict` / `setdefault` |

---

## 5. setdefault and counting

```python
d = {}
d.setdefault("tags", []).append("python")
print(d)
text = "a b a c b a"
freq = {}
for w in text.split():
    freq[w] = freq.get(w, 0) + 1
print(freq)
```

```text
{'tags': ['python']}
{'a': 3, 'b': 2, 'c': 1}
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `d[k]` without check | `KeyError` | `.get()`, `in`, or `try/except` |
| `update` returns `None` | `d = d.update(b)` wipes dict | Call bare `d.update(b)` |
| `del d[k]` missing | `KeyError` | `d.pop(k, None)` |
| Mutating while iterating | `RuntimeError` | Iterate `list(d.items())` |
| Shallow merge shares values | Nested mutation leaks | `copy.deepcopy` for nested |

## Checklist

- [ ] Merge with `update`, `|`, `{**a, **b}`
- [ ] Delete with `pop(default)`, `del`, `clear`
- [ ] Build list-of-dicts and `by_id` index
- [ ] Handle `KeyError` three ways
- [ ] Count with `.get(k, 0)` / `setdefault`

#### [Previous](01-part.md) | [Next](exercises.md)
