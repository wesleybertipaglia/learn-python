# Exceptions — raise, Messages, Validation Pattern

Catching handles other people's errors. `raise` creates your own — so bad data fails loud and early.

## `raise`: Reject Bad Input on Purpose

```python
def withdraw(balance, amount):
    """Return new balance. Raise on invalid amount."""
    if amount <= 0:
        raise ValueError(f"amount must be positive, got {amount}")
    if amount > balance:
        raise ValueError(f"insufficient funds: {amount} > {balance}")
    return balance - amount

print(withdraw(100, 30))
print(withdraw(100, 500))
```

```text
70
Traceback (most recent call last):
  ...
ValueError: insufficient funds: 500 > 100
```

Why raise instead of returning `None`? Because `None` is easy to ignore — the caller forgets to check and the bug travels. An exception **cannot** be ignored silently; it crashes at the exact spot with your message attached.

| Return `None` | `raise ValueError` |
|---|---|
| Caller must remember to check | Impossible to ignore |
| Bug surfaces far from cause | Fails at the cause with your message |
| Good for "missing is normal" | Good for "this must never happen" |

---

## Custom Messages: Say What + What Was Got

Bad: `raise ValueError("bad input")`. Good: include the value and the rule:

```python
raise ValueError(f"age must be 0-130, got {age!r}")
raise KeyError(f"unknown account: {name!r}")
```

The `{value!r}` format shows quotes for strings (`'abc'` vs `abc`) — small detail, big debugging win. The reader should never have to re-run your code to learn what the bad value was.

---

## The Input Validation Pattern

This is the loop you will use in every CLI program from now on, including the bank challenge:

```python
def read_float(prompt):
    """Keep asking until the user types a valid number."""
    while True:
        text = input(prompt)
        try:
            return float(text)
        except ValueError:
            print(f"'{text}' is not a number — try again")

amount = read_float("Amount? ")
print(f"got {amount}")
```

```text
Amount? abc
'abc' is not a number — try again
Amount? 12.5
got 12.5
```

Why it works: `return` inside `try` exits the whole function (and the loop) on success. Failure prints and loops. No flags, no counters, no `break` needed.

Same shape for `int()`:

```python
def read_int(prompt):
    while True:
        text = input(prompt)
        try:
            return int(text)
        except ValueError:
            print(f"'{text}' is not a whole number — try again")
```

> Validate at the boundary (right where `input()` arrives), then trust the value everywhere inside. One validation function per type beats scattered `try` blocks in every menu option.

---

## Never Bare `except`: The Rule With Teeth

```python
# NEVER do this
try:
    age = int(input("Age? "))
except:
    print("something went wrong")
```

Three ways this hurts you:

1. Catches `KeyboardInterrupt` — Ctrl+C stops working, users hate you.
2. Catches `NameError` from your own typos — hides bugs instead of showing them.
3. Tells the user nothing — "something went wrong" helps nobody.

```python
# Always do this
try:
    age = int(input("Age? "))
except ValueError:
    print("please type digits only")
```

If you truly need a safety net (top-level crash handler), use `except Exception:` — still narrow enough to let Ctrl+C through — and log the error.

---

## Bugs You Will Write

| Bug | Symptom | Fix |
|---|---|---|
| Bare `except:` | Ctrl+C ignored, typos hidden | Name the exception |
| `raise` with no message | `ValueError` with zero context | `raise ValueError(f"... got {x!r}")` |
| Validating deep inside logic | Same `try` copied 6 times | `read_int`/`read_float` at the boundary |
| Returning `None` for invalid | Silent wrong math downstream | `raise` so it fails at the cause |

---

## Checklist

- [ ] I `raise ValueError` with a message containing the bad value
- [ ] I can write `read_int`/`read_float` validation loops from memory
- [ ] No bare `except:` anywhere in my code — specific types only
- [ ] I validate at the input boundary, not scattered through logic

---
#### [Previous](01-part.md) | [Next](exercises.md)
