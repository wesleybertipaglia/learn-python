# Operators — Arithmetic, Comparison, Logic

Operators are the verbs of Python. Three families: math, comparison, logic.

## Arithmetic: More Than `+ - * /`

| Operator | Name | Example | Result |
|---|---|---|---|
| `+ - *` | Add, sub, mul | `2 + 3`, `2 * 3` | `5`, `6` |
| `/` | True division | `10 / 4` | `2.5` (always `float`) |
| `//` | Floor division | `10 // 4` | `2` |
| `%` | Modulo (remainder) | `10 % 3` | `1` |
| `**` | Power | `2 ** 10` | `1024` |

```python
print(10 / 4)    # 2.5
print(10 // 4)   # 2
print(10 % 3)    # 1
print(2 ** 10)   # 1024
print(-7 // 2)   # -4 (floors toward -inf, not toward zero!)
```

```text
2.5
2
1
1024
-4
```

Why three divisions? `//` counts whole groups (how many full boxes), `%` gives the leftover (what does not fit). Together they solve "paging", "even/odd", "wrap-around" problems:

```python
total = 25
per_page = 10
print(total // per_page, "full pages,", total % per_page, "leftover")
print("even?" , 42 % 2 == 0)
```

```text
2 full pages, 5 leftover
even? True
```

> `%` with negatives follows the divisor's sign: `-7 % 3` is `2`, not `-1`. If that surprises you, you are thinking of C — Python floors instead of truncating.

---

## Comparison: Always Returns `bool`

| Operator | Meaning | Example |
|---|---|---|
| `==` / `!=` | Equal / not equal | `3 == 3`, `"a" != "b"` |
| `< <= > >=` | Ordering | `2 < 5`, `"b" > "a"` |
| Chained | `a < b < c` | `1 < x < 10` |

```python
print(3 == 3)
print(3 != 4)
print(2 < 5)
print(1 < 3 < 10)   # chained: both must hold
```

```text
True
True
True
True
```

Strings compare by Unicode code point — fine for `"apple" < "banana"`, useless for user-facing sorting of accented text. Do not overthink it yet.

---

## Logical: `and` / `or` / `not`

```python
age = 20
has_ticket = True
print(age >= 18 and has_ticket)
print(age < 18 or has_ticket)
print(not has_ticket)
```

```text
True
True
False
```

| Operator | Rule | Example |
|---|---|---|
| `and` | True only if both sides True | `True and False` → `False` |
| `or` | True if either side True | `True or False` → `True` |
| `not` | Flips the value | `not True` → `False` |

Beginner trap — testing one variable against two values:

```python
color = "red"
# WRONG: always True ("blue" is truthy)
print(color == "red" or "blue")
# RIGHT: repeat the comparison
print(color == "red" or color == "blue")
```

```text
True
True
```

The first line prints `True` even when `color` is `"green"`. Read lesson 04 part 2 to learn exactly why (short-circuit returns operands, not just `True`/`False`).

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `"3" + 4` → `TypeError` | Mixed `str` and `int` | Convert first: `int("3") + 4` |
| `5 / 2` expected `2` | Got `2.5` | You wanted `//` |
| `x == "a" or "b"` always truthy | Wrong boolean logic | Write `x == "a" or x == "b"` |
| `=` instead of `==` in a condition | `SyntaxError` | `=` assigns, `==` compares |

---

## Checklist

- [ ] I can predict `//`, `%`, `**` results including `-7 // 2`
- [ ] I know `/` always returns `float`
- [ ] I can chain comparisons (`1 < x < 10`)
- [ ] I never write `x == "a" or "b"` — I repeat the variable

---
#### [Previous](../03-data-types/exercises.md) | [Next](02-part.md)
