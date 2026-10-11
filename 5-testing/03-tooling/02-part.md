# Tooling II — Ruff, Mypy, and CI

Tests catch wrong behavior. Linters catch wrong style, type checkers catch wrong assumptions, CI runs all of it on every push.

## Ruff: Lint + Format in One Binary

Ruff replaces flake8 + black + isort with a single fast tool.

```bash
pip install ruff
```

Check (lint):

```bash
ruff check .
```

```text
All checks passed!
```

A failure looks like this:

```text
cart.py:1:1: F401 `os` imported but unused
Found 1 error.
```

Fix the import, re-run. Common loop:

```bash
ruff check . --fix
```

Format (replaces black):

```bash
ruff format .
```

```text
2 files reformatted
```

Minimal config in `pyproject.toml`:

```toml
# pyproject.toml
[tool.ruff]
line-length = 100
```

```bash
cat pyproject.toml
```

```text
[tool.ruff]
line-length = 100
```

| Command | Purpose |
|---|---|
| `ruff check .` | Find unused imports, undefined names, style bugs |
| `ruff check . --fix` | Auto-fix what is safe |
| `ruff format .` | Normalize formatting (run after fix) |
| `ruff format --check .` | CI mode: fail if anything needs formatting |

> Run `ruff format` before every commit. Arguing about quotes and blank lines is a waste of human review time.

---

## Mypy Basics: Types as Cheap Tests

Python is dynamically typed; mypy adds a static check for annotated code.

```bash
pip install mypy
```

Annotate the boundary (public functions), let inference handle the rest:

```python
# cart.py
def total(prices: list[float]) -> float:
    return sum(prices)


def slug(title: str) -> str:
    return title.strip().lower().replace(" ", "-")
```

```bash
mypy cart.py
```

```text
Success: no issues found in 1 source file
```

A caught bug:

```python
# oops.py
def total(prices: list[float]) -> float:
    return sum(prices)


total("abc")  # type error: str is not list[float]
```

```bash
mypy oops.py
```

```text
oops.py:5: error: Argument 1 to "total" has incompatible type "str"; expected "list[float]"  [arg-type]
Found 1 error in 1 file (checked 1 source file)
```

Practical rules:

| Do | Do not |
|---|---|
| Annotate public function signatures | Annotate every loop variable |
| Fix mypy errors instead of `# type: ignore` | Silence errors to get green CI |
| Run `mypy .` on typed code | Expect mypy to catch logic bugs (it will not) |

---

## CI Snippet: GitHub Actions Minimal

This workflow runs on every push: install, lint, type-check, test with coverage gate.

```yaml
# .github/workflows/ci.yml
name: ci
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install pytest pytest-cov ruff mypy
      - run: ruff check .
      - run: ruff format --check .
      - run: mypy .
      - run: python -m pytest --cov=. --cov-fail-under=90 -q
```

Local equivalent before pushing:

```bash
ruff check . && ruff format --check . && mypy . && python -m pytest --cov=. --cov-fail-under=90 -q
```

```text
All checks passed!
Success: no issues found in 1 source file
1 passed, coverage: 100%
```

Order matters: fast checks (ruff) first, slow suite last so you fail in seconds, not minutes.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `ruff` not in CI requirements | CI passes locally, fails remotely | Pin dev deps: `pip install pytest pytest-cov ruff mypy` in workflow |
| `mypy` errors on untyped defs | Flood of `no-untyped-def` notes | Start strict only on new files; add annotations incrementally |
| CI runs bare `pytest` | Wrong interpreter, missing cov gate | Always `python -m pytest --cov=. --cov-fail-under=90` |
| Formatting-only CI failures | PR blocked by quotes/spaces | Run `ruff format .` locally before push |

---

## Checklist

- [ ] `ruff check .` and `ruff format --check .` pass
- [ ] `mypy .` passes on annotated code
- [ ] One local command runs lint + types + tests with coverage gate
- [ ] CI yaml runs the same four steps on push

---
#### [Previous](01-part.md) | [Next](exercises.md)
