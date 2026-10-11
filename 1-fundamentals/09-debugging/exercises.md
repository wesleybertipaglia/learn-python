# Exercises — Debugging

## 1. Bottom-Up Reading

This crashes. Without running it, answer: what is the error type, which line, which value?

```python
def parse_age(text):
    return int(text)

age = parse_age("abc")
print(age + 1)
```

| Question | Your answer |
|---|---|
| Exception type (last line)? | |
| Crashing line? | |
| Offending value? | |

Then run it and check. You pass if all three match.

## 2. Fix the `NameError`

```python
user_name = "ada"
print(user_nmae)
```

Tasks: read the 3.12 suggestion (`Did you mean...?`), fix the typo, run clean. One sentence: what are the other two causes of `NameError` besides typos?

## 3. Fix the `TypeError`

```python
age = input("Age? ")
print(age + 1)
```

Typing `30` crashes. Explain WHY (`type()` of `age`?), then fix with validated `int()` conversion so `abc` prints a friendly message instead of crashing.

## 4. `breakpoint()` Hunt

This returns the wrong total. Find why with `breakpoint()` (not by staring):

```python
def total(prices):
    acc = ""
    for p in prices:
        acc += p
    return acc

print(total([10, 20, 30]))
```

Hint: `(Pdb) p type(acc)` on the first pause tells you everything. Fix: `acc = 0`.

## 5. `ruff check` Clean (Stretch)

Run on your fix from exercise 4 plus any earlier exercise file:

```bash
pip install ruff
ruff check total.py
```

Requirements: zero findings. Fix every `F401`/`F821` it reports. Paste the final `ruff check` output (`All checks passed!`) alongside your solution.

---

## Checklist

- [ ] Traceback predicted correctly before running
- [ ] `NameError` and `TypeError` fixed with root-cause explanations
- [ ] Bug found via `breakpoint()`/`pdb`, not guessing
- [ ] `ruff check` passes clean

---
#### [Previous](02-part.md) | [Next](../challenge.md)
