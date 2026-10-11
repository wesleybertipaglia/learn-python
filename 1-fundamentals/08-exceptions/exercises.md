# Exercises — Exceptions

## 1. Name the Exception

What does each snippet raise? Predict, then run:

```python
int("abc")     # ?
"a" + 1        # ?
[1][5]         # ?
{}["nope"]     # ?
1 / 0          # ?
```

| Code | Exception | Why |
|---|---|---|
| `int("abc")` | | |
| `"a" + 1` | | |
| `{}["nope"]` | | |

## 2. Safe Age Parser

Write `parse_age(text)` returning an `int`, or `None` with a message on failure. Use `try/except ValueError/else`:

```text
parse_age("30") → 30 (prints "parsed ok")
parse_age("abc") → None (prints "bad age: 'abc'")
```

## 3. `read_float` Loop

Write the validation-loop function from `02-part.md` from memory:

```python
def read_float(prompt):
    """Keep asking until the user types a valid number."""
```

Test transcript:

```text
Amount? abc
'abc' is not a number — try again
Amount? 12.5
got 12.5
```

No crash on any input. Ctrl+C must still work (no bare `except`).

## 4. Raise With Context

Write `withdraw(balance, amount)` that raises `ValueError` with the bad value in the message for `amount <= 0` and `amount > balance`. Verify both:

```text
withdraw(100, 500) → ValueError: insufficient funds: 500 > 100
```

## 5. Find the Bare `except` (Stretch)

Explain in two sentences why this is dangerous, then fix it:

```python
try:
    age = int(input("Age? "))
except:
    print("something went wrong")
```

Hint: what happens to Ctrl+C? What happens to a typo like `intt(...)` inside the `try`?

---

## Checklist

- [ ] All five exception names identified correctly
- [ ] `read_float` never crashes on bad input
- [ ] `withdraw` messages contain the offending values
- [ ] Zero bare `except:` in any solution

---
#### [Previous](02-part.md) | [Next](../09-debugging/01-part.md)
