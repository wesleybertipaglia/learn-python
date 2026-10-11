# Exercises — Getting Started

No new concepts. Just prove your machine works.

## 1. Version Report

```bash
python --version
```

(On Linux/macOS use `python3 --version` if `python` fails.) It must say 3.12+. If not, fix your install (`01-part.md`) before continuing.

```text
Python 3.12.x
```

## 2. REPL Experiments

In the REPL (`python`, no args), run each line and note the output:

```python
2 + 3 * 4
```

```python
"py" + "thon"
```

```python
len("hello")
```

Answer: why is the first not `20`? What does `len()` do? Type `exit()` to leave.

## 3. Hello Script

Create `hello.py`, run it with `python hello.py`:

```python
print("hello, world")
```

Then delete the closing quote, run again, read the error, and undo. Getting comfortable with errors now saves hours later.

## 4. Name Greeter

Create `greeter.py`:

```python
name = input("What is your name? ")
print(f"Hello, {name}!")
```

| Input | Expected output |
|---|---|
| `ada` | `Hello, ada!` |
| `linus` | `Hello, linus!` |

## 5. Age Next Year (Stretch)

Ask for an age, print next year's age with `int()`. Typing `thirty` should crash with `ValueError` — correct for now; lesson 08 teaches graceful handling.

```text
Age? 30
Next year you will be 31
```

---

## Checklist

- [ ] `python --version` reports 3.12+
- [ ] `hello.py` and `greeter.py` run correctly
- [ ] I have seen (and recovered from) one `SyntaxError`

---
#### [Previous](02-part.md) | [Next](../02-syntax/01-part.md)
