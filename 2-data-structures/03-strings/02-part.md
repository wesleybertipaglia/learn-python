# Strings Part 2 — Slicing, Searching, and Parsing

Slice, test, and parse text the idiomatic way.

## 1. Slicing strings

```python
s = "python"
print(s[0:4], s[:2], s[2:], s[::-1], s[::2])
print(s[-3:])
```

```text
pyth py thon nohtyp pto
hon
```

| Slice | Meaning |
|---|---|
| `s[:n]` | First n chars |
| `s[-n:]` | Last n chars |
| `s[::-1]` | Reversed |
| `s[a:b]` | `a` inclusive, `b` exclusive |

> Slicing never raises `IndexError` — `"hi"[10:]` is just `""`.

---

## 2. Membership and prefix tests

```python
text = "hello world"
print("world" in text, "mars" not in text)
print(text.startswith("hello"), text.endswith("world"))
print(text.find("o"), text.count("l"))
```

```bash
python string_search.py
```

```text
True True
True True
4 3
```

Prefer `in` and `startswith` over manual loops or regex for simple checks.

---

## 3. Parsing lines

```python
line = "name: ada, age: 36"
parts = [p.strip() for p in line.split(",")]
print(parts)
record = {}
for p in parts:
    k, v = p.split(":")
    record[k.strip()] = v.strip()
print(record)
```

```text
['name: ada', 'age: 36']
{'name': 'ada', 'age': '36'}
```

Pattern: `split` lines → `strip` → `split` on delimiter → `strip` again.

---

## 4. The join pattern

```python
words = ["python", "is", "fun"]
print(" ".join(words))
print("\n".join(words))
print(", ".join(f"{w!r}" for w in words))
csv_row = ["ada", "36", "london"]
print(",".join(csv_row))
```

```text
python is fun
python
is
fun
'python', 'is', 'fun'
ada,36,london
```

| Joiner | Output style |
|---|---|
| `" ".join` | Sentence |
| `"\n".join` | Multi-line block |
| `",".join` | CSV row |
| `"/".join` | Path-like |

---

## 5. Cleaning user input

```python
raw = "  YES \n"
clean = raw.strip().lower()
print(repr(clean), clean == "yes")
email = " Ada@Example.COM "
print(email.strip().lower())
```

```text
'yes' True
'ada@example.com'
```

Standard chain: `strip()` then `lower()` (or `casefold()` for aggressive matching).

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `s.split(",")` on spaces | Unsplit string | Use `split()` |
| Forgetting `strip()` | `" 42" != "42"` | Always `strip()` input |
| Case mismatch | `"Yes" != "yes"` | Compare `.lower()` forms |
| `find` returns `-1` | Treated as truthy | Check `!= -1` or use `in` |
| `join` on ints | `TypeError` | `map(str, ...)` first |

## Checklist

- [ ] Slice with `[:n]`, `[-n:]`, `[::-1]`
- [ ] Test with `in`, `startswith`, `endswith`
- [ ] Parse `key: value` lines with `split` + `strip`
- [ ] Build output with `" ".join` / `"\n".join`
- [ ] Normalize input with `strip().lower()`

#### [Previous](01-part.md) | [Next](exercises.md)
