# Syntax — Indentation, Comments, Variables

Python uses indentation instead of braces. That one decision shapes everything you write.

## Indentation: 4 Spaces, Always

Blocks of code are defined by indentation. Same level = same block:

```python
age = 20

if age >= 18:
    print("adult")
    print("you can vote")
print("done")
```

```text
adult
you can vote
done
```

The first unindented line (`print("done")`) is outside the `if`. That is how Python knows — no `{ }`, no `end` keyword.

> Mixing tabs and spaces is the fastest way to get a TabError. Configure your editor to insert 4 spaces when you press Tab. VS Code does this by default for Python.

```python
# This crashes — do not mix tabs and spaces
if True:
    print("spaces")
	print("tab")  # TabError
```

---

## Comments With `#`

Anything after `#` is ignored:

```python
# Ask the user for their name
name = input("Name? ")  # trailing comment: pause and read input
print(name)
```

Use comments to explain **why**, not **what**:

| Bad comment | Good comment |
|---|---|
| `x = x + 1  # add 1 to x` | `x = x + 1  # retry counter: give up after 3` |
| `print(d)  # print d` | `# debug: show parsed date before validation` |

---

## Variables: Names for Values

A variable is a name bound to a value. No declaration, no type annotation needed:

```python
name = "ada"
age = 36
height = 1.65
print(name, age, height)
```

```text
ada 36 1.65
```

Why no declaration? Python is **dynamically typed**: the type lives on the value, not the variable. You can rebind a name to a different type (legal, but usually confusing — do not do it on purpose).

```python
x = 10
print(type(x))
x = "now a string"
print(type(x))
```

```text
<class 'int'>
<class 'str'>
```

---

## Naming: `snake_case` Everywhere

| Rule | Example |
|---|---|
| Variables and functions: lowercase + underscores | `user_name`, `total_price` |
| Constants: ALL CAPS | `MAX_RETRIES = 3` |
| Start with letter or underscore, then letters/digits/underscores | `_private`, `score2` |
| Never start with a digit, never use spaces or dashes | `2fast` is illegal |

```python
user_name = "grace"
total_price = 19.99
MAX_RETRIES = 3
print(user_name, total_price, MAX_RETRIES)
```

Reserved words you cannot use as names: `if`, `else`, `for`, `while`, `def`, `return`, `import`, `None`, `True`, `False`, and about 25 more. Your editor highlights them — if your variable name turns purple/blue, pick another name.

---

## `print()` and `input()` Recap + f-strings

You met these in lesson 01. Now the pattern you will use daily:

```python
name = input("Name? ")
age = input("Age? ")
print(f"{name} is {age} years old")
```

```text
Name? ada
Age? 36
ada is 36 years old
```

An f-string starts with `f` and lets you drop `{expressions}` straight into text. It is the modern way to format output — forget `%` formatting and `.format()` for now.

```python
price = 19.99
qty = 3
print(f"Total: {price * qty}")
```

```text
Total: 59.97
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `IndentationError: unexpected indent` | Extra spaces before a line | Dedent to match the block |
| `TabError: inconsistent use of tabs` | Mixed tabs and spaces | Convert all indents to 4 spaces |
| `SyntaxError: invalid syntax` on a name | Used a keyword (`class = 5`) | Rename the variable |
| `NameError` on a typo | `user_nmae` vs `user_name` | Copy-paste names instead of retyping |

---

## Checklist

- [ ] I indent blocks with exactly 4 spaces, never tabs
- [ ] I can explain why `print("done")` at column 0 is outside the `if`
- [ ] My variables are `snake_case` and avoid keywords
- [ ] I can write an f-string with at least one `{expression}`

---
#### [Previous](../01-getting-started/exercises.md) | [Next](02-part.md)
