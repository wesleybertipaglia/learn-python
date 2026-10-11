# Exercises — Pytest Tables, Fixtures, Errors

Use `python -m pytest -v`. All files go in `tests/`.

> If a test needs copy-paste, it needs `parametrize`. If two tests share setup, it needs a fixture.

---

## 1. Parametrize shipping

Given:

```python
def shipping(weight):
    return 0 if weight <= 0 else 5 + weight * 2
```

Write one parametrized test with at least 4 rows:

| weight | expected |
|---|---|
| 0 | 0 |
| 1 | 7 |
| 2 | 9 |
| -3 | 0 |

Expected:

```bash
python -m pytest tests/test_shipping.py -v
```

```text
4 passed in 0.03s
```

---

## 2. Fixture + tmp_path receipt

Write a `@pytest.fixture` returning `["apple 2", "total 2"]`, plus a test that writes those lines to `tmp_path / "receipt.txt"` with `write_text` and reads them back. No files in the repo root.

---

## 3. Raises for `set_age`

Given `set_age` from the intro (raises `TypeError` for non-int, `ValueError` for negative), write two `pytest.raises` tests with `match` on the message.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Test file not `test_*.py` | Collected 0 tests | Rename to `tests/test_*.py` |
| Fixture not imported/defined | `fixture 'x' not found` error | Define `@pytest.fixture def x()` in same file |
| Asserting outside `raises` block | False green test | Keep raising call inside the `with` block |

---

## Checklist

- [ ] `python -m pytest -v` shows each parametrize row separately
- [ ] Fixture test and `tmp_path` test both pass
- [ ] Both `raises` tests use `match=`

---
#### [Previous](02-part.md) | [Next](../03-tooling/01-part.md)
