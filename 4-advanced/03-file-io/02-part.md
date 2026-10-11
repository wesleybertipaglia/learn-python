# CSV, JSON, and Errors

`csv` for tables, `json` for nested data. Handle `FileNotFoundError` where files are optional.

> CSV for spreadsheets, JSON for APIs and persistence. Both are in the stdlib — no pip needed.

## CSV Module

```python
import csv
from pathlib import Path

p = Path("/tmp/opencode/users.csv")
p.parent.mkdir(parents=True, exist_ok=True)
with open(p, "w", newline="", encoding="utf-8") as f:
    w = csv.DictWriter(f, fieldnames=["name", "balance"])
    w.writeheader()
    w.writerow({"name": "ana", "balance": 100})
    w.writerow({"name": "bo", "balance": 250})

with open(p, newline="", encoding="utf-8") as f:
    for row in csv.DictReader(f):
        print(row)
```

```text
{'name': 'ana', 'balance': '100'}
{'name': 'bo', 'balance': '250'}
```

Notes:

- Always `newline=""` with `csv`, or you get blank rows on Windows.
- Values come back as strings. Convert with `int(row["balance"])`.
- `DictReader`/`DictWriter` beat index-based `reader` for clarity.

| Mode | Code | Returns |
|---|---|---|
| Write dicts | `csv.DictWriter(f, fieldnames=[...])` | CSV file |
| Read dicts | `csv.DictReader(f)` | `dict` per row |
| Write rows | `csv.writer(f)` | CSV file |
| Read rows | `csv.reader(f)` | `list` per row |

## JSON Dump and Load

```python
import json
from pathlib import Path

p = Path("/tmp/opencode/account.json")
data = {"owner": "ana", "balance": 120.5, "tags": ["savings"]}
p.write_text(json.dumps(data, indent=2), encoding="utf-8")
print(p.read_text(encoding="utf-8"))

loaded = json.loads(p.read_text(encoding="utf-8"))
print(loaded["owner"], loaded["balance"])
```

```text
{
  "owner": "ana",
  "balance": 120.5,
  "tags": [
    "savings"
  ]
}
ana 120.5
```

For direct file objects use `dump`/`load`:

```python
import json

with open("/tmp/opencode/account.json", encoding="utf-8") as f:
    print(json.load(f))
```

```text
{'owner': 'ana', 'balance': 120.5, 'tags': ['savings']}
```

Inspect JSON from the shell:

```bash
python -c "import json; print(json.dumps({'a': 1}, indent=2))"
```

```text
{
  "a": 1
}
```

## Error Handling: `FileNotFoundError`

```python
import json
from pathlib import Path

def load_or_default(path: str) -> dict:
    p = Path(path)
    try:
        return json.loads(p.read_text(encoding="utf-8"))
    except FileNotFoundError:
        return {"owner": "new", "balance": 0.0}
    except json.JSONDecodeError:
        print("corrupt file, starting fresh")
        return {"owner": "new", "balance": 0.0}

print(load_or_default("/tmp/opencode/missing.json"))
```

```text
{'owner': 'new', 'balance': 0.0}
```

Catch specific exceptions, in order from specific to broad. Never bare `except:` — it hides bugs.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Missing `newline=""` in csv | Blank lines on Windows | `open(p, "w", newline="")` |
| Forgetting JSON values are typed | `balance` read as str from CSV | Convert: `float(row["balance"])` |
| Bare `except:` | Swallows `KeyboardInterrupt`, bugs | `except FileNotFoundError:` |
| `json.load` on empty file | `JSONDecodeError` | Handle it, return default |
| Writing without `indent=2` | One giant line, hard diff | `json.dumps(data, indent=2)` |

## Checklist

- [ ] I can round-trip a dict through CSV
- [ ] I can round-trip a dict through JSON with `dump`/`load`
- [ ] I use `newline=""` for CSV and `encoding="utf-8"` everywhere
- [ ] I handle `FileNotFoundError` with a sensible default
- [ ] I never use bare `except:`

#### [Previous](./01-part.md) | [Next](./exercises.md)
