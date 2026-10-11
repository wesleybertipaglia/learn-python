# Debugging — Reading Tracebacks

Nobody writes bug-free code. Professionals just read the error faster than beginners. This lesson is that skill.

## Read Bottom-Up: Last Line First

```python
def double(n):
    return n * 2

print(double("3"))
```

Wait — `"3" * 2` is `"33"`, no crash. Here is a real crash:

```python
age = int("thirty")
print(age + 1)
```

```text
Traceback (most recent call last):
  File "/home/user/demo.py", line 1, in <module>
    age = int("thirty")
ValueError: invalid literal for int() with base 10: 'thirty'
```

Three zones, read in this order:

| Order | Zone | Answers |
|---|---|---|
| 1. Last line | `ValueError: ... 'thirty'` | WHAT failed and on which value |
| 2. File/line frames | `File "demo.py", line 1` | WHERE it failed |
| 3. Middle lines | The source echo `age = int(...)` | WHICH statement blew up |

> The last line is the diagnosis. The lines above are the address. Beginners stare at the top; experts read the bottom first.

---

## Anatomy of a Multi-Frame Traceback

```python
def parse_age(text):
    return int(text)

def main():
    text = input("Age? ")
    age = parse_age(text)
    print(f"next year: {age + 1}")

main()
```

```text
Age? abc
Traceback (most recent call last):
  File "/home/user/demo.py", line 10, in <module>
    main()
  File "/home/user/demo.py", line 6, in main
    age = parse_age(text)
  File "/home/user/demo.py", line 2, in parse_age
    return int(text)
ValueError: invalid literal for int() with base 10: 'abc'
```

Call chain: module → `main()` (line 6) → `parse_age()` (line 2) → crash. Your bug is usually in YOUR lowest frame — here `parse_age` got `'abc'`. Fix at the boundary (validate before converting) rather than inside `int()`.

---

## `NameError`: the Typo Error

```python
user_name = "ada"
print(user_nmae)
```

```text
Traceback (most recent call last):
  File "/home/user/demo.py", line 2, in <module>
    print(user_nmae)
NameError: name 'user_nmae' is not defined. Did you mean: 'user_name'?
```

Python 3.12 suggests the close match — read it. Causes in order of frequency:

| Cause | Example | Fix |
|---|---|---|
| Typo | `user_nmae` | Copy-paste names |
| Wrong order | Call before `def`/assignment | Move definition above use |
| Scope | Function-local used outside | `return` it (lesson 07) |
| Forgotten import | `math.sqrt` without `import math` | Add the import |

---

## `TypeError`: Right Shape, Wrong Combination

```python
print("Age: " + 30)
```

```text
Traceback (most recent call last):
  File "/home/user/demo.py", line 1, in <module>
    print("Age: " + 30)
TypeError: can only concatenate str (not "int") to str
```

```python
print(len(42))
```

```text
Traceback (most recent call last):
  ...
TypeError: object of type 'int' has no len()
```

`TypeError` recipe: print `type()` of each operand. One of them is not what you assumed — usually a stray `str` from `input()`.

```python
age = input("Age? ")   # str!
print(age + 1)         # TypeError: str + int
```

Fix: `age = int(input("Age? "))` with validation (lesson 08).

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Reading traceback top-down | Lost in frames, miss the error | Last line first, then lowest YOUR frame |
| `NameError` after rename | Renamed def but not the call | Rename with IDE (F2), not by hand |
| `TypeError` on `+` | `str` from `input()` mixed with `int` | Convert at the boundary |
| Re-running without saving | Traceback line does not match code | Save (Ctrl+S) before every run |

---

## Checklist

- [ ] I read the last traceback line first, frames second
- [ ] I can trace module → function → crash line in a 3-frame traceback
- [ ] I fix `NameError` via typo/order/scope checklist before guessing
- [ ] For `TypeError` I print `type()` of each operand

---
#### [Previous](../08-exceptions/exercises.md) | [Next](02-part.md)
