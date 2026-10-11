# Functions — def, Params, return, Docstrings

Functions turn a script into a toolkit: name a job, give it inputs, get back an output.

## `def`: Name a Block of Code

```python
def greet(name):
    print(f"Hello, {name}!")

greet("ada")
greet("grace")
```

```text
Hello, ada!
Hello, grace!
```

| Part | Meaning |
|---|---|
| `def greet(name):` | Define a function called `greet` taking one parameter `name` |
| Body (indented) | Runs each time the function is called |
| `greet("ada")` | Call: `name` is bound to `"ada"` for that run |

Why functions? The same reason as loops: write once, reuse safely. Plus names — `withdraw(balance, amount)` documents intent in a way inline code never does.

---

## Parameters vs Arguments

- **Parameter**: the name in the definition (`name`).
- **Argument**: the value at the call site (`"ada"`).

```python
def add(a, b):
    return a + b

print(add(2, 3))       # positional: a=2, b=3
print(add(b=10, a=1))  # keyword: order does not matter
```

```text
5
11
```

Keyword arguments shine when a function has several parameters — the call reads like a label instead of a puzzle.

---

## `return`: Send a Value Back

```python
def square(n):
    return n * n

result = square(5)
print(result)
```

```text
25
```

`return` does two things: produces the value **and exits the function immediately**. Nothing after `return` in that path runs:

```python
def demo():
    return 1
    print("never runs")

print(demo())
```

```text
1
```

> `print()` displays. `return` delivers. Beginners print inside functions and then wonder why the caller gets `None` — if the caller needs the value, you must `return` it.

---

## `None` Return: Doing vs Computing

A function with no `return` returns `None`:

```python
def shout(text):
    print(text.upper())

result = shout("hi")
print(result)
```

```text
HI
None
```

Two kinds of functions:

| Kind | Returns | Example |
|---|---|---|
| Effect (does something) | `None` | `print_menu()`, `save()` |
| Computation (answers something) | A value | `square(5)` → `25` |

Do not mix them casually — a function that both prints and returns is harder to test and reuse.

---

## Docstrings: The One-Line Manual

The first string inside `def` documents the function. `help()` shows it:

```python
def withdraw(balance, amount):
    """Return balance minus amount, or None if invalid."""
    if amount <= 0 or amount > balance:
        return None
    return balance - amount

help(withdraw)
```

```text
Help on function withdraw in module __main__:

withdraw(balance, amount)
    Return balance minus amount, or None if invalid.
```

Write docstrings as **what + contract**: what it returns and what counts as invalid. One line is enough at this level.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Forgetting `return` | Caller gets `None` | `return` the computed value |
| `print` instead of `return` | Value visible but unusable | `return` it; let the caller print |
| Calling before defining | `NameError` | Define functions at top, call at bottom |
| Positional mix-up `add(10, 1)` vs `add(1, 10)` | Wrong result, no error | Use keywords for 3+ params |

---

## Checklist

- [ ] I can define and call a function with params and `return`
- [ ] I know parameter (definition) vs argument (call site)
- [ ] My effect functions return `None`, my compute functions return values
- [ ] Every function I write this week has a one-line docstring

---
#### [Previous](../06-loops/exercises.md) | [Next](02-part.md)
