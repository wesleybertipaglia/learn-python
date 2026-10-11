# Exercises: File I/O

> Paths, text, CSV, and JSON with real files in `/tmp`.

## Task 1: Round-Trip Text

Write `"hello"` to `/tmp/opencode/ex1.txt` with UTF-8, read it back.

```python
from pathlib import Path

p = Path("/tmp/opencode/ex1.txt")
p.parent.mkdir(parents=True, exist_ok=True)
p.write_text("hello\n", encoding="utf-8")
print(p.read_text(encoding="utf-8"))
```

```text
Expected: hello
```

## Task 2: JSON Save/Load

Save `{"owner": "ana", "balance": 10}` to JSON, load it, assert equality.

```python
import json
# use json.dumps + write_text, then json.loads + read_text
```

```text
Expected: dicts equal, owner == ana
```

List your work:

```bash
ls /tmp/opencode/ && echo "--- files above ---"
```

```text
ex1.txt
account.json
```

| Exercise | Focus | Hint |
|---|---|---|
| Text | `Path` + encoding | `write_text(..., encoding="utf-8")` |
| JSON | `dump`/`load` | `indent=2` for readable files |
| Missing file | `try/except` | Catch `FileNotFoundError` |

## Task 3: Missing File

Write `load_or_default()` returning `{}` when the file does not exist.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| No parent dir | `FileNotFoundError` on write | `mkdir(parents=True, exist_ok=True)` |
| No encoding | Decode error | `encoding="utf-8"` |
| Empty JSON | `JSONDecodeError` | Handle it |

## Checklist

- [ ] Text round-trip works
- [ ] JSON round-trip preserves data
- [ ] Missing file returns default, no crash

#### [Previous](./02-part.md) | [Next](../04-modules-venv-pip/01-part.md)
