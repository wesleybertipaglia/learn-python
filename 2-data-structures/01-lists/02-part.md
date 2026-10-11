# Lists Part 2 — Iteration, Sorting, and Stacks

Loop idiomatically, sort correctly, nest deliberately, and use a list as a stack.

## 1. Iteration the Python way

```python
langs = ["python", "rust", "go"]
for lang in langs:
    print(lang.title())

for i, lang in enumerate(langs, start=1):
    print(i, lang)
```

```text
Python
Rust
Go
1 python
2 rust
3 go
```

| Pattern | Code | When |
|---|---|---|
| Values only | `for x in lst:` | Most cases |
| Index + value | `for i, x in enumerate(lst):` | Need position |
| Reversed | `for x in reversed(lst):` | No copy |
| Zipped | `for a, b in zip(l1, l2):` | Parallel walk |

> Never do `for i in range(len(lst)): print(lst[i])` unless you must mutate indexes. Use `enumerate`.

---

## 2. Sorting: sorted() vs list.sort()

```python
nums = [5, 1, 4, 2]
print(sorted(nums))
print(nums)  # unchanged
nums.sort()
print(nums)  # mutated
words = ["pear", "fig", "apple"]
print(sorted(words, key=len))
words.sort(key=len, reverse=True)
print(words)
```

```bash
python list_sort.py
```

```text
[1, 2, 4, 5]
[5, 1, 4, 2]
[1, 2, 4, 5]
['fig', 'pear', 'apple']
['pear', 'apple', 'fig']
```

`sorted()` returns a new list and works on any iterable. `list.sort()` sorts in place and returns `None`.

---

## 3. Nested lists

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
]
print(matrix[0][2])
print(matrix[1])
matrix[1][0] = 40
for row in matrix:
    print(row)
```

```text
3
[4, 5, 6]
[1, 2, 3]
[40, 5, 6]
```

Build grids with a comprehension so rows are independent:

```python
grid = [[0] * 3 for _ in range(2)]
grid[0][0] = 1
print(grid)
```

```text
[[1, 0, 0], [0, 0, 0]]
```

---

## 4. List as a stack (LIFO)

```python
stack = []
stack.append("a")
stack.append("b")
stack.append("c")
print(stack.pop())
print(stack.pop())
print(stack)
print(bool(stack), bool([]))
```

```text
c
b
['a']
True False
```

Push with `append`, pop with `pop()`. Empty-list check: `if stack:`.

> For queues use `collections.deque`, not `list.pop(0)` — popping from the front of a list is O(n).

---

## 5. Unpacking preview

```python
first, *middle, last = [1, 2, 3, 4, 5]
print(first, middle, last)
a, b = [10, 20]
print(a, b)
```

```text
1 [2, 3, 4] 5
10 20
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `nums.sort()` assigned to var | `None` printed | `sort()` is in-place; use `sorted()` for a copy |
| Mutating while iterating | Skipped items | Iterate over `lst[:]` or build a new list |
| `pop(0)` in a loop | Slow on big lists | Use `deque.popleft()` |
| `range(len())` everywhere | Verbose, error-prone | Use `enumerate()` or direct iteration |
| Shared rows `[[0]*n]*m` | Editing one edits all | Use `[[0]*n for _ in range(m)]` |

## Checklist

- [ ] Iterate directly and with `enumerate(start=...)`
- [ ] Choose `sorted()` vs `.sort()` and use `key=` + `reverse=`
- [ ] Read and write nested lists with `[r][c]`
- [ ] Implement a stack with `append` / `pop`
- [ ] Unpack with `first, *rest, last`

#### [Previous](01-part.md) | [Next](exercises.md)
