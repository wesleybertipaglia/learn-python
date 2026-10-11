# Exercises — Conditionals

## 1. Grade Assigner

Ask for a score 0–100, print the grade:

| Score | Grade |
|---|---|
| 90+ | A |
| 80–89 | B |
| 70–79 | C |
| 60–69 | D |
| below 60 | F |

```text
Score? 85
B
```

Watch the ordering — check `>= 90` first.

## 2. Ternary Rewrite

Rewrite with a ternary (one line of assignment):

```python
age = int(input("Age? "))
if age >= 18:
    status = "adult"
else:
    status = "minor"
print(status)
```

Then answer: when would you NOT use a ternary for this?

## 3. `match` Command Parser

Write a program using `match` with cases `"quit"`, `"help"`, `"status"`, and a `case _:` wildcard:

```text
Command? status
all systems go
```

```text
Command? dance
unknown command: dance
```

Remove the wildcard and try an unknown command. What happens? (Nothing — that silence is the bug the wildcard prevents.)

## 4. Guard Clause Refactor

Flatten this nesting with guard clauses:

```python
def withdraw(balance, amount):
    if amount > 0:
        if amount <= balance:
            return balance - amount
        else:
            return None
    else:
        return None
```

Verify: `withdraw(100, 30)` → `70`, `withdraw(100, 500)` → `None`, `withdraw(100, -5)` → `None`.

## 5. Zero vs Missing (Stretch)

`balance` may be `None` (no account), `0` (empty), or positive. Print three different messages. Why does `if not balance:` fail here? Write the correct version using `is None`.

---

## Checklist

- [ ] Grader handles boundary scores (90, 80, 60) correctly
- [ ] Ternary version produces identical output
- [ ] `match` program has a wildcard and I know what breaks without it
- [ ] Guard version passes all three `withdraw` tests

---
#### [Previous](02-part.md) | [Next](../06-loops/01-part.md)
