# Exercises — Coverage, Ruff, Mypy

Run everything from the repo root with `python -m pytest`.

> Green tests with red lint is still red. Ship all four greens: pytest, coverage, ruff, mypy.

---

## 1. Close the gap to 90%+

Given a module with an untested branch, run:

```bash
python -m pytest --cov=cart --cov-report=term-missing
```

```text
cart.py       6      2    66%   4-5
```

Add the missing test(s) until `--cov-fail-under=90` passes:

```bash
python -m pytest --cov=cart --cov-fail-under=90 -q
```

```text
1 passed, coverage: 100%
```

---

## 2. Ruff clean

Run `ruff check .` and `ruff format --check .`. Fix findings, re-run until `All checks passed!`:

```bash
ruff check .
ruff check . --fix
ruff format .
```

---

## 3. Mypy on one module

Add annotations (`list[float] -> float`, `str -> str`) to two functions, run `mypy .`, and fix the first error without `# type: ignore`.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Wrong `--cov` target | Empty report | Use source package name, not `tests` |
| Ignoring mypy with comments | Hidden type bug ships | Annotate signature, fix call site |
| Format-only diffs in PR | Noisy review | Run `ruff format .` before commit |

---

## Checklist

- [ ] Coverage gate `--cov-fail-under=90` passes
- [ ] `ruff check .` and `ruff format --check .` pass
- [ ] `mypy .` reports no errors

---
#### [Previous](02-part.md) | [Next](../challenge.md)
