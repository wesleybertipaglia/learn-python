# Conditionals — match, Truthiness, Guard Clauses

`if/elif/else` handles everything. These three tools handle it *better* in specific cases.

## `match`: Clean Multi-Way Branching (3.10+)

When one value has many possible shapes, `match` beats a wall of `elif`:

```python
command = input("Command? ")

match command:
    case "quit":
        print("bye!")
    case "help":
        print("commands: quit, help, status")
    case "status":
        print("all systems go")
    case _:
        print(f"unknown command: {command}")
```

```text
Command? help
commands: quit, help, status
```

| Piece | Role |
|---|---|
| `match <value>:` | The value to test |
| `case "literal":` | Runs if value equals the literal |
| `case _:` | Wildcard — like `else`, catches everything |

`match` also destructures — it can unpack lists and dicts by shape:

```python
point = (0, 5)

match point:
    case (0, 0):
        print("origin")
    case (0, y):
        print(f"on y-axis at {y}")
    case (x, 0):
        print(f"on x-axis at {x}")
    case (x, y):
        print(f"at {x}, {y}")
```

```text
on y-axis at 5
```

> `match` needs Python 3.10+. Check with `python --version` — this course requires 3.12, so you are fine. If you share code with someone on 3.9, stick to `if/elif`.

---

## Truthiness in Conditions: Write Less, Mean More

From lesson 03: empty/zero/`None` is falsy. In conditions that gives you idioms every Python dev expects:

```python
# Good — idiomatic
if not errors:
    print("all clear")

# Bad — noisy, marks you as a beginner
if len(errors) == 0:
    print("all clear")
```

| Instead of | Write |
|---|---|
| `if name == "":` | `if not name:` |
| `if name != "":` | `if name:` |
| `if x is not None and len(x) > 0:` | `if x:` (when None and empty both mean "no data") |
| `if x == None:` | `if x is None:` (always) |

Careful with numbers: `if not balance:` treats `0` like "missing". If `0` is a valid balance (it is!), test explicitly:

```python
balance = 0
if balance is None:
    print("no account")
elif balance == 0:
    print("empty but open")
```

---

## Guard Clauses: Return Early, Nest Less

Deep nesting is hard to read. Guards flip the logic: handle bad cases first, then the main path stays flat:

```python
# Nested — harder to follow
def withdraw(balance, amount):
    if amount > 0:
        if amount <= balance:
            return balance - amount
        else:
            return None
    else:
        return None
```

```python
# Guard clauses — flat and obvious
def withdraw(balance, amount):
    if amount <= 0:
        return None
    if amount > balance:
        return None
    return balance - amount

print(withdraw(100, 30))
print(withdraw(100, 500))
```

```text
70
None
```

Each guard says "this input is invalid, I am done". After the guards, the remaining code deals only with valid data. You will use this pattern in the bank challenge.

---

## `if` vs `match`: Which One?

| Situation | Pick |
|---|---|
| Range checks (`x < 10`), compound logic | `if/elif/else` |
| One value, many literal options | `match` |
| Unpacking tuples/dicts by shape | `match` |
| Python < 3.10 compatibility needed | `if/elif/else` |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `match` on 3.9 | `SyntaxError` | Upgrade to 3.10+ or use `if/elif` |
| Missing `case _:` | Silent no-op on unexpected input | Always add a wildcard that reports or rejects |
| `if not balance:` eats `0` | Zero treated as missing | Use `is None` when `0` is valid |
| Guard returns `None` implicitly | Caller gets `None` surprise | `return None` explicitly so intent is visible |

---

## Checklist

- [ ] I can write a `match` with literal cases and a `case _:` wildcard
- [ ] I can match a tuple pattern like `case (0, y):`
- [ ] I prefer `if not errors:` over `if len(errors) == 0:`
- [ ] I flatten nested `if`s with guard clauses

---
#### [Previous](01-part.md) | [Next](exercises.md)
