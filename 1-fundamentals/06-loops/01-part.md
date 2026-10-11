# Loops — for, range, while, break/continue

If you are copying a line more than twice, you need a loop.

## `for` + `range()`: Repeat N Times

```python
for i in range(5):
    print(i)
```

```text
0
1
2
3
4
```

`range()` never includes its stop value. That trips everyone once, then becomes second nature:

| Call | Produces |
|---|---|
| `range(5)` | `0 1 2 3 4` |
| `range(1, 6)` | `1 2 3 4 5` |
| `range(0, 10, 2)` | `0 2 4 6 8` |
| `range(5, 0, -1)` | `5 4 3 2 1` |

```python
for n in range(1, 6):
    print(f"2 x {n} = {2 * n}")
```

```text
2 x 1 = 2
2 x 2 = 4
2 x 3 = 6
2 x 4 = 8
2 x 5 = 10
```

Why `for` instead of copy-paste? One place to fix, zero chance of editing 4 copies and missing the 5th.

---

## `while`: Repeat Until a Condition Changes

```python
password = ""
while password != "secret":
    password = input("Password? ")
print("welcome!")
```

```text
Password? no
Password? secret
welcome!
```

Use `while` when you **do not know how many iterations** you need (user input, retry loops). Use `for` when you do (5 rows, every item in a list).

> Every `while` needs something inside that moves toward the exit. If nothing changes the condition, you have an infinite loop — press Ctrl+C to escape it.

```python
# INFINITE — count never changes. Do not run this.
count = 0
while count < 5:
    print(count)
```

```python
# Fixed — count moves toward 5
count = 0
while count < 5:
    print(count)
    count += 1
```

```text
0
1
2
3
4
```

---

## `break`: Leave the Loop Now

```python
while True:
    text = input("Type 'quit' to exit: ")
    if text == "quit":
        break
    print(f"you typed: {text}")
print("bye!")
```

```text
Type 'quit' to exit: hello
you typed: hello
Type 'quit' to exit: quit
bye!
```

`while True` + `break` is the standard menu/input loop — you will use it in the bank challenge. The `break` is the exit; without it the loop truly never ends.

---

## `continue`: Skip to the Next Iteration

```python
for n in range(1, 6):
    if n == 3:
        continue
    print(n)
```

```text
1
2
4
5
```

`break` quits the whole loop. `continue` quits only this lap:

| Statement | Effect |
|---|---|
| `break` | Exit the loop entirely |
| `continue` | Skip rest of body, start next iteration |

```python
# Skip negatives, stop at first zero
for n in [5, -1, 9, 0, 7]:
    if n < 0:
        continue
    if n == 0:
        break
    print(n)
```

```text
5
9
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Infinite `while` | Program hangs, no prompt | Ctrl+C, then make the condition change inside |
| Off-by-one with `range` | `range(5)` expected `1-5` | Got `0-4`; use `range(1, 6)` |
| `break` in nested loop | Only inner loop exits | Restructure or use a function + `return` |
| Forgetting `+= 1` in `while` | Loop never ends | Prefer `for range()` when count is known |

---

## Checklist

- [ ] I can write `range()` with 1, 2, and 3 arguments correctly
- [ ] I choose `for` for known counts, `while` for unknown counts
- [ ] I can write a `while True` + `break` input loop from memory
- [ ] I know `break` exits the loop and `continue` skips one lap

---
#### [Previous](../05-conditionals/exercises.md) | [Next](02-part.md)
