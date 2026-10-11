# Syntax — Statements, Blocks, Scope, PEP 8

Part 1 gave you the atoms. This part gives you the rules for combining them.

## Statements vs Blocks

A **statement** is one instruction. A **block** is a group of statements that run together under a header ending with `:`.

```python
# Three statements, no blocks
name = "ada"
print(name)
print(len(name))
```

```text
ada
3
```

```python
# One header + one block of two statements
age = 20
if age >= 18:
    print("adult")
    print("can vote")
```

```text
adult
can vote
```

The colon + indent is the entire block syntax. Every compound statement (`if`, `for`, `while`, `def`, `try`, `match`) works this way — learn it once, reuse it everywhere.

> If a line ends with `:`, the next line MUST be indented. If it is not, Python raises `IndentationError: expected an indented block`.

---

## Scope Intro: Where Names Are Visible

A name defined inside a block or function is not always visible outside it. The basic rule for now:

```python
if True:
    message = "inside"
print(message)  # works! if-blocks do NOT create scope
```

```text
inside
```

```python
def greet():
    greeting = "hello"
    print(greeting)

greet()
print(greeting)  # NameError — functions DO create scope
```

```text
hello
Traceback (most recent call last):
  ...
NameError: name 'greeting' is not defined
```

| Construct | Creates new scope? |
|---|---|
| `if` / `for` / `while` blocks | No |
| `def` (function body) | Yes |
| Module (top level of file) | Yes — the global scope |

Full scope rules (LEGB) come in lesson 07. For now: if a variable is born inside a function, it dies there.

---

## PEP 8 Basics: Code Others Can Read

PEP 8 is Python's style guide. You do not need all of it — just these five rules:

| Rule | Example |
|---|---|
| 4 spaces per indent | `    print(x)` |
| Spaces around `=` and operators | `total = price + tax` not `total=price+tax` |
| One statement per line | Never `x = 1; y = 2` |
| Blank line between logical sections | Separate input / logic / output |
| Max ~88–100 chars per line | Break long lines with parentheses |

```python
# Hard to read
name=input("Name? ");print(f"Hello,{name}!")

# Easy to read
name = input("Name? ")
print(f"Hello, {name}!")
```

Style is not vanity — in a course with hundreds of learners, consistent style is what makes your bug reports answerable.

---

## `type()` — Ask What Something Is

```python
print(type(42))
print(type(3.14))
print(type("hello"))
print(type(True))
```

```text
<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
```

Use `type()` when an error confuses you. Half of all `TypeError`s are "I thought this was an `int` but it is a `str`" (usually from `input()`).

---

## `help()` — Built-in Documentation

```python
help(len)
```

```text
Help on built-in function len in module builtins:

len(obj, /)
    Return the number of items in a container.
```

In the REPL, `help(str)` shows every string method. In a script it dumps to stdout — useful, but the REPL is the nicer place to browse. `help()` + `type()` together answer "what is this and what can it do?" without opening a browser.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `IndentationError: expected an indented block` | Colon with nothing indented after it | Indent the next line 4 spaces |
| `NameError` after a function call | Using a function-local name outside | Return the value or move the variable out |
| Style chaos (`x=1+2`) | Code works but is unreadable | Run `ruff check` (lesson 09) or fix spacing by hand |
| `help()` floods your script output | Called `help()` in a `.py` file | Move exploration to the REPL |

---

## Checklist

- [ ] I can spot the header (`:`) and block (indented lines) in any `if`/`for`/`def`
- [ ] I know `if`-blocks leak names but `def`-blocks do not
- [ ] My code has spaces around operators and one statement per line
- [ ] I can use `type()` and `help()` to investigate an unknown value

---
#### [Previous](01-part.md) | [Next](exercises.md)
