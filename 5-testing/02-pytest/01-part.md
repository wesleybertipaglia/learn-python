# Pytest Basics — Discovery and Assert Rewriting

Hand-rolled `check_` scripts do not scale: one failure stops the file, nothing is auto-discovered, output is a bare traceback. Pytest fixes all three.

## Install and First Run

```bash
pip install pytest
```

Check the version (3.12+ environment assumed):

```bash
python -m pytest --version
```

```text
pytest 8.3.4
```

> Always run pytest as `python -m pytest`, never bare `pytest` — this guarantees you use the pytest installed for your current interpreter and venv.

Minimal project:

```bash
ls -R .
```

```text
.:
cart.py  tests/
./tests/:
test_cart.py
```

```python
# cart.py
def total(prices):
    return sum(prices)
```

```python
# tests/test_cart.py
from cart import total


def test_total_normal():
    assert total([10, 20]) == 30


def test_total_empty():
    assert total([]) == 0
```

```bash
python -m pytest
```

```text
============================= test session starts ==============================
collected 2 items

tests/test_cart.py ..                                                    [100%]

============================== 2 passed in 0.02s ===============================
```

---

## Discovery Rules

Pytest finds tests without any registration:

| Rule | Example |
|---|---|
| Files `test_*.py` or `*_test.py` | `tests/test_cart.py` ✅, `tests/cart_check.py` ❌ |
| Classes `Test*` with no `__init__` | `class TestCart:` ✅ |
| Functions `test_*` | `def test_total_empty():` ✅ |
| Run from repo root | `python -m pytest` collects `tests/` recursively |

```bash
python -m pytest --collect-only -q
```

```text
tests/test_cart.py::test_total_normal
tests/test_cart.py::test_total_empty

2 tests collected in 0.01s
```

One file not matching the pattern is silently ignored — the most common "pytest found 0 tests" cause.

---

## Test Functions and `-v` Output

Each test function is independent: failure in one never skips the others.

```python
# tests/test_discount.py
from cart import total


def test_total_floats():
    assert total([2.5, 2.5]) == 5.0


def test_total_fails_demo():
    assert total([1, 2]) == 999  # will fail on purpose
```

```bash
python -m pytest -v
```

```text
tests/test_cart.py::test_total_normal PASSED                             [ 25%]
tests/test_cart.py::test_total_empty PASSED                              [ 50%]
tests/test_discount.py::test_total_floats PASSED                         [ 75%]
tests/test_discount.py::test_total_fails_demo FAILED                     [100%]

1 failed, 3 passed in 0.05s
```

Useful flags:

| Flag | Effect |
|---|---|
| `-v` | One line per test with `PASSED`/`FAILED` |
| `-q` | Compact dots output |
| `-x` | Stop after first failure |
| `path::test_name` | Run a single test: `python -m pytest tests/test_cart.py::test_total_empty -v` |

---

## Assert Rewriting

Plain `assert` in pytest is more powerful than in a script. Pytest rewrites asserts at import time to show values:

```python
def test_rewrite_demo():
    got = [10, 20, 30]
    assert sum(got) == 61
```

```bash
python -m pytest tests/test_demo.py -v
```

```text
FAILED tests/test_demo.py::test_rewrite_demo - assert 60 == 61
```

Compare with a hand-rolled script, which only prints `AssertionError` with no values. Rewriting works for `==`, `in`, `is`, comparisons, and boolean operators — for free, no helper library.

> Keep asserts plain: `assert result == expected`. Do not wrap them in `assertTrue()` or custom helpers — you lose the rewriting output.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| File named `cart_test_helpers.py` not collected | `0 tests collected`, helpers actually contain tests | Rename to `test_*.py` or keep helpers in `conftest.py`/non-test files |
| Function named `check_total` | Silently skipped | Rename to `test_total` |
| Running bare `pytest` in wrong venv | Imports resolve to old code | Always `python -m pytest` from repo root |
| Assert with message hiding values | `assert x == y, "bad"` prints only `"bad"` | Let rewriting speak; add messages only for domain context |

---

## Checklist

- [ ] `pip install pytest` succeeds and `python -m pytest --version` prints 8.x
- [ ] `tests/test_*.py` files are discovered with `--collect-only`
- [ ] You can run one test with `path::test_name -v`
- [ ] You have seen a rewritten assert diff (`assert 60 == 61`)

---
#### [Previous](../01-introduction/exercises.md) | [Next](02-part.md)
