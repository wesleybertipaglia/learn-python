# Challenge — Log Analyzer + Notes CLI

Build an installable CLI that streams a log file lazily, times its work with a decorator, and persists notes as CSV and JSON. It ties together all four advanced topics in one shippable project.

> You are done when a stranger can create a venv, run `pip install -e .`, and analyze a huge log without blowing up memory.

---

## 1. Project Layout

```bash
mkdir -p lognotes/src/lognotes/data && ls -R lognotes
```

```text
lognotes/
  pyproject.toml
  src/lognotes/__init__.py
  src/lognotes/analyzer.py
  src/lognotes/timing.py
  src/lognotes/store.py
  src/lognotes/cli.py
  data/app.log
```

Minimal `pyproject.toml`:

```text
[project]
name = "lognotes"
version = "0.1.0"
requires-python = ">=3.12"
[project.scripts]
lognotes = "lognotes.cli:main"
```

| File | Owns | Must use |
|---|---|---|
| `analyzer.py` | Lazy log streaming | Generator + `yield` |
| `timing.py` | `@timed` decorator | `functools.wraps` |
| `store.py` | Notes CSV/JSON persistence | `pathlib`, `csv`, `json` |
| `cli.py` | `main()` + `__main__` guard | Imports only, no duplicated logic |

A simple `@dataclass` for a parsed line is fine. Do not invent a class hierarchy — OOP was module 3.

---

## 2. Venv + Install

```bash
cd lognotes && python -m venv .venv && source .venv/bin/activate
pip install -e . && which python
```
| Step | Command | Proves |
|---|---|---|
| Create + activate | `python -m venv .venv`, `source .venv/bin/activate` | `which python` shows `.venv` |
| Install | `pip install -e .` | `lognotes` command appears |
| Freeze | `pip freeze > requirements.txt` | Env is reproducible |

> Never commit `.venv/` or `__pycache__/`. That is what `.gitignore` is for.

---

## 3. Stream Logs with a Generator

`data/app.log`:

```text
2026-10-11T09:00:01 INFO server started
2026-10-11T09:00:07 ERROR disk full on /dev/sda1
2026-10-11T09:00:12 ERROR timeout after 30s
```

Stream it — never `read().splitlines()` the whole file:

```python
from collections.abc import Iterator
from dataclasses import dataclass
from pathlib import Path

@dataclass
class LogLine:
    timestamp: str
    level: str
    message: str

def stream_lines(path: Path) -> Iterator[str]:
    with open(path, encoding="utf-8") as f:
        for line in f:
            if line.strip():
                yield line.strip()

def parse(line: str) -> LogLine:
    ts, level, message = line.split(" ", 2)
    return LogLine(ts, level, message)

def errors_only(path: Path) -> Iterator[LogLine]:
    for raw in stream_lines(path):
        entry = parse(raw)
        if entry.level == "ERROR":
            yield entry

def count_levels(path: Path) -> dict[str, int]:
    from collections import Counter
    return dict(Counter(parse(r).level for r in stream_lines(path)))
```

```bash
python -c "from pathlib import Path; from lognotes.analyzer import count_levels; print(count_levels(Path('data/app.log')))"
```
```text
{'INFO': 1, 'ERROR': 2}
```
| Rule | Why |
|---|---|
| `yield` per line | Constant memory on huge files |
| `with open(...)` | Closes even on parse errors |
| Genexpr into `Counter` | No intermediate list |

---

## 4. Time It with a Decorator

```python
import functools
import time

def timed(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.perf_counter() - start:.3f}s")
        return result
    return wrapper
```
| Requirement | Check |
|---|---|
| `@functools.wraps` | `wrapped.__name__` preserved |
| Forwards `*args, **kwargs` | Works on any signature |
| Returns the value | Decorated result equals plain result |

> A decorator that loses `__name__` or swallows the return value is broken — `@wraps` and `return result` are non-negotiable.

---

## 5. Notes in CSV + JSON

Missing files must not crash:

