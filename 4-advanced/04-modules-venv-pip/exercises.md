# Exercises: Modules and Environments

> Guard scripts, use stdlib, and build a clean venv.

## Task 1: Main Guard

Write `greeter.py` with `hello()` and a `__main__` guard. Import must be silent.

```python
def hello(name: str) -> str:
    return f"hello, {name}!"
```

```text
Expected: python greeter.py prints hello, ana!; import is silent
```

## Task 2: Counter Words

Use `collections.Counter` to find the most common word in a list.

```python
from collections import Counter
print(Counter(["a", "b", "a"]).most_common(1))
```

```text
Expected: [('a', 2)]
```

Setup check:

```bash
python -m venv .venv && source .venv/bin/activate && python --version
```

```text
Python 3.12.3
```

| Exercise | Focus | Hint |
|---|---|---|
| Guard | `__main__` | Print only under guard |
| Counter | Stdlib | `most_common(1)` |
| Venv | Isolation | `which python` shows `.venv` |

## Task 3: Freeze

Install one package, run `pip freeze > requirements.txt`, reinstall with `-r`.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Code runs on import | Print at import | Move under guard |
| File named `json.py` | Broken imports | Rename file |
| Global install | Polluted system | Use venv |

## Checklist

- [ ] Import is silent, direct run prints
- [ ] Counter returns correct top word
- [ ] requirements.txt reproduces env

#### [Previous](./02-part.md) | [Next](../challenge.md)
