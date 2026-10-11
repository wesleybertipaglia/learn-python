# Tooling I — unittest Map and Coverage

Pytest is the default, but you will meet stdlib `unittest` in old codebases. And passing tests mean little if half your code never runs — that is what coverage measures.

## unittest in 60 Seconds (and the pytest Map)

`unittest` ships with Python. Same ideas, more boilerplate:

```python
# tests/test_total_unittest.py
import unittest

from cart import total


class TestTotal(unittest.TestCase):
    def test_normal(self):
        self.assertEqual(total([10, 20]), 30)

    def test_empty(self):
        self.assertEqual(total([]), 0)


if __name__ == "__main__":
    unittest.main()
```

```bash
python -m pytest tests/test_total_unittest.py -v
```

```text
tests/test_total_unittest.py::TestTotal::test_empty PASSED               [ 50%]
tests/test_total_unittest.py::TestTotal::test_normal PASSED              [100%]

2 passed in 0.03s
```

Pytest runs `unittest` classes as-is, so you can migrate file by file.

Translation table:

| unittest | pytest equivalent |
|---|---|
| `class TestX(unittest.TestCase)` | Plain `def test_*` functions (or plain class) |
| `self.assertEqual(a, b)` | `assert a == b` |
| `self.assertTrue(x)` | `assert x` |
| `self.assertRaises(ValueError)` + `with` | `pytest.raises(ValueError)` + `with` |
| `setUp(self)` method | `@pytest.fixture` |
| `python -m unittest` | `python -m pytest` (more output, less code) |

> New code: write pytest. Old `unittest` code: leave it running under pytest, convert only when you touch the file.

---

## Coverage: Did Your Tests Even Run That Line?

Coverage counts which lines executed during tests.

Install:

```bash
pip install coverage pytest-cov
```

Run:

```bash
python -m pytest --cov=cart --cov-report=term-missing
```

Example with a gap:

```python
# cart.py
def final_price(price, coupon=""):
    if price < 0:
        raise ValueError("price must be >= 0")
    if coupon == "HALF":
        return price / 2
    return price
```

```python
# tests/test_cart.py
from cart import final_price


def test_no_coupon():
    assert final_price(100) == 100
```

```text
Name      Stmts   Miss  Cover   Missing
---------------------------------------
cart.py       6      2    66%   4-5
---------------------------------------
TOTAL         6      2    66%
1 passed in 0.05s
```

Lines 4–5 (the `HALF` branch) never ran. Add the missing test:

```python
def test_half_coupon():
    assert final_price(100, "HALF") == 50


def test_negative_raises():
    import pytest

    with pytest.raises(ValueError):
        final_price(-1)
```

```bash
python -m pytest --cov=cart --cov-report=term-missing -q
```

```text
Name      Stmts   Miss  Cover   Missing
---------------------------------------
cart.py       6      0   100%
---------------------------------------
1 passed, coverage: 100%
```

Coverage config (`pyproject.toml`) to enforce a floor:

```bash
python -m pytest --cov=cart --cov-report=term-missing --cov-fail-under=90 -q
```

```text
Required test coverage of 90% not reached. Total coverage: 66.00%
```

| Command | Purpose |
|---|---|
| `--cov=cart` | Measure package `cart` |
| `--cov-report=term-missing` | Show uncovered line numbers |
| `--cov-fail-under=90` | Fail the run below 90% |
| `--cov-report=html` | Browseable `htmlcov/index.html` report |

> 100% coverage with bad asserts is theater — coverage tells you what ran, only good asserts tell you it was correct.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `--cov` with wrong package name | `0%`, empty report | Point at the source: `--cov=cart`, not `--cov=tests` |
| `unittest` `setUp` doing real I/O | Slow, flaky suite | Replace with fixture + `tmp_path` during migration |
| Chasing 100% on glue code | Brittle tests on `if __name__` blocks | Exclude with `# pragma: no cover` or set floor at 90% |
| Coverage run from wrong dir | `ModuleNotFoundError: cart` | Run `python -m pytest` from repo root |

---

## Checklist

- [ ] You can read the unittest → pytest translation table
- [ ] `pip install coverage pytest-cov` works
- [ ] `python -m pytest --cov=<pkg> --cov-report=term-missing` shows Miss lines
- [ ] You can add a test that closes a coverage gap

---
#### [Previous](../02-pytest/exercises.md) | [Next](02-part.md)
