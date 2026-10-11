# Lists — Exercises

Practice creation, slicing, methods, copies, and stacks.

## Setup

```bash
mkdir -p lists_ex && cd lists_ex
python --version  # need 3.12+
```

```python
print([1, 2, 3][::-1])
```

```text
[3, 2, 1]
```

| Exercise | Focus | Hint |
|---|---|---|
| 1 | Slicing | `stop` excluded |
| 2 | Methods | `extend` vs `append` |
| 3 | Copy | `is` vs `==` |
| 4 | Sort | `key=len` |
| 5 | Stack | `append`/`pop` |

> Run each script with `python file.py` and compare real output before moving on.

---

## 1. Slice playground
Given `nums = list(range(10))`, print first 3, last 3, every 2nd, reversed.

## 2. Todo manager
From `[]`: `append` 2 tasks, `extend` 2 more, `insert` one at 1, `remove` one, `pop` last and print it plus final list.

## 3. Alias detective
Create `a = [1, [2]]`, `b = a`, `c = a.copy()`. Append to `a`, mutate `a[1]`. Print all three plus `is` checks.

## 4. Sort by length
Sort `["pear", "fig", "pineapple", "kiwi"]` by length with `sorted()`, then `.sort(reverse=True)`. Print both.

## 5. Bracket checker (stack)
Use a list stack to check `"{[()]}"` vs `"{[(])}"`. Push openers, pop on closers, print `True`/`False`.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Off-by-one slice | Missing item | Remember `stop` excluded |
| `append` vs `extend` | Nested list | Pick the right method |
| `sort()` returns `None` | Data wiped | Call bare `data.sort()` |
| Aliasing surprise | Two vars change | Copy explicitly |
| Empty `pop()` | `IndexError` | Guard with `if stack:` |

## Checklist

- [ ] All 5 scripts run without errors
- [ ] No `range(len(...))` where `enumerate` fits
- [ ] Stack handles empty input
- [ ] Can explain shallow vs deep copy

#### [Previous](02-part.md) | [Next](../02-tuples/01-part.md)
