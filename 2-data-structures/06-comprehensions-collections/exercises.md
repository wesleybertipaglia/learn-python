# Comprehensions and Collections — Exercises

Practice one-liners, generators, and stdlib helpers.

## Setup

```bash
mkdir -p comp_ex && cd comp_ex
python --version
```

```python
print([x for x in range(3)])
```

```text
[0, 1, 2]
```

| Exercise | Focus | Hint |
|---|---|---|
| 1 | List comp | `if` filter |
| 2 | Dict comp | `enumerate` |
| 3 | Generator | `sum(...)` |
| 4 | Counter | `most_common` |
| 5 | deque | `popleft` |

> If a comprehension needs 3 loops or nested `if/else`, rewrite it as a plain `for`.

---

## 1. Squares and evens
Build `[x*x for x in range(10)]`, then filter evens only. Print both. Do it once as list comp, once as generator fed to `sum`.

## 2. Length map
From `["ada","grace","linus"]` build `{name: len(name)}` and `{i: name}` with `enumerate(start=1)`. Print both dicts.

## 3. Lazy sum
Compute `sum(x*x for x in range(1000) if x % 2 == 0)` without building a list. Print result and explain memory win.

## 4. Top words
Use `Counter("red blue red green blue red".split())` to print counts and `most_common(2)`. Look up a missing word to show `0`.

## 5. Ticket queue
Implement FIFO with `deque`: `append` 3 customers, `popleft` 2, print remaining and order served. Compare with `list.pop(0)` comment.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Filter misplaced | `SyntaxError` | `[... for x in it if c]` |
| List for huge data | High memory | Generator `(...)` |
| `Counter` manual count | Verbose loop | `Counter(it)` |
| `pop(0)` queue | O(n) slowdown | `deque.popleft()` |
| Mega one-liner | Unreadable | Split into loop |

## Checklist

- [ ] Comps have transform + filter
- [ ] Dict comps use `enumerate` once
- [ ] Generator avoids temp list
- [ ] `Counter` shows top-2
- [ ] `deque` serves FIFO order

#### [Previous](02-part.md) | [Next](../challenge.md)
