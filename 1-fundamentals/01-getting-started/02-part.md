# Getting Started — REPL vs Scripts

There are two ways to run Python: throwaway experiments (REPL) and saved programs (scripts). You need both.

## The REPL: Your Scratch Pad

REPL = Read, Eval, Print, Loop. Type a line, Python runs it instantly.

Start it:

```bash
python
```

```text
Python 3.12.4 (main, ...) on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

Try this:

```python
2 + 3
```

```text
5
```

```python
name = "ana"
print(f"hello {name}")
```

```text
hello ana
```

Exit with `exit()` or Ctrl+D (Ctrl+Z on Windows).

Why the REPL exists:

| Use REPL for | Do NOT use REPL for |
|---|---|
| Testing one expression | Code you want to keep |
| Checking what a function does | Anything longer than 10 lines |
| Debugging ("what is this value?") | Homework or projects |

> The REPL forgets everything when you close it. If the code matters, put it in a file.

---

## Scripts: Code That Survives

A script is just a text file ending in `.py`. Create `hello.py`:

```python
print("hello, world")
```

Run it:

```bash
python hello.py
```

```text
hello, world
```

That is the whole workflow: edit file → run → see output → repeat.

---

## `print()` — Your First Function

`print()` sends text to the terminal. It takes almost anything:

```python
print("text in quotes")
print(42)
print(2 + 3)
print("answer:", 42)
```

```text
text in quotes
42
5
answer: 42
```

Commas add a space automatically. You will use `print()` in every lesson to see what your code does.

---

## `input()` — Reading From the Keyboard

`input()` pauses the program and waits for the user to type something:

```python
name = input("What is your name? ")
print(f"Hello, {name}!")
```

Run it:

```bash
python hello.py
```

```text
What is your name? ada
Hello, ada!
```

Three facts that trip up every beginner:

1. `input()` **always returns a string**, even if the user types digits.
2. The prompt string (`"What is your name? "`) is optional but you almost always want it.
3. The program **blocks** — nothing after `input()` runs until Enter is pressed.

Convert to a number when you need math (more in lesson 03):

```python
age_text = input("Age? ")
age = int(age_text)
print(f"Next year you will be {age + 1}")
```

```text
Age? 30
Next year you will be 31
```

---

## REPL vs Script — Side by Side

| Question | REPL | Script |
|---|---|---|
| How to start | `python` with no args | `python hello.py` |
| Where does code live | Memory only | A `.py` file |
| Good for | Experiments | Real programs |
| Can you re-run it | No (retype everything) | Yes (run the file again) |

Rule of thumb: explore in the REPL, keep in a script.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `NameError: name 'x' is not defined` | Variable from a closed REPL session | REPL memory is gone — retype it or use a file |
| Script does nothing | Forgot to save, or ran wrong file | Save (Ctrl+S), then `ls` to confirm you are in the right folder |
| `input()` seems frozen | It is waiting for Enter | Type something and press Enter |
| `TypeError` adding `input()` result | `input()` returns `str` | Wrap with `int()` or `float()` first |

---

## Checklist

- [ ] I can start the REPL, type expressions, and exit cleanly
- [ ] I can create `hello.py` and run it with `python hello.py`
- [ ] I can use `print()` with strings, numbers, and multiple arguments
- [ ] I can use `input()` and convert the result with `int()` when needed

---
#### [Previous](01-part.md) | [Next](exercises.md)
