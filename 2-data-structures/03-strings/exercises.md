# Strings — Exercises

Practice methods, f-strings, slicing, and parsing.

## Setup

```bash
mkdir -p strings_ex && cd strings_ex
python --version
```

```python
print(f"{'ada':>10}|")
```

```text
       ada|
```

| Exercise | Focus | Hint |
|---|---|---|
| 1 | strip/split | `strip().split()` |
| 2 | f-string | `:.2f` |
| 3 | Slicing | `[::-1]` |
| 4 | Parsing | `split(":")` |
| 5 | join | `"\n".join` |

> Strings are immutable — every method returns a new string you must assign.

---

## 1. Cleanup crew
Given `"  Ada LOVELACE \n"`, strip it, title-case it, and print `repr` before/after. Count the letter `a` case-insensitively.

## 2. Receipt formatter
Given `item="coffee", price=3.5, qty=2`, print `f"coffee x2 = $7.00"` using `:.2f` and alignment `:>10` for price.

## 3. Palindrome slice
Check if `"racecar"` equals its `[::-1]`. Test `"python"` too. Print both booleans and first-3/last-3 slices.

## 4. Config parser
Parse `"host: localhost, port: 8000"` into `{"host": "localhost", "port": "8000"}` using `split(",")` then `split(":")` with `strip()`.

## 5. Join report
From `["ada", "grace", "linus"]` build: comma line, newline block, and `" | ".join(...)`. Print all three.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| No reassign | Change lost | `s = s.strip()` |
| Wrong split sep | No split | Match delimiter |
| Case-sensitive compare | Missed match | `.lower()` both sides |
| Format without `f` | Literal `{x}` | Prefix `f` |
| Join non-str | `TypeError` | `map(str, xs)` |

## Checklist

- [ ] All 5 scripts print expected output
- [ ] f-strings use specs correctly
- [ ] Slicing has no `IndexError`
- [ ] Parser strips whitespace
- [ ] Joins produce 3 layouts

#### [Previous](02-part.md) | [Next](../04-dicts/01-part.md)
