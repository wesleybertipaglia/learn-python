# Exercises — Data Types

## 1. Predict the Type

Write your prediction, then check with `type()`:

```python
print(type(42))        # ?
print(type(3.0))       # ?
print(type("42"))      # ?
print(type(True))      # ?
print(type(None))      # ?
print(type(10 / 2))    # ? careful
print(type(10 // 2))   # ? careful
```

| Expression | Your guess | Actual |
|---|---|---|
| `10 / 2` | | |
| `10 // 2` | | |

The last two are the point: `/` always makes `float`.

## 2. Broken Addition

This crashes. Fix it two ways (f-string and conversion):

```python
age = input("Age? ")
print("Next year: " + age + 1)
```

Requirements: typing `30` prints `Next year: 31`. Explain in one sentence why the original fails.

## 3. Slice Practice

Given `word = "python"`, write slices that produce:

| Target | Your slice |
|---|---|
| `"py"` | |
| `"thon"` | |
| `"hon"` (use a negative index) | |
| `"python"` (a full copy via slice) | |

Verify each with `print()`.

## 4. Truthiness Table

Run and explain each line of output:

```python
for value in [0, 1, "", "x", None, [], [0]]:
    print(repr(value), bool(value))
```

Questions:
- Why is `[0]` truthy even though `0` is falsy?
- Why is `bool("False")` True?

## 5. Price Formatter (Stretch)

Ask for a price and quantity, print the total with exactly 2 decimals:

```text
Price? 19.99
Qty? 3
Total: $59.97
```

Hint: `float()` for price, `int()` for qty, `:.2f` for output.

---

## Checklist

- [ ] `/` vs `//` types predicted correctly
- [ ] Broken addition fixed and explained
- [ ] All four slices produce the right strings
- [ ] I can explain why `[0]` is truthy

---
#### [Previous](02-part.md) | [Next](../04-operators/01-part.md)
