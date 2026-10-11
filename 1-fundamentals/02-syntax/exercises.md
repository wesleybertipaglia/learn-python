# Exercises — Syntax

## 1. Fix the Indentation

This file crashes. Fix it so the output matches:

```python
age = 20
if age >= 18:
print("adult")
print("done")
```

Expected:

```text
adult
done
```

Hint: exactly one line needs 4 spaces in front of it.

## 2. Good Names / Bad Names

Which of these are legal variable names? Try each in the REPL:

```text
user_name   total2   2total   my-var   class   _hidden   MAX_SIZE
```

| Name | Legal? | Why / why not |
|---|---|---|
| `user_name` | Yes | — |
| `2total` | ? | — |
| `my-var` | ? | — |
| `class` | ? | — |

Fill in the table. For every illegal name, write the error Python gives you.

## 3. Comments That Earn Their Keep

Rewrite the comments so each one explains WHY:

```python
# set x to 5
x = 5  # x is 5
print(x)  # print x
```

Suggested scenario: `x` is the number of login attempts allowed. Rewrite accordingly.

## 4. `type()` Detective

Predict, then verify in the REPL:

```python
print(type(42))
print(type(3.0))
print(type("42"))
print(type(True))
print(type(input))  # note: no parentheses!
```

Write one sentence for each explaining the result. The last one surprises most beginners.

## 5. Greeter With Style (Stretch)

Rewrite this ugly greeter following PEP 8 (spacing, one statement per line, good names):

```python
n=input("Name? ");a=input("Age? ");print(f"{n} is {a}!")
```

Requirements: `snake_case` names, spaces around `=`, one statement per line, f-string output identical.

---

## Checklist

- [ ] Fixed file runs and prints exactly two lines
- [ ] I can explain why each illegal name is illegal
- [ ] My comments explain WHY, not WHAT
- [ ] `type()` predictions all verified in the REPL

---
#### [Previous](02-part.md) | [Next](../03-data-types/01-part.md)
