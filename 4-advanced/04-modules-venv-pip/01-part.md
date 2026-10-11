# Imports and the Standard Library

Python ships with batteries. Learn `import`, `from`, `__main__`, then tour `datetime`, `collections`, `json`.

> Import modules, not copy-paste. Guard scripts with `if __name__ == "__main__":`.

## `import`, `from`, `__name__`

```python
# math_demo.py
import math
from pathlib import Path

print(math.sqrt(16))
print(Path("a") / "b.txt")
```

```text
4.0
a/b.txt
```

| Form | Code | Use |
|---|---|---|
| Whole module | `import math` | `math.sqrt(16)` |
| Specific names | `from pathlib import Path` | `Path(".")` |
| Alias | `import pandas as pd` | Shorten long names |

The `__name__` guard separates reuse from execution:

```python
# greeter.py
def hello(name: str) -> str:
    return f"hello, {name}!"

if __name__ == "__main__":
    print(hello("ana"))
```

```bash
python greeter.py
python -c "import greeter; print(greeter.hello('bo'))"
```

```text
hello, ana!
hello, bo!
```

Without the guard, importing `greeter` would print immediately. With it, import is silent and direct run executes.

## Stdlib Tour 1: `datetime`

```python
from datetime import date, datetime, timedelta

today = date(2026, 10, 11)
print(today)
print(today.isoformat())
print(datetime.now().strftime("%Y-%m-%d %H:%M"))
print(today + timedelta(days=7))
```

```text
2026-10-11
2026-10-11
2026-10-11 09:30
2026-10-18
```

Store dates as ISO strings (`2026-10-11`) in JSON/CSV. Parse with `date.fromisoformat(s)`.

## Stdlib Tour 2: `collections` and `json`

```python
from collections import Counter, defaultdict

votes = ["ana", "bo", "ana", "mia", "ana"]
print(Counter(votes))
print(Counter(votes).most_common(1))

balances: dict[str, float] = defaultdict(float)
balances["ana"] += 100.0
print(dict(balances))
```

```text
Counter({'ana': 3, 'bo': 1, 'mia': 1})
[('ana', 3)]
{'ana': 100.0}
```

```python
import json

raw = '{"owner": "ana", "balance": 10.5}'
print(json.loads(raw))
```

```text
{'owner': 'ana', 'balance': 10.5}
```

| Module | Gives you | Skip pip for |
|---|---|---|
| `datetime` | Dates, deltas, formatting | Date math |
| `collections` | `Counter`, `defaultdict`, `deque` | Counting, grouping |
| `json` | `dumps`/`loads`/`dump`/`load` | Configs, persistence |
| `pathlib` | `Path` | All file paths |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Missing `__main__` guard | Import prints/runs code | Wrap script code in guard |
| Circular imports | `ImportError` | Move shared code to third module |
| `from x import *` | Name clashes, unreadable | Import explicit names |
| Naive vs aware datetimes | Wrong comparisons | Use ISO strings + consistent tz |
| Shadowing stdlib (`json.py`) | `ImportError`, weird errors | Never name files `json.py`, `random.py` |

## Checklist

- [ ] I can explain `import` vs `from` vs alias
- [ ] My scripts use `if __name__ == "__main__":`
- [ ] I can format and parse ISO dates
- [ ] I can count with `Counter` and group with `defaultdict`
- [ ] I never name my file after a stdlib module

#### [Previous](../03-file-io/exercises.md) | [Next](./02-part.md)
