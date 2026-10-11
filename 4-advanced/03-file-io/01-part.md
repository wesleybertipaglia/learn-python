# Files with pathlib and with

Use `pathlib.Path` for paths and `with open(...)` for files. Files always close, encodings always explicit.

> Hardcode no paths. Use `Path`, use `with`, pass `encoding="utf-8"`.

## `pathlib.Path` Essentials

```python
from pathlib import Path

p = Path("notes/hello.txt")
print(p.parent)
print(p.name)
print(p.suffix)
print(Path.cwd())
```

```text
notes
hello.txt
.txt
/current/working/dir
```

Common operations:

| Operation | Code |
|---|---|
| Join | `Path("data") / "out.txt"` |
| Exists | `p.exists()` |
| Make dirs | `p.parent.mkdir(parents=True, exist_ok=True)` |
| List dir | `list(Path(".").iterdir())` |
| Home | `Path.home()` |

Never concatenate paths with `+ "/" +`. The `/` operator handles OS separators.

Check your layout:

```bash
ls -R . && python -c "from pathlib import Path; print(list(Path('.').iterdir()))"
```

```text
./notes/hello.txt
[PosixPath('notes/hello.txt')]
```

## `open` + `with`, Text Read/Write

```python
from pathlib import Path

p = Path("/tmp/opencode/demo.txt")
p.parent.mkdir(parents=True, exist_ok=True)
p.write_text("hello\nworld\n", encoding="utf-8")
print(p.read_text(encoding="utf-8"))
```

```text
hello
world
```

For larger control, use `open`:

```python
from pathlib import Path

p = Path("/tmp/opencode/demo2.txt")
with open(p, "w", encoding="utf-8") as f:
    f.write("ana\nbo\n")

with open(p, encoding="utf-8") as f:
    for line in f:
        print(line.strip().upper())
```

```text
ANA
BO
```

`with` closes the file even on exceptions. Manual `f.close()` is a bug source.

## Encodings: Always UTF-8

```python
from pathlib import Path

p = Path("/tmp/opencode/unicode.txt")
p.write_text("héllo — üñîcodé ✓\n", encoding="utf-8")
print(p.read_text(encoding="utf-8"))
```

```text
héllo — üñîcodé ✓
```

| Rule | Why |
|---|---|
| Always `encoding="utf-8"` | Defaults vary by OS, breaks on Windows |
| `write_text` for small files | One-liner, auto close |
| `open + with` for streaming | Constant memory |
| `\n` newlines, strip on read | Avoid `"\r\n"` surprises |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| No `encoding=` | `UnicodeDecodeError` on other machines | Always `encoding="utf-8"` |
| Missing `with` | Leaked file handle, locked file | Wrap every `open` in `with` |
| String path concat | Broken on Windows | `Path(a) / b` |
| Missing parent dir | `FileNotFoundError` on write | `p.parent.mkdir(parents=True, exist_ok=True)` |
| `read()` on huge file | `MemoryError` | Iterate `for line in f:` |

## Checklist

- [ ] I build paths with `Path / "name"`
- [ ] I read/write small files with `read_text` / `write_text`
- [ ] I stream large files with `with open(...)`
- [ ] I always pass `encoding="utf-8"`
- [ ] I create parent dirs before writing
- [ ] I can explain why `with` guarantees closing on errors

> Done with files? Next: structured CSV and JSON with proper error handling.

#### [Previous](../02-functions-deep-dive/exercises.md) | [Next](./02-part.md)
