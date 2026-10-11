# Test Layout and Edge Cases

A test you cannot find is a test that never runs. This lesson covers where to put tests, which edge cases always deserve a test, and what not to test.

## The `tests/` Layout

Standard small-project layout:

```bash
ls -R mycart
```

```text
mycart/
  cart.py
  tests/
    test_cart.py
```

Rules:

| Rule | Example |
|---|---|
| Source at root or in a package | `cart.py` or `shop/cart.py` |
| One `tests/` folder at repo root | `tests/test_cart.py` |
| Test file mirrors module name | `cart.py` → `test_cart.py` |
| Test file is importable plain Python | `from cart import total` |

```python
# cart.py
def total(prices):
    return sum(prices)
```

```python
# tests/test_cart.py
from cart import total


def check_total_empty():
    assert total([]) == 0
```

Run the check from the project root so imports resolve:

```bash
python tests/test_cart.py
```

```text
check_total_empty passed
```

> If you need `sys.path` hacks to import your own code, your layout is wrong — run from the repo root or install the package with `pip install -e .`.

---

## Edge Cases: Empty, None, Invalid

Most production bugs live at the boundaries. Test this short list for every function:

| Category | Question | Example |
|---|---|---|
| Empty | What if the collection/string is empty? | `total([])`, `slug("")` |
| Zero / boundary | What happens at 0, 1, max? | `discount(100, 0)`, `discount(100, 100)` |
| `None` | Allowed or rejected? | `greet(None)` → `TypeError` or `"Hello, guest"`? |
| Invalid type | Wrong type passed? | `total("abc")` raises `TypeError` |
| Invalid value | Right type, nonsense value? | `final_price(-1)` raises `ValueError` |

```python
def slug(title):
    return title.strip().lower().replace(" ", "-")


def check_slug_edges():
    assert slug("Hello World") == "hello-world"  # normal
    assert slug("") == ""  # empty
    assert slug("  Hi  ") == "hi"  # whitespace
    assert slug("A  B") == "a--b"  # double space is visible
    print("slug edges ok")


if __name__ == "__main__":
    check_slug_edges()
```

```text
slug edges ok
```

And the invalid-input side:

```python
def set_age(age):
    if not isinstance(age, int) or isinstance(age, bool):
        raise TypeError("age must be int")
    if age < 0:
        raise ValueError("age must be >= 0")
    return age


def check_set_age():
    assert set_age(0) == 0  # boundary
    try:
        set_age(-1)
    except ValueError:
        pass
    else:
        raise AssertionError("expected ValueError for -1")
    try:
        set_age("20")
    except TypeError:
        pass
    else:
        raise AssertionError("expected TypeError for '20'")
    print("set_age ok")


if __name__ == "__main__":
    check_set_age()
```

```bash
python check_age.py
```

```text
set_age ok
```

> Decide the `None` contract once and test it: either accept `None` with a documented default, or reject it loudly. The worst option is crashing with `AttributeError: 'NoneType'` three layers deep.

---

## What Not to Test

More tests are not always better. Skip these:

| Skip | Why |
|---|---|
| Third-party library internals | Test your usage, not `requests` itself |
| Trivial getters/setters with no logic | No behavior to break |
| Exact stdlib output formats you do not control | Brittle, breaks on upgrade |
| Everything through one giant test | One failure hides all signal; split it |
| Implementation details (private helpers, call counts) | Refactors turn red for no user-visible reason |

Good heuristic: test observable behavior — return values, raised exceptions, files written. If a user cannot observe the difference, do not test it.

```python
# Worth testing: business rule
def shipping(weight):
    return 0 if weight <= 0 else 5 + weight * 2


assert shipping(0) == 0
assert shipping(2) == 9
```

```python
# Not worth testing: alias with no logic
# from json import dumps as to_json  -- nothing to test here
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Tests live next to scratch files | Nobody runs them, imports break | Move to `tests/test_*.py`, run from repo root |
| Only happy-path tests | Empty/`None`/negative inputs crash in prod | Add empty, zero, `None`, invalid cases per function |
| `None` silently accepted | `TypeError` far from the cause | Validate early, raise `TypeError`/`ValueError` with message |
| Testing private names (`_helper`) | Every refactor breaks tests | Test the public function that uses the helper |
| One mega-test with 30 asserts | First failure hides 29 others | One behavior per test (pytest makes this free) |

---

## Checklist

- [ ] Your project has a `tests/` folder with `test_<module>.py` files
- [ ] Every function has empty/zero/boundary cases covered
- [ ] `None` and invalid inputs have a decided, tested contract
- [ ] You can name one thing you deliberately chose not to test

---
#### [Previous](01-part.md) | [Next](exercises.md)
