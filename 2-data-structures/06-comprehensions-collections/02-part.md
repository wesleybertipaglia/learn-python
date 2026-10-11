# Collections Part 2 — deque, Counter, defaultdict

Three stdlib tools that replace hand-rolled loops: fast queues, counting, and auto-dicts.

## 1. deque as a queue (FIFO)

```python
from collections import deque
q = deque(["a", "b"])
q.append("c")       # right end
q.appendleft("z")   # left end
print(list(q))
print(q.popleft())  # FIFO take
print(q.pop())      # LIFO take
print(list(q))
```

```text
['z', 'a', 'b', 'c']
z
c
['a', 'b']
```

| deque op | List equivalent | Speed |
|---|---|---|
| `popleft()` | `pop(0)` | O(1) vs O(n) |
| `appendleft(x)` | `insert(0, x)` | O(1) vs O(n) |
| `append/pop` | Same | O(1) both |

```bash
python deque_queue.py
```

> Rule: stack → `list`; queue → `deque`. Never `list.pop(0)` in a loop.

---

## 2. Counter

```python
from collections import Counter
votes = ["a", "b", "a", "c", "a", "b"]
c = Counter(votes)
print(c)
print(c.most_common(2))
print(c["a"], c["missing"])
print(sum(c.values()))
```

```text
Counter({'a': 3, 'b': 2, 'c': 1})
[('a', 3), ('b', 2)]
3 0
6
```

Missing keys return `0`, never `KeyError`. Great for frequencies and top-N.

---

## 3. defaultdict

```python
from collections import defaultdict
groups = defaultdict(list)
for name, lang in [("ada", "py"), ("grace", "c"), ("linus", "py")]:
    groups[lang].append(name)
print(dict(groups))

scores = defaultdict(int)
for w in ["a", "b", "a"]:
    scores[w] += 1
print(dict(scores))
```

```text
{'py': ['ada', 'linus'], 'c': ['grace']}
{'a': 2, 'b': 1}
```

| Factory | Default | Use |
|---|---|---|
| `list` | `[]` | Grouping |
| `int` | `0` | Counting |
| `set` | `set()` | Dedup groups |
| `dict` | `{}` | Nested dicts |

---

## 4. Choosing the right tool

```python
from collections import Counter, defaultdict, deque
text = "red blue red"
print(Counter(text.split()))
print(Counter("abracadabra").most_common(3))
```

```text
Counter({'red': 2, 'blue': 1})
[('a', 5), ('b', 2), ('r', 2)]
```

| Task | Tool | One-liner |
|---|---|---|
| FIFO queue | `deque` | `q.popleft()` |
| Top-N freq | `Counter` | `.most_common(n)` |
| Group by key | `defaultdict(list)` | `d[k].append(v)` |
| Index by id | `dict comp` | `{u["id"]: u for u in rows}` |

---

## 5. Small combined example

```python
from collections import Counter, deque
queue = deque(["task1", "task2", "task1"])
done = Counter()
while queue:
    job = queue.popleft()
    done[job] += 1
print(dict(done))
```

```text
{'task1': 2, 'task2': 1}
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `list.pop(0)` queue | Slow on large n | `deque.popleft()` |
| `Counter` missing key check | Unneeded `if` | Just read it — default `0` |
| `defaultdict` shared default myth | Surprise | Factory called per key — safe |
| `dict(d)` needed for print | `defaultdict(...)` repr | Convert for clean output |
| Using `deque` as stack only | Overkill | Plain `list` is fine for LIFO |

## Checklist

- [ ] Run FIFO queue with `append` / `popleft`
- [ ] Count with `Counter` + `most_common`
- [ ] Group with `defaultdict(list)`
- [ ] Pick `list` vs `deque` vs `Counter` correctly
- [ ] Convert to `dict` for display

#### [Previous](01-part.md) | [Next](exercises.md)
