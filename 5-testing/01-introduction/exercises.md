# Exercises — Hand-Rolled Asserts

No pytest yet. Write plain `assert` checks, run with `python`.

> Hand-rolled asserts are the gym before the sport: painful on purpose, so pytest feels like relief.
---
## 1. Total with asserts

```python
def total(prices):
    return sum(prices)
```

Write `check_total` asserting:

| Input | Expected |
|---|---|
| `[10, 20]` | `30` |
| `[]` | `0` |
| `[2.5, 2.5]` | `5.0` |

```bash
python check_total.py
```

```text
check_total passed
```
---
## 2. Slug edges

```python
def slug(title):
    return title.strip().lower().replace(" ", "-")
```

Assert normal + two edges (empty, whitespace).
---
## 3. Reject invalid age

```python
def set_age(age):
    if not isinstance(age, int) or isinstance(age, bool):
        raise TypeError("age must be int")
    if age < 0:
        raise ValueError("age must be >= 0")
    return age
```

Check `set_age(0) == 0`, `set_age(-1)` → `ValueError`, `set_age("20")` → `TypeError` via `try/except/else`.
---
## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Missing `else` branch | Invalid input silently passes | `else: raise AssertionError(...)` |
| Float `==` | `0.1 + 0.2` fails | Use `abs(a - b) < 1e-9` |
| Happy-path only | Edge crash in prod | Add empty/zero/invalid rows |

---
## Checklist

- [ ] All three run with plain `python <file>.py`
- [ ] Each check prints `passed` on success
- [ ] Invalid inputs use `try/except/else`
---
#### [Previous](02-part.md) | [Next](../02-pytest/01-part.md)
