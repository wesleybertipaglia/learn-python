# Challenge — Test a Small Money/Cart Library

Build and fully test a tiny shop library. You write the implementation **and** the suite. Coverage gate: **90%+**.

> You are done when a stranger can clone the folder, run one command, and see green.

---

## 1. Project Layout

Create exactly this:

```bash
mkdir -p minishop/tests && ls -R minishop
```

```text
minishop/
  money.py
  tests/
    test_money.py
```

---

## 2. Spec to Implement (`money.py`)

Implement these four functions (pure, annotated, no I/O):

| Function | Contract |
|---|---|
| `total(prices: list[float]) -> float` | Sum; `[]` → `0.0`; raise `TypeError` if any item is not `int`/`float` (reject `bool`) |
| `discount(total: float, percent: float) -> float` | `total - total*percent/100`; `percent` must satisfy `0 <= percent <= 100` else `ValueError`; `total < 0` → `ValueError` |
| `final_price(price: float, coupon: str = "") -> float` | `price < 0` → `ValueError`; `"HALF"` → `price/2`; `"SAVE10"` → `price-10` floored at `0`; anything else → `price` |
| `save_receipt(path, lines: list[str]) -> None` | Write `"\n".join(lines)` as UTF-8 to `path`; `path` may be `str` or `Path` |

Reference implementation sketch (fill in validation yourself):

```python
from pathlib import Path


def total(prices: list[float]) -> float:
    s = 0.0
    for p in prices:
        if isinstance(p, bool) or not isinstance(p, (int, float)):
            raise TypeError("price must be int or float")
        s += p
    return s


def discount(total: float, percent: float) -> float:
    if total < 0:
        raise ValueError("total must be >= 0")
    if not 0 <= percent <= 100:
        raise ValueError("percent must be 0..100")
    return total - total * percent / 100
```

```python
def final_price(price: float, coupon: str = "") -> float:
    if price < 0:
        raise ValueError("price must be >= 0")
    if coupon == "HALF":
        return price / 2
    if coupon == "SAVE10":
        return max(0.0, price - 10)
    return price


def save_receipt(path, lines: list[str]) -> None:
    Path(path).write_text("\n".join(lines), encoding="utf-8")
```

---

## 3. Full Pytest Suite (`tests/test_money.py`)

Your suite **must** include all four techniques:

| Required | Example |
|---|---|
| Plain `test_*` functions | `def test_total_empty(): ...` |
| `@pytest.mark.parametrize` | `discount` and `final_price` tables (4+ rows each) |
| `@pytest.fixture` | At least one fixture (e.g. `sample_prices`) |
| `pytest.raises` with `match` | `TypeError` for `total`, `ValueError` for `discount`/`final_price` |
| `tmp_path` | `save_receipt` round-trip test |

Suite skeleton:

```python
import pytest

from money import discount, final_price, save_receipt, total


@pytest.fixture
def sample_prices():
    return [10.0, 20.0, 30.0]


def test_total_sample(sample_prices):
    assert total(sample_prices) == 60.0


@pytest.mark.parametrize(
    ("amount", "pct", "expected"),
    [(100, 0, 100), (100, 50, 50), (200, 25, 150.0), (80, 100, 0)],
)
def test_discount_table(amount, pct, expected):
    assert discount(amount, pct) == expected


@pytest.mark.parametrize(
    ("price", "coupon", "expected"),
    [(100, "", 100), (100, "HALF", 50), (100, "SAVE10", 90), (5, "SAVE10", 0.0)],
)
def test_final_price_table(price, coupon, expected):
    assert final_price(price, coupon) == expected


def test_total_rejects_str():
    with pytest.raises(TypeError, match="must be int or float"):
        total([10, "x"])


def test_discount_rejects_percent():
    with pytest.raises(ValueError, match="0..100"):
        discount(100, 150)


def test_save_receipt(tmp_path):
    target = tmp_path / "r.txt"
    save_receipt(target, ["apple 2", "total 2"])
    assert target.read_text(encoding="utf-8") == "apple 2\ntotal 2"
```

---

## 4. Run It: Example Session Transcript

All commands run from inside `minishop/`:

```bash
pip install pytest pytest-cov
python -m pytest -v
```

```text
tests/test_money.py::test_total_sample PASSED                            [ 10%]
tests/test_money.py::test_discount_table[100-0-100] PASSED                [ 20%]
tests/test_money.py::test_final_price_table[100-HALF-50] PASSED           [ 60%]
tests/test_money.py::test_total_rejects_str PASSED                       [ 80%]
tests/test_money.py::test_save_receipt PASSED                            [100%]

12 passed in 0.05s
```

Coverage gate:

```bash
python -m pytest --cov=. --cov-report=term-missing --cov-fail-under=90 -q
```

```text
Name              Stmts   Miss  Cover   Missing
-----------------------------------------------
money.py             24      1    96%   31
tests/test_money.py  30      0   100%
-----------------------------------------------
TOTAL                54      1    98%
12 passed, coverage: 98%
```

Lint + types (if installed):

```bash
ruff check . && ruff format --check . && mypy .
python -m pytest --cov=. --cov-fail-under=90 -q
```

```text
All checks passed!
Success: no issues found in 1 source file
12 passed, coverage: 98%
```

> If any step is red, fix it before moving on — the grading script runs the same sequence.

---

## 5. Grading Checklist

| # | Requirement | Points |
|---|---|---|
| 1 | `money.py` implements all 4 functions with stated contracts | 3 |
| 2 | Suite has plain tests + `parametrize` (2 tables, 4+ rows each) | 3 |
| 3 | Suite has a `fixture`, `tmp_path` test, and `raises`+`match` tests | 3 |
| 4 | `python -m pytest -v` — all green | 2 |
| 5 | Coverage `--cov-fail-under=90` passes, `term-missing` shows no surprise gaps | 2 |
| 6 | `ruff check` + `ruff format --check` clean (or documented skips) | 1 |
| 7 | No C code, no files written outside `tmp_path`, runs from repo root | 1 |

**Pass: 12+/15.** Full marks need 100% on `money.py` plus mypy-clean annotations.

---

## 6. Extras (Pick One)

| Extra | Hint |
|---|---|
| HTML coverage report | `python -m pytest --cov=. --cov-report=html`, open `htmlcov/index.html` |
| `ids=` for parametrize rows | `@pytest.mark.parametrize(..., ids=["no-discount", "half", ...])` |
| Strict mypy gate | `mypy --strict money.py` and fix every note |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `bool` passes as price | `total([True]) == 1` instead of `TypeError` | Reject `bool` explicitly before `int` check |
| `SAVE10` goes negative | `final_price(5, "SAVE10") == -5` | Clamp with `max(0.0, ...)` |
| `save_receipt` writes to repo dir | Stray `receipt.txt` committed | Use only `tmp_path / "file.txt"` in tests |
| Coverage counts `tests/` | Gate passes but `money.py` is 60% | Read the per-file table, require 90%+ on `money.py` |

---
#### [Previous](03-tooling/exercises.md) | [Next](../README.md)
