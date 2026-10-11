# Exercises — Loops

## 1. Countdown

Print `5 4 3 2 1` then `liftoff!` using `range()` with a negative step. One loop, no hard-coded prints.

```text
5
4
3
2
1
liftoff!
```

## 2. Input Loop With `break`

Keep asking for a password until the user types `secret`, then print `welcome!`:

```text
Password? no
Password? secret
welcome!
```

Requirements: `while True` + `break`. No fixed retry count.

## 3. Skip and Stop

Given `nums = [5, -1, 9, 0, 7]`: skip negatives with `continue`, stop at the first `0` with `break`, print the rest. Expected output: `5` then `9`.

| Input value | Action |
|---|---|
| negative | skip |
| `0` | stop entirely |
| positive | print |

## 4. `enumerate` + `zip` Menu

Given these lists, print a numbered menu (1-based) with prices:

```python
items = ["coffee", "tea", "juice"]
prices = [2.50, 1.80, 3.00]
```

Expected:

```text
1. coffee: $2.50
2. tea: $1.80
3. juice: $3.00
```

Hint: `zip(items, prices)` plus `enumerate(..., start=1)`.

## 5. Multiplication Table (Stretch)

Print a 3×3 table with nested loops (see `02-part.md` example). Then answer: how many times does the inner body run for `range(1, 10)` × `range(1, 10)`? Verify by counting with a counter variable.

---

## Checklist

- [ ] Countdown uses a single `range(5, 0, -1)` loop
- [ ] Password loop exits only on `secret`
- [ ] Skip/stop prints exactly `5` and `9`
- [ ] Menu shows 1-based numbers with `$x.xx` prices

---
#### [Previous](02-part.md) | [Next](../07-functions/01-part.md)
