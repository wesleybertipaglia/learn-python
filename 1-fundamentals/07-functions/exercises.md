# Exercises — Functions

## 1. Square vs Print

Write both, then explain the difference:

```python
def square(n):
    return n * n

def shout(text):
    print(text.upper())
```

Questions:
- What does `print(square(5))` show?
- What does `x = shout("hi")` store in `x`? Verify with `print(x)`.

## 2. Docstring + `help()`

Add a one-line docstring to:

```python
def withdraw(balance, amount):
    if amount <= 0 or amount > balance:
        return None
    return balance - amount
```

Then run `help(withdraw)` in the REPL. It should show your line.

## 3. Default Greeting

Write `greet(name, greeting="Hello")` so that:

| Call | Output |
|---|---|
| `greet("ada")` | `Hello, ada!` |
| `greet("grace", "Hi")` | `Hi, grace!` |

## 4. Mutable Default Demo

Run the broken version, observe the leak, then fix it:

```python
def add_item(item, cart=[]):
    cart.append(item)
    return cart

print(add_item("apple"))
print(add_item("pear"))
```

Requirements: after the fix, the second call prints `['pear']` only. One-sentence explanation of WHY the original leaks.

## 5. `*args` Sum (Stretch)

Write `total(*numbers)` returning `sum(numbers)`. Verify `total(1, 2, 3)` → `6` and `total()` → `0`. Then write `profile(**info)` that prints each `key: value` pair.

---

## Checklist

- [ ] I can show `return` vs `print` with `square`/`shout`
- [ ] `help()` displays my docstring
- [ ] Defaults and `*args` versions pass the table tests
- [ ] Mutable-default fix verified and explained

---
#### [Previous](02-part.md) | [Next](../08-exceptions/01-part.md)
