# Comprehensions Part 1 — List, Dict, Set, and Generators

Comprehensions build collections in one readable line. Prefer them over manual loops.

## 1. List comprehensions

```python
squares = [x * x for x in range(6)]
print(squares)
names = ["ada", "grace"]
upper = [n.upper() for n in names]
print(upper)
doubled = [x * 2 for x in range(5) if x % 2 == 0]
print(doubled)
```

```text
[0, 1, 4, 9, 16, 25]
['ADA', 'GRACE']
[0, 4, 8]
```

| Part | Role | Example |
|---|---|---|
| `[expr` | Transform | `x*x` |
| `for x in it` | Source | `for x in range(6)` |
| `if cond]` | Filter | `if x % 2 == 0` |

```bash
python comp_list.py
```

> Read it aloud: "square of x for each x in range if x is even."

---

## 2. Conditional expressions

```python
labels = ["even" if x % 2 == 0 else "odd" for x in range(5)]
print(labels)
nums = [5, -1, 0, 7]
positives = [n for n in nums if n > 0]
print(positives)
```

```text
['even', 'odd', 'even', 'odd', 'even']
[5, 7]
```

Filter `if` goes after `for`; value-choice `if/else` goes before `for`.

---

## 3. Dict comprehensions

```python
names = ["ada", "grace", "linus"]
lengths = {n: len(n) for n in names}
print(lengths)
by_id = {i: n for i, n in enumerate(names, start=1)}
print(by_id)
evens = {x: x * x for x in range(6) if x % 2 == 0}
print(evens)
```

```text
{'ada': 3, 'grace': 5, 'linus': 5}
{1: 'ada', 2: 'grace', 3: 'linus'}
{0: 0, 2: 4, 4: 16}
```

---

## 4. Set comprehensions

```python
text = "abracadabra"
print({ch for ch in text})
print({x % 3 for x in range(9)})
```

```text
{'a', 'b', 'r', 'c', 'd'}
{0, 1, 2}
```

## 5. Generator expressions intro

```python
total = sum(x * x for x in range(1_000_000))
print(total > 0)
gen = (x * 2 for x in range(3))
print(gen)
print(list(gen))
print(sum(1 for w in ["a", "bb"] if len(w) > 1))
```

```text
True
<generator object <genexpr> at 0x...>
[0, 2, 4]
1
```

Parens `(...)` make a lazy generator — no list stored. Feed it straight to `sum`, `join`, `max`.

| Use list comp | Use generator |
|---|---|
| Need result twice | One pass, huge input |
| Small data | Memory matters |
| Indexing | Streaming to `sum`/`any` |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `[x for x]` leak confusion | Old Python scoping myth | 3.x comps have own scope — safe |
| `if/else` in wrong spot | `SyntaxError` | Value `if/else` before `for`, filter after |
| Huge `[...]` OOM | Memory blowup | Use `(...)` generator |
| Over-nested comp | Unreadable | Max 2 loops; else plain `for` |
| Mutating in comp | Side effects | Comps build, never mutate |

## Checklist

- [ ] Write list comp with transform + filter
- [ ] Place `if/else` vs `if` correctly
- [ ] Build dicts with `{k: v for ...}`
- [ ] Build sets with `{x for ...}`
- [ ] Stream with `(x for ...)` into `sum`

#### [Previous](../05-sets/exercises.md) | [Next](02-part.md)
