# Dicts — Exercises

Practice access, merging, records, and safe lookups.

## Setup

```bash
mkdir -p dicts_ex && cd dicts_ex
python --version
```

```python
print({"a": 1}.get("b", 99))
```

```text
99
```

| Exercise | Focus | Hint |
|---|---|---|
| 1 | `.get()` | Default value |
| 2 | Views | `.items()` |
| 3 | Merge | `a \| b` |
| 4 | Records | `by_id` index |
| 5 | Counting | `.get(w, 0)` |

> Never index a possibly-missing key with `[]` — use `.get()`, `in`, or `try/except`.

---

## 1. Safe lookup
Given `user = {"name": "ada"}`, print `age` with default `30` via `.get()`. Then try `user["age"]` in `try/except KeyError` and print fallback.

## 2. Gradebook loop
Given `{"ada": 95, "grace": 99, "linus": 88}`, print each `name: score` via `.items()`, then print keys and values lists.

## 3. Merge configs
Merge `{"host": "a", "port": 80}` with `{"port": 8000, "debug": True}` using `|`. Show originals unchanged, then `update` in place.

## 4. Record index
From `[{"id":1,"name":"ada"},{"id":2,"name":"grace"}]` build `by_id`, look up id `2`, and print all names with a loop.

## 5. Word counter
Count words in `"red blue red green blue red"` with `freq[w] = freq.get(w, 0) + 1`. Print dict and most common word with `max(freq, key=freq.get)`.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Bare `d[k]` | `KeyError` | `.get(k, default)` |
| `d.update` assignment | `None` | Don't assign result |
| Missing `strip` on keys | Lookup miss | Normalize keys |
| Deleting while looping | `RuntimeError` | Loop over `list(d)` |
| Unhashable key | `TypeError` | Use str/tuple |

## Checklist

- [ ] Safe lookups never crash
- [ ] Loops use `.items()` unpacking
- [ ] Merges show `|` vs `update`
- [ ] Record index answers in O(1)
- [ ] Counter finds correct top word

#### [Previous](02-part.md) | [Next](../05-sets/01-part.md)
