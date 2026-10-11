# Exceptions — try/except/else/finally

Programs crash on bad input. Exceptions let you survive it.

## Errors vs Exceptions

- **SyntaxError**: code never ran (missing colon, bad indent). Fix the text.
- **Exception**: code ran but hit a runtime problem (bad value, missing key). Catch it.

```python
print(int("thirty"))
```

```text
Traceback (most recent call last):
  File "...", line 1, in <module>
ValueError: invalid literal for int() with base 10: 'thirty'
```

The traceback is Python telling you exactly what failed. Lesson 09 teaches full traceback reading — for now, read the **last line first**: `ValueError: ...`.

---

## `try` / `except`: Catch and Recover

```python
text = input("Age? ")
try:
    age = int(text)
    print(f"Next year: {age + 1}")
except ValueError:
    print(f"'{text}' is not a number")
```

```text
Age? thirty
'thirty' is not a number
```

```text
Age? 30
Next year: 31
```

| Clause | Runs when |
|---|---|
| `try:` | Always (attempt the risky code) |
| `except ValueError:` | Only if that error occurs |
| `except (ValueError, TypeError):` | If any listed error occurs |

Catch **specific** errors. `except ValueError` says "I know what can go wrong here". A bare `except:` (part 2) hides bugs you need to see.

---

## `else` and `finally`: The Full Shape

```python
def parse_age(text):
    try:
        age = int(text)
    except ValueError:
        print(f"bad age: {text!r}")
        return None
    else:
        print("parsed ok")
        return age
    finally:
        print("attempt finished")

print(parse_age("30"))
print("---")
print(parse_age("abc"))
```

```text
parsed ok
attempt finished
30
---
bad age: 'abc'
attempt finished
None
```

| Clause | Meaning |
|---|---|
| `else:` | Runs only if NO exception (success path) |
| `finally:` | Always runs (cleanup: close files, release locks) |

Use `else` to keep the `try` block minimal — only the line that can actually fail goes in `try`.

---

## The Common Builtins You Must Recognize

| Exception | When | Example |
|---|---|---|
| `ValueError` | Right type, wrong value | `int("abc")` |
| `TypeError` | Wrong type entirely | `"a" + 1` |
| `KeyError` | Missing dict key | `d["nope"]` |
| `IndexError` | Bad list/string index | `[1][5]` |
| `ZeroDivisionError` | Divide by zero | `1 / 0` |
| `FileNotFoundError` | Missing file | `open("nope.txt")` |

```python
try:
    print(int("abc"))
except ValueError:
    print("value problem")
except TypeError:
    print("type problem")
```

Order matters only when exceptions overlap (subclasses). For the flat list above, any order works — but put the most specific first as a habit.

> Catch what you can handle, let the rest propagate. Swallowing `KeyError` when you expected `ValueError` turns a loud, fixable crash into silent wrong behavior.

---

## Bugs You Will Write

| Bug | Symptom | Fix |
|---|---|---|
| Bare `except:` | Hides `KeyboardInterrupt`, typos, everything | `except ValueError:` — always name it |
| Giant `try:` block | Cannot tell which line failed | Wrap only the risky line |
| `except` that does nothing (`pass`) | Bug silently ignored | Log or report; never silent `pass` in course code |
| Confusing `ValueError` vs `TypeError` | Wrong handler | `int("abc")` → Value, `"a" + 1` → Type |

---

## Checklist

- [ ] I read tracebacks bottom-up (last line = the error)
- [ ] My `try` blocks wrap one risky line, not ten
- [ ] I catch specific exceptions, never bare `except:`
- [ ] I can use `else` (success) and `finally` (always) correctly

---
#### [Previous](../07-functions/exercises.md) | [Next](02-part.md)
