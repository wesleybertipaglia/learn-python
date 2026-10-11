# Testing Introduction — Why Test

You write code. It works today. You change one line next week and it silently breaks. A test is a small program that catches that break for you.

This lesson covers why tests pay off, the Arrange-Act-Assert pattern, the `assert` statement, and why pure functions are the easiest code to test.

## Why Test at All

Manual checking does not scale:

| Manual check | Automated test |
|---|---|
| You run the code and eyeball output | `python -m pytest` runs every check in milliseconds |
| You forget edge cases under pressure | Edge cases are written down once, run forever |
| Refactors are scary | Refactors are safe — tests turn red if you break something |
| Bugs reach users | Bugs are caught before commit |

A test answers one question: "does this function still do what I promised?"

```python
def add(a, b):
    return a + b


assert add(2, 3) == 5
print("ok")
```

```text
ok
```

> A test is executable documentation. If the test says `add(2, 3) == 5`, that is the spec — no wiki page needed.

---

## Arrange-Act-Assert

Every test has the same three steps:

| Step | Meaning | Example |
|---|---|---|
| Arrange | Set up inputs | `total = 100` |
| Act | Call the code | `result = discount(total, 10)` |
| Assert | Check the outcome | `assert result == 90` |

Here it is in code:

```python
def discount(total, percent):
    return total - total * percent / 100


# Arrange
total = 200
# Act
result = discount(total, 25)
# Assert
assert result == 150.0
print("discount ok:", result)
```

```text
discount ok: 150.0
```

Keep the three steps visually separate. One Act per test. If you need two Acts, you need two tests.

---

## The `assert` Statement

`assert condition, message` does nothing when the condition is true and raises `AssertionError` when it is false:

```python
def is_adult(age):
    return age >= 18


assert is_adult(20) is True
assert is_adult(17) is False
assert is_adult(18) is True, "18 must count as adult"
print("all asserts passed")
```

```text
all asserts passed
```

A failing assert looks like this:

```bash
python check.py
```

```text
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError: 18 must count as adult
```

Rules for good asserts:

| Do | Do not |
|---|---|
| Compare exact values: `assert total(["a"]) == 1` | Assert vague truthiness: `assert result` |
| Add a message for non-obvious cases | Add a message to every line (noise) |
| One logical check per test function (later, with pytest) | Pack 15 asserts into one script |

> If an assert needs a comment to explain it, rewrite the assert. `assert balance == 90` beats `assert x == 90  # balance after fee`.

---

## Pure Functions Are Easy to Test

A pure function depends only on its arguments and has no side effects. No files, no network, no global mutation.

```python
def total(prices):
    """Pure: same input always gives same output."""
    return sum(prices)


assert total([10, 20, 30]) == 60
assert total([]) == 0
print("pure ok")
```

```text
pure ok
```

Compare with an impure version:

```python
TAX = 0.2

def total_with_tax(prices):
    global TAX
    TAX += 0.01  # side effect: hidden state changes
    return sum(prices) * (1 + TAX)
```

You cannot test `total_with_tax` reliably because the answer changes on every call. Lesson: push logic into pure functions, keep I/O at the edges.

| Testable | Hard to test |
|---|---|
| `def net(price, rate): return price * (1 - rate)` | Function that reads `input()` inside |
| `def slug(s): return s.lower().replace(" ", "-")` | Function that writes to a file and returns `None` |
| `def valid(email): return "@" in email` | Function that calls an API with no mock |

---

## First Hand-Rolled Test with `assert`

No framework yet. Just a `check_` script you run with `python`:

```python
# shop.py
def final_price(price, coupon=""):
    if price < 0:
        raise ValueError("price must be >= 0")
    if coupon == "HALF":
        return price / 2
    return price


def check_final_price():
    # Arrange-Act-Assert, three tiny cases
    assert final_price(100) == 100
    assert final_price(100, "HALF") == 50
    try:
        final_price(-5)
    except ValueError:
        pass
    else:
        raise AssertionError("negative price must raise ValueError")
    print("check_final_price passed")


if __name__ == "__main__":
    check_final_price()
```

```bash
python shop.py
```

```text
check_final_price passed
```

This pattern works but gets painful fast: no auto-discovery, one failure hides the rest, output is either silence or a traceback. That is the problem pytest solves in the next module.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Asserting `== True` on truthy values | `assert [1] == True` fails | Use `assert [1] == [1]` or `assert bool([1]) is True` |
| Floating-point equality | `assert 0.1 + 0.2 == 0.3` fails | Use `abs(a - b) < 1e-9` or pytest `approx` (next lesson) |
| Testing impure code directly | Flaky, order-dependent results | Extract pure helper, test that |
| Swallowing the failure message | Bare `assert x == y` with cryptic data | Add message only when data is unclear: `assert x == y, f"got {x!r}"` |

---

## Checklist

- [ ] You can explain Arrange-Act-Assert in one sentence
- [ ] You know `assert cond, msg` raises `AssertionError` on failure
- [ ] You can spot a pure vs. impure function
- [ ] You wrote and ran a `check_` script with `python shop.py`

---
#### [Previous](../../4-advanced/challenge.md) | [Next](02-part.md)
