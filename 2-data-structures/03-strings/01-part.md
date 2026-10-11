# Strings Part 1 — Immutability, Methods, and f-strings

Strings are immutable sequences of characters. Master methods and formatting early.

## 1. Immutability

```python
s = "hello"
print(s[0], s[-1], len(s))
try:
    s[0] = "H"
except TypeError as e:
    print("TypeError:", e)
s2 = "H" + s[1:]
print(s2)
```

```text
h o 5
TypeError: 'str' object does not support item assignment
Hello
```

```bash
python string_immutable.py
```

| Operation | Mutates? | Returns |
|---|---|---|
| `s[0] = "x"` | No — error | `TypeError` |
| `s + "!"` | No | New string |
| `s.replace()` | No | New string |
| `s.upper()` | No | New string |

> Every "change" builds a new string. Reassign: `s = s.strip()`.

---

## 2. Essential methods

```python
raw = "  Ada Lovelace  "
print(repr(raw.strip()))
print("a,b,c".split(","))
print("a b  c".split())
print("2024-01-02".split("-"))
print("hi".replace("i", "ello"))
```

```text
'Ada Lovelace'
['a', 'b', 'c']
['a', 'b', 'c']
['2024', '01', '02']
'hello'
```

| Method | Example | Result |
|---|---|---|
| `strip()` | `" x ".strip()` | `"x"` |
| `split(sep)` | `"a,b".split(",")` | `["a","b"]` |
| `join(list)` | `",".join(["a","b"])` | `"a,b"` |
| `replace(a,b)` | `"hi".replace("i","o")` | `"ho"` |
| `lower()/upper()` | `"Hi".lower()` | `"hi"` |

---

## 3. f-strings

```python
name, age = "ada", 36
print(f"{name} is {age} years old")
print(f"{name!r} has {len(name)} letters")
pi = 3.14159
print(f"{pi:.2f}")
```

```text
ada is 36 years old
'ada' has 3 letters
3.14
```

Use `!r` for debug repr, `:.2f` for 2 decimals.

---

## 4. Format spec mini-guide

```python
n = 42
print(f"{n:5d}|")
print(f"{n:05d}")
print(f"{3.14159:.1f} {1000:,}")
print(f"{0.856:.1%}")
print(f"{'hi':>10}|{'hi':<10}|{'hi':^10}|")
```

```text
   42|
00042
3.1 1,000
85.6%
        hi|hi        |    hi    |
```

| Spec | Meaning | Example |
|---|---|---|
| `:05d` | Zero-pad width 5 | `00042` |
| `:.2f` | 2 decimals | `3.14` |
| `:,` | Thousands sep | `1,000` |
| `:.1%` | Percent | `85.6%` |
| `:>10` | Right align | `"        hi"` |

---

## 5. Building strings efficiently

```python
parts = ["a", "b", "c"]
print("-".join(parts))
print("".join(str(i) for i in range(3)))
```

```text
a-b-c
012
```

> Join beats `+=` in a loop. Never `s += x` for thousands of pieces — collect then `join`.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `s[0] = "X"` | `TypeError` | `s = "X" + s[1:]` |
| Forgetting `strip()` reassign | Whitespace stays | `s = s.strip()` |
| `split(",")` on `"a b"` | No split | Use `split()` for whitespace |
| `join` with non-str | `TypeError` | `",".join(map(str, xs))` |
| `{}` without `f` prefix | Literal braces | Add `f"..."` |

## Checklist

- [ ] Explain string immutability
- [ ] Use `strip`, `split`, `join`, `replace`
- [ ] Format with f-strings and `!r`
- [ ] Apply `:05d`, `:.2f`, `:,`, `:>10`
- [ ] Build strings with `join`, not `+=` loops

#### [Previous](../02-tuples/exercises.md) | [Next](02-part.md)
