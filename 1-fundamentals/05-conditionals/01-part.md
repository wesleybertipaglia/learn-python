# Conditionals — if / elif / else

Every program that makes decisions uses this one shape. Learn it cold.

## The Basic Shape

```python
age = int(input("Age? "))

if age >= 18:
    print("adult")
elif age >= 13:
    print("teenager")
else:
    print("child")
```

```text
Age? 15
teenager
```

| Keyword | Runs when |
|---|---|
| `if` | Its condition is truthy (checked first, required) |
| `elif` | Previous conditions failed and this one holds (zero or more) |
| `else` | Everything above failed (at most one, no condition) |

Python evaluates top-down and runs **only the first matching branch**. Even if two conditions are true, only the first prints:

```python
x = 20
if x > 10:
    print("big")
elif x > 15:
    print("very big")  # never runs — first branch already won
```

```text
big
```

> Order matters: put the most specific condition first. `if x > 10` before `if x > 15` shadows the second branch forever.

---

## Indentation Is the Logic

There are no braces. The indent decides what belongs to the branch:

```python
age = 20
if age >= 18:
    print("adult")
    print("can vote")
print("done")  # always runs — column 0
```

```text
adult
can vote
done
```

Dedent one line too early and it escapes the branch. Indent one line too far and you get `IndentationError`. When a branch misbehaves, check indentation before logic.

---

## Conditions Are Just Truthiness

The condition can be any expression — Python calls `bool()` on it:

```python
name = input("Name? ")
if name:
    print(f"Hello, {name}")
else:
    print("You typed nothing")
```

```python
if 1 < 3 < 10:
    print("in range")
```

```text
in range
```

Common condition patterns:

| Pattern | Meaning |
|---|---|
| `if x == 5:` | Equality |
| `if x is None:` | Missing value |
| `if name:` | Non-empty string |
| `if errors:` | Non-empty list = has errors |
| `if 1 <= x <= 9:` | Chained range check |

---

## The Ternary: One-Line `if`

For simple either/or assignments:

```python
age = 20
status = "adult" if age >= 18 else "minor"
print(status)
```

```text
adult
```

Syntax: `value_if_true if condition else value_if_false`. It is an **expression** (produces a value), not a statement.

| Use ternary | Use a block |
|---|---|
| Assigning one of two values | Anything with side effects |
| Short, readable on one line | Nested conditions |
| `label = "even" if n % 2 == 0 else "odd"` | More than two branches |

> If your ternary needs a second `else`, it is no longer simple — rewrite it as `if/elif/else`. Nested ternaries are a readability crime.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `=` instead of `==` | `SyntaxError: invalid syntax` | `=` assigns, `==` compares |
| Shadowed branch | `elif` never runs | Put narrower conditions first |
| Over-indented `else` | `SyntaxError` or wrong branch | `else` aligns with its `if` |
| Nested ternary soup | Nobody (including you) can read it | Expand to `if/elif/else` |

---

## Checklist

- [ ] I can write `if/elif/else` with correct colons and 4-space indents
- [ ] I know only the first matching branch runs
- [ ] I can use truthiness (`if name:`) instead of `== ""`
- [ ] I use ternaries only for simple two-way assignments

---
#### [Previous](../04-operators/exercises.md) | [Next](02-part.md)
