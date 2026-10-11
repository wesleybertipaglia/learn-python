# Exercises — Operators

## 1. Predict the Output

No REPL yet — write your answer first, then verify:

```python
print(2 + 3 * 4)
print((2 + 3) * 4)
print(10 / 4)
print(10 // 4)
print(10 % 3)
print(2 ** 3 ** 2)
print(True or True and False)
```

| Expression | Your answer | Actual |
|---|---|---|
| `2 + 3 * 4` | | |
| `2 ** 3 ** 2` | | |
| `True or True and False` | | |

## 2. Even or Odd

Ask for an integer, print whether it is even or odd:

```text
Number? 7
7 is odd
```

| Input | Expected |
|---|---|
| `42` | `42 is even` |
| `7` | `7 is odd` |

Hint: `n % 2 == 0`.

## 3. `is` vs `==` Lab

Run and explain every line:

```python
a = [1, 2]
b = [1, 2]
print(a == b)
print(a is b)
x = None
print(x is None)
```

Question: why is `a == b` True while `a is b` is False? One sentence.

## 4. The Classic Trap

Explain why this program prints `Access granted` even for user `bob`:

```python
user = "bob"
if user == "admin" or "root":
    print("Access granted")
```

Fix it two ways: with a repeated comparison, and with `in`.

## 5. Default Name (Stretch)

Write a 3-line program: ask for a name, fall back to `"anonymous"` when the user presses Enter with no text, greet them. Hint: `or`.

```text
Name?
Hello, anonymous!
```

---

## Checklist

- [ ] Precedence predictions verified (especially `**` associativity)
- [ ] Even/odd works for negative numbers too
- [ ] I can explain `==` vs `is` without hand-waving
- [ ] Trap fixed both ways and I know why the original is truthy

---
#### [Previous](02-part.md) | [Next](../05-conditionals/01-part.md)
