# Data Types — Strings, f-strings, None and Truthiness

Numbers compute. Strings communicate. And `None` plus truthiness quietly control every `if` you will ever write.

## String Indexing: Positions Start at 0

```python
word = "python"
print(word[0])
print(word[1])
print(word[-1])   # -1 = last character
```

```text
p
y
n
```

| Expression | Meaning |
|---|---|
| `word[0]` | First character |
| `word[-1]` | Last character |
| `word[len(word) - 1]` | Same as `[-1]`, the long way |

Out of range = crash:

```python
word = "hi"
print(word[5])
```

```text
Traceback (most recent call last):
  ...
IndexError: string index out of range
```

> Indexing never wraps around. `word[5]` on a 2-character string is an error, not a guess. Check `len()` first when the index comes from user input.

---

## Slicing: `[start:stop]` Grabs a Range

Slicing copies characters from `start` up to **but not including** `stop`:

```python
word = "python"
print(word[0:2])   # p y
print(word[2:])    # from index 2 to the end
print(word[:4])    # from start up to (not incl.) index 4
print(word[:])     # whole copy
```

```text
py
thon
pyth
python
```

| Slice | Result for `"python"` | Rule |
|---|---|---|
| `[0:2]` | `"py"` | Stop is exclusive |
| `[2:]` | `"thon"` | Omitted stop = to the end |
| `[:4]` | `"pyth"` | Omitted start = from 0 |
| `[-3:]` | `"hon"` | Negative = count from end |

Slices never crash on overflow — they just clamp:

```python
print("hi"[0:100])
```

```text
hi
```

---

## f-strings: Formatting Without Pain

Basic f-strings (lesson 02) plus the two format tricks you need immediately:

```python
name = "ada"
age = 36
print(f"{name} is {age} years old")
```

```python
price = 19.99
print(f"Price: ${price:.2f}")   # .2f = 2 decimals
print(f"Rate: {0.856:.1%}")     # .1% = percentage
```

```text
Price: $19.99
Rate: 85.6%
```

| Spec | Meaning | Example output |
|---|---|---|
| `:.2f` | 2 decimal places | `19.99` |
| `:,.0f` | Thousands separator | `1,234,568` from `1234567.8` |
| `:.1%` | Percentage, 1 decimal | `85.6%` |
| `:>10` | Right-align width 10 | `"       ada"` |

Multiline with intention — f-strings compose fine with `\n` or triple quotes, but keep them short. If your f-string needs three expressions, compute the value first, then format it.

---

## `None`: the Absence of a Value

`None` means "nothing here yet". Functions return it by default, and it is the standard placeholder:

```python
nickname = None
print(nickname)
print(type(nickname))
print(nickname is None)
```

```text
None
<class 'NoneType'>
True
```

Always compare with `is`, never `==` (lesson 04 explains why). `None` is a singleton — there is exactly one of it in your whole program.

---

## Truthiness: Every Value Is True or False in an `if`

Python treats more than just `True`/`False` as boolean in conditions:

```python
for value in [42, "hi", 0, "", None, [], [1]]:
    print(repr(value), "->", bool(value))
```

```text
42 -> True
'hi' -> True
0 -> False
'' -> False
None -> False
[] -> False
[1] -> True
```

The rule: **empty / zero / None is falsy, everything else is truthy.**

| Falsy (the complete short list) | Truthy |
|---|---|
| `False`, `None`, `0`, `0.0` | Any nonzero number |
| `""` (empty string) | Any non-empty string |
| `[]`, `{}`, `()` (empty containers) | Any non-empty container |

Write idiomatic checks:

```python
name = input("Name? ")
if not name:
    print("You typed nothing!")

items = []
if items:
    print("has items")
else:
    print("empty list is falsy")
```

```text
Name?
You typed nothing!
empty list is falsy
```

> Prefer `if not name:` over `if name == "":` and `if x is None:` over `if x == None:`. Shorter, faster, and what every Python codebase expects.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `IndexError: string index out of range` | `word[i]` past the end | Check `len(word)` or use a slice |
| Off-by-one in slices | `[0:2]` "missing" a char | Remember stop is exclusive |
| `if x == None:` works but linters complain | Style + subtle subclass bugs | Use `if x is None:` |
| `bool("False")` is `True` | Non-empty string is truthy | Compare text explicitly: `s == "False"` |

---

## Checklist

- [ ] I can index (`word[0]`, `word[-1]`) and slice (`word[1:4]`) without guessing
- [ ] I can format with `:.2f` and explain why slices never crash but indexes do
- [ ] I use `is None` / `is not None` instead of `==`
- [ ] I can list all falsy values from memory and use `if not x:` correctly

---
#### [Previous](01-part.md) | [Next](exercises.md)
