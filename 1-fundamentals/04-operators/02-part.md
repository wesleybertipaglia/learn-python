# Operators — Precedence, Identity, Membership, Assignment

Part 1 gave you the operators. This part teaches the rules that decide what they actually do.

## Precedence: What Runs First

| Priority | Operators |
|---|---|
| Highest | `**` |
| Then | `*  /  //  %` (left to right) |
| Then | `+  -` |
| Then | Comparisons (`==  <  in  is`) |
| Then | `not` |
| Lowest | `and`, then `or` |

```python
print(2 + 3 * 4)      # 14, not 20
print(2 ** 3 ** 2)    # 512 — ** is right-associative: 2 ** (3 ** 2)
print(True or True and False)  # True — and binds tighter than or
```

```text
14
512
True
```

> When in doubt, add parentheses. `(2 + 3) * 4` is free, obvious, and immune to misremembered tables. Senior engineers parenthesize too.

---

## `is` vs `==`: Identity vs Equality

- `==` asks "same **value**?"
- `is` asks "same **object in memory**?"

```python
a = [1, 2]
b = [1, 2]
print(a == b)   # True — same contents
print(a is b)   # False — two different list objects
```

```text
True
False
```

```python
x = None
print(x is None)    # True — correct idiom
print(x == None)    # also True, but wrong style
```

Use `==` for values, `is` for `None` (and occasionally `True`/`False`). Never use `is` to compare numbers or strings — CPython caches small ints so `256 is 256` happens to work, but `257 is 257` may not. That is an implementation accident, not a promise.

---

## `in`: Membership in One Word

```python
print("py" in "python")
print("z" in "python")
print("admin" in ["admin", "root"])
```

```text
True
False
True
```

| Use | Example |
|---|---|
| Substring test | `"@" in email` |
| List membership | `"ana" in users` |
| Guard before indexing | `if key in counts:` |

`in` is why you rarely need manual search loops at this level.

---

## Augmented Assignment: `+=` and Friends

```python
total = 0
total += 5   # same as total = total + 5
total *= 2   # same as total = total * 2
print(total)
```

```text
10
```

| Shortcut | Long form |
|---|---|
| `x += 1` | `x = x + 1` |
| `x -= 2` | `x = x - 2` |
| `x *= 3` | `x = x * 3` |
| `x /= 2` | `x = x / 2` |
| `x //= 2`, `x %= 2`, `x **= 2` | Same pattern |

Why bother? Less typing, and for lists `+=` mutates in place (matters in module 2). For numbers it is pure convenience.

---

## Short-Circuit: `and`/`or` Return Operands

`and` and `or` do not return `True`/`False` — they return **one of their operands**:

```python
print(0 or "default")        # "default" — 0 is falsy, so return right side
print("hi" or "default")     # "hi" — left side truthy, returned as-is
print("hi" and "there")      # "there" — both truthy, return last
print("" and "there")        # "" — first falsy, returned immediately
```

```text
default
hi
there

```

This explains the part-1 trap: `color == "red" or "blue"` returns `"blue"` (truthy) when the comparison fails — so the `if` always passes.

Idiomatic uses:

```python
name = input("Name? ") or "anonymous"   # default when input is ""
print(f"Hello, {name}")
```

```python
# Guard pattern: only call .upper() if name is non-empty
name = ""
if name and name[0] == "a":
    print("starts with a")
```

The second example never indexes `""` because `and` stops at the first falsy operand. That is short-circuit evaluation doing real work.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `2 + 3 * 4` expected `20` | Got `14` | Precedence: `*` first; add parens |
| `is` for number comparison | Works for `256`, breaks for `1000` | Always use `==` for values |
| `x == "a" or "b"` always truthy | Branch always taken | `x in ("a", "b")` or repeat comparison |
| `x /= 2` surprises | `x` became `float` | Use `//=` if you want to stay integral |

---

## Checklist

- [ ] I can rank `**`, `*`, `+`, comparisons, `not`, `and`, `or` by precedence
- [ ] I use `==` for values and `is None` for `None`
- [ ] I can use `in` for substrings and list membership
- [ ] I can explain what `"" or "default"` returns and why

---
#### [Previous](01-part.md) | [Next](exercises.md)
