# Venv, pip, and Project Layout

One project, one virtual environment. Pin dependencies. Lay out code like a pro from day one.

> Never `pip install` into system Python. Always venv first, then install.

## Creating a venv

```bash
python -m venv .venv
source .venv/bin/activate
python --version
which python
```

```text
Python 3.12.3
.../myproject/.venv/bin/python
```

| Step | Command | Note |
|---|---|---|
| Create | `python -m venv .venv` | Once per project |
| Activate (Linux/macOS) | `source .venv/bin/activate` | Prompt shows `(.venv)` |
| Activate (Windows) | `.venv\Scripts\activate` | Different path |
| Leave | `deactivate` | Back to system |
| Ignore | Add `.venv/` to `.gitignore` | Never commit it |

Verify you are inside:

```bash
which python && pip --version
```

```text
.../.venv/bin/python
pip 24.0 from .../.venv/lib/python3.12/site-packages
```

If the path lacks `.venv`, you forgot to activate.

## `pip` and `requirements.txt`

```bash
pip install requests
pip freeze > requirements.txt
cat requirements.txt
pip install -r requirements.txt
```

```text
requests==2.32.3
requests==2.32.3
Collecting requests==2.32.3
Successfully installed requests-2.32.3
```

Workflow:

1. Activate venv.
2. `pip install <pkg>` while developing.
3. `pip freeze > requirements.txt` before committing.
4. Teammate runs `pip install -r requirements.txt` to reproduce.

```python
# quick smoke test that requests imports
import requests
print(requests.__version__)
```

```text
2.32.3
```

## Project Layout and `pyproject.toml`

Minimal professional layout:

```text
myproject/
  src/myapp/__init__.py
  src/myapp/accounts.py
  tests/test_accounts.py
  pyproject.toml
  requirements.txt
  README.md
```

Minimal `pyproject.toml` (Python 3.12+):

```toml
[project]
name = "myapp"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["requests>=2.32"]

[tool.pytest.ini_options]
testpaths = ["tests"]
```

Run tests from the project root:

```bash
python -m pytest -q
```

```text
1 passed in 0.02s
```

| File | Purpose |
|---|---|
| `src/myapp/*.py` | Real code, importable as `myapp.accounts` |
| `tests/test_*.py` | Tests, mirror module names |
| `pyproject.toml` | Metadata + build config |
| `requirements.txt` | Pinned snapshot for reproducibility |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Installing globally | Breaks system tools | Always activate `.venv` first |
| Committing `.venv/` | Giant repo, conflicts | Add to `.gitignore` |
| Unpinned deps | Works today, breaks tomorrow | `pip freeze > requirements.txt` |
| `src` not importable | `ModuleNotFoundError` | Run from root, add `__init__.py` |
| Two Pythons confusion | `pip` vs `pip3` mismatch | Use `python -m pip install ...` |

## Checklist

- [ ] I can create and activate a venv
- [ ] `which python` points inside `.venv`
- [ ] I can install, freeze, and reinstall from `requirements.txt`
- [ ] My project has `src/`, `tests/`, `pyproject.toml`
- [ ] `.venv/` is git-ignored

#### [Previous](./01-part.md) | [Next](./exercises.md)