```python
import csv, json
from pathlib import Path

def save_notes_csv(path: Path, rows: list[dict]) -> None:
    path.parent.mkdir(parents=True, exist_ok=True)
    with open(path, "w", newline="", encoding="utf-8") as f:
        w = csv.DictWriter(f, fieldnames=["title", "body"])
        w.writeheader()
        w.writerows(rows)

def load_notes_csv(path: Path) -> list[dict]:
    try:
        with open(path, newline="", encoding="utf-8") as f:
            return list(csv.DictReader(f))
    except FileNotFoundError:
        return []

def save_notes_json(path: Path, rows: list[dict]) -> None:
    path.parent.mkdir(parents=True, exist_ok=True)
    path.write_text(json.dumps(rows, indent=2), encoding="utf-8")

def load_notes_json(path: Path) -> list[dict]:
    try:
        return json.loads(path.read_text(encoding="utf-8"))
    except (FileNotFoundError, json.JSONDecodeError):
        return []
```

| Case | Behavior |
|---|---|
| Missing file | Return `[]`, no traceback |
| Corrupt JSON | Return `[]` via `JSONDecodeError` |
| Missing parent dir | `mkdir(parents=True, exist_ok=True)` |

---

## 6. Wire the CLI

```python
from pathlib import Path
from lognotes.analyzer import count_levels, errors_only
from lognotes.store import load_notes_csv, save_notes_csv
from lognotes.timing import timed

@timed
def summarize(path: Path) -> dict[str, int]:
    return count_levels(path)

def main() -> None:
    import argparse
    p = argparse.ArgumentParser(prog="lognotes")
    sub = p.add_subparsers(dest="cmd", required=True)
    sub.add_parser("summary").add_argument("logfile")
    add = sub.add_parser("add-note")
    add.add_argument("--title", required=True)
    add.add_argument("--body", required=True)
    args = p.parse_args()
    if args.cmd == "summary":
        print(summarize(Path(args.logfile)))
        for e in errors_only(Path(args.logfile)):
            print(e.timestamp, e.message)
    else:
        db = Path("data/notes.csv")
        rows = load_notes_csv(db)
        rows.append({"title": args.title, "body": args.body})
        save_notes_csv(db, rows)
        print(f"saved note {args.title!r}")

if __name__ == "__main__":
    main()
```

```bash
lognotes summary data/app.log
lognotes add-note --title "disk" --body "buy bigger disk"
```
```text
summarize took 0.002s
{'INFO': 1, 'ERROR': 2}
2026-10-11T09:00:07 disk full on /dev/sda1
saved note 'disk'
```

---

## 7. Example Transcript

```text
$ source .venv/bin/activate && pip install -e .
Successfully installed lognotes-0.1.0
$ lognotes summary data/app.log
summarize took 0.002s
{'INFO': 1, 'ERROR': 2}
$ lognotes add-note --title "disk" --body "buy bigger disk"
saved note 'disk'
```

---

## 8. Tips

- Assert `summarize.__name__ == "count_levels"` to catch a missing `@wraps`.
- Keep I/O at the edges: `parse` stays pure; only `stream_lines` and `store.py` touch disk.
- Never name a file `json.py` or `csv.py` — it shadows the stdlib.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `read_text().splitlines()` | Memory spike on big files | `yield` line-by-line with `with open` |
| No `@wraps` | `__name__` is `wrapper` | Add `@functools.wraps(func)` |
| Dropped return | `None` instead of dict | `return result` in wrapper |
| No `encoding=` | `UnicodeDecodeError` elsewhere | Always `encoding="utf-8"` |
## Checklist (Grading)

- [ ] `stream_lines` uses `yield` + `with open`, constant memory; `errors_only` filters `ERROR`
- [ ] `@timed` uses `functools.wraps`, forwards args, returns value, prints time
- [ ] CSV round-trip via `pathlib` + `csv` (`newline=""`, UTF-8); JSON via `json` (`indent=2`)
- [ ] Missing/corrupt files return `[]` (`FileNotFoundError` handled)
- [ ] `src/lognotes/` + minimal `pyproject.toml`, venv + `pip install -e .` works
- [ ] `lognotes summary` and `add-note` work from the installed entry point
- [ ] Import is silent; `__main__` guard present
## Extras (pick 1–2)

- [ ] `--json` flag on `summary` writing counts via `save_notes_json`
- [ ] `--level INFO` filter flag reusing the generator pipeline
---
#### [Previous](04-modules-venv-pip/exercises.md) | [Next](../5-testing/01-introduction/01-part.md)
