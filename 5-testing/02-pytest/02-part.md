# Pytest Power — Parametrize, Fixtures, tmp_path, raises

Three nearly identical test functions are a smell. Pytest gives you four tools to kill duplication: `parametrize`, `fixture`, `tmp_path`, and `raises`.

## `@pytest.mark.parametrize`

Run the same test with different data:

```python
# tests/test_slug.py
import pytest

from cart import slug


@pytest.mark.parametrize(
    ("raw", "expected"),
    [
        ("Hello World", "hello-world"),
        ("", ""),
        ("  Hi  ", "hi"),
        ("A  B", "a--b"),
    ],
)
def test_slug(raw, expected):
    assert slug(raw) == expected
```

```bash
python -m pytest tests/test_slug.py -v
```

```text
tests/test_slug.py::test_slug[Hello World-hello-world] PASSED            [ 25%]
tests/test_slug.py::test_slug[-] PASSED                                  [ 50%]
tests/test_slug.py::test_slug[  Hi  -hi] PASSED                          [ 75%]
tests/test_slug.py::test_slug[A  B-a--b] PASSED                          [100%]

4 passed in 0.03s
```

Each row is an independent test with its own ID. One row failing does not hide the others.

| Rule | Detail |
|---|---|
| First arg is a string of param names | `"raw,expected"` or `("raw", "expected")` |
| Second arg is a list of tuples | One tuple per case |
| IDs come from values | Add `ids=[...]` for readable names when values are large |

> If you copy-paste a test function and change one number, stop — that is a `parametrize` table waiting to happen.

---

## `@pytest.fixture`

A fixture builds reusable test data. The test declares what it needs by naming the fixture as an argument:

```python
# tests/test_cart2.py
import pytest


@pytest.fixture
def prices():
    return [10, 20, 30]


def test_total(prices):
    assert sum(prices) == 60


def test_count(prices):
    assert len(prices) == 3
```

Each test gets a fresh return value (function scope by default), so mutation in one test cannot leak into the next.

Fixture scope cheat sheet:

| Scope | Meaning | Use for |
|---|---|---|
| `function` (default) | Fresh per test | Mutable lists, dicts |
| `module` | Once per file | Read-only sample data |
| `session` | Once per run | Expensive setup (DB, big file) |

```python
@pytest.fixture(scope="module")
def catalog():
    return {"apple": 2, "pear": 3}
```

---

## `tmp_path`: Real Files Without the Mess

`tmp_path` is a built-in fixture giving you an empty temporary directory (`pathlib.Path`) per test. Pytest deletes it after the run.

```python
# shop.py
from pathlib import Path


def save_receipt(path, lines):
    Path(path).write_text("\n".join(lines), encoding="utf-8")
```

```python
# tests/test_receipt.py
from shop import save_receipt


def test_save_receipt(tmp_path):
    target = tmp_path / "receipt.txt"
    save_receipt(target, ["apple 2", "total 2"])
    assert target.read_text(encoding="utf-8") == "apple 2\ntotal 2"
```

```bash
python -m pytest tests/test_receipt.py -v
```

```text
tests/test_receipt.py::test_save_receipt PASSED                          [100%]

1 passed in 0.03s
```

Never write test files into your repo directory — `tmp_path` keeps runs hermetic and parallel-safe.

---

## `pytest.raises`: Testing Exceptions

`pytest.raises` asserts that code raises, and captures the exception for inspection:

```python
# tests/test_age.py
import pytest

from shop import set_age


def test_negative_rejected():
    with pytest.raises(ValueError, match=">= 0"):
        set_age(-1)


def test_wrong_type_rejected():
    with pytest.raises(TypeError, match="must be int"):
        set_age("20")
```

```bash
python -m pytest tests/test_age.py -v
```

```text
tests/test_age.py::test_negative_rejected PASSED                         [ 50%]
tests/test_age.py::test_wrong_type_rejected PASSED                       [100%]

2 passed in 0.02s
```

Without `match`, any message passes. With `match` (a regex), you pin the message too. The `with` block must contain exactly the raising call — setup goes outside:

```python
def test_raises_layout():
    age = -1  # Arrange outside
    with pytest.raises(ValueError):  # Act + Assert together
        set_age(age)
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Forgetting `import pytest` | `NameError: parametrize` | Add `import pytest` at top of test file |
| Mutable fixture shared across tests | Tests pass alone, fail together | Keep default `function` scope; do not cache mutables at module level |
| `tmp_path` joined with `+` | `TypeError: unsupported operand` | Use `/` operator: `tmp_path / "file.txt"` |
| Code outside `raises` block | Test passes but proves nothing | Put only the raising line inside `with pytest.raises(...)` |
| `match` treated as exact string | Regex chars (`+`, `>=`) misbehave | Escape or keep match short: `match=">= 0"` works, `match="a+b"` needs `r"a\+b"` |

---

## Checklist

- [ ] You can convert 3 copy-pasted tests into one `parametrize` table
- [ ] You can write a `@pytest.fixture` and consume it as an argument
- [ ] You use `tmp_path` instead of writing into the repo
- [ ] Every `ValueError`/`TypeError` contract has a `pytest.raises` test

---
#### [Previous](01-part.md) | [Next](exercises.md)
