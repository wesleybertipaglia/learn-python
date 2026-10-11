# Data Types — int, float, str, bool, None

Python has no declarations, but values still have types. Five basics cover 90% of beginner code.

## The Big Five

| Type | Meaning | Example |
|---|---|---|
| `int` | Whole numbers | `42`, `-7`, `0` |
| `float` | Decimal numbers | `3.14`, `-0.5`, `2.0` |
| `str` | Text | `"hello"`, `'it'` |
| `bool` | Truth values | `True`, `False` |
| `None` | "No value" | `None` |

```python
age = 36          # int
price = 19.99     # float
name = "ada"      # str
member = True     # bool
nickname = None   # NoneType
print(type(age), type(price), type(name), type(member), type(nickname))
```

```text
<class 'int'> <class 'float'> <class 'str'> <class 'bool'> <class 'NoneType'>
```

Why care? Because `+` means different things per type: `2 + 3` is `5`, but `"2" + "3"` is `"23"`. When your program does something weird, check types first.

---

## Dynamic Typing: the Name Is Just a Label

The type sticks to the **value**, not the variable:

```python
x = 10
print(type(x))
x = 3.14
print(type(x))
x = "hello"
print(type(x))
```

```text
<class 'int'>
<class 'float'>
<class 'str'>
```

This is flexible but dangerous — a variable that was an `int` on line 5 can be a `str` on line 50. Keep one name = one type per program and you avoid an entire class of bugs.

> `type()` shows the truth. When in doubt, print the type, not just the value. `"42"` and `42` look similar in output but behave completely differently.

---

## `type()` — Your Magnifying Glass

```python
print(type(42))
print(type(3.14))
print(type("hi"))
print(type(True))
print(type(None))
```

```text
<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
<class 'NoneType'>
```

You already met `type()` in lesson 02. Now use it as a debugging habit: any time `input()` is involved, the type is `str` until you convert it.

---

## Conversions: `int()`, `float()`, `str()`, `bool()`

| Call | Result | Note |
|---|---|---|
| `int("42")` | `42` | Fails on `"42.5"` or `"abc"` |
| `float("3.5")` | `3.5` | Also accepts `"3"` → `3.0` |
| `str(42)` | `"42"` | Almost never fails |
| `bool(0)` / `bool("")` | `False` | Empty things are falsy (part 2) |
| `int(3.9)` | `3` | Truncates toward zero, does NOT round |

```python
age_text = input("Age? ")   # always str, e.g. "30"
age = int(age_text)         # now int 30
print(f"Next year: {age + 1}")
```

```text
Age? 30
Next year: 31
```

What happens on bad input:

```python
print(int("thirty"))
```

```text
Traceback (most recent call last):
  ...
ValueError: invalid literal for int() with base 10: 'thirty'
```

That `ValueError` is normal — lesson 08 shows how to catch it so your program does not crash.

---

## Quick Gotchas

```python
print(int(3.9))       # 3, not 4 — truncation
print(float("3"))     # 3.0
print(str(3.14))      # "3.14"
print(10 / 4)         # 2.5 — / always returns float
print(10 // 4)        # 2 — // returns int when both sides are int
```

```text
3
3.0
3.14
2.5
2
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `TypeError: can only concatenate str` | `"Age: " + 30` | Use f-string or `str(30)` |
| `ValueError` from `int()` | User typed `"abc"` | Validate / `try/except` (lesson 08) |
| `int("3.5")` crashes | Float-shaped string | Use `float("3.5")` first, then `int()` if needed |
| Silent truncation | `int(3.9)` → `3` surprises you | Use `round()` if you wanted rounding |

---

## Checklist

- [ ] I can name the five basic types and give an example of each
- [ ] I can explain dynamic typing in one sentence
- [ ] I convert `input()` with `int()`/`float()` before doing math
- [ ] I know `/` gives `float` and `int(3.9)` truncates to `3`

---
#### [Previous](../02-syntax/exercises.md) | [Next](02-part.md)
