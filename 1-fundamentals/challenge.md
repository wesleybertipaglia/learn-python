# Challenge — CLI Bank Simulator

Everything in this module converges here: variables, conditionals, loops, functions, exceptions. Build a small bank that never crashes on bad input.

## The Spec

Menu-driven program. Accounts stored as a **list of dicts**: `{"name": str, "balance": float}`. Six options:

```text
=== Python Bank ===
1. Create account
2. Deposit
3. Withdraw
4. List accounts
5. Show total funds
6. Quit
Choice? 
```

| Option | Behavior |
|---|---|
| 1. Create | Ask name + opening balance; reject duplicates and negative balances |
| 2. Deposit | Ask name + amount; amount must be > 0; unknown name → message |
| 3. Withdraw | Ask name + amount; must be > 0 and ≤ balance; otherwise message |
| 4. List | Print every account `name: $balance` with 2 decimals, or `No accounts yet.` |
| 5. Total | Print `Total funds: $X.XX` across all accounts |
| 6. Quit | Print `Goodbye!` and exit |

> The program must NEVER crash on bad input. Empty names, letters where numbers belong, unknown accounts, bad menu choices — every one prints a message and re-shows the menu.

---

## Example Transcript

```text
=== Python Bank ===
1. Create account
2. Deposit
3. Withdraw
4. List accounts
5. Show total funds
6. Quit
Choice? 1
Name? ada
Opening balance? 100
Account 'ada' created with $100.00.

=== Python Bank ===
1. Create account
2. Deposit
3. Withdraw
4. List accounts
5. Show total funds
6. Quit
Choice? 2
Name? ada
Amount? abc
'abc' is not a number — try again.
Amount? 50
Deposited $50.00. New balance: $150.00.

=== Python Bank ===
1. Create account
2. Deposit
3. Withdraw
4. List accounts
5. Show total funds
6. Quit
Choice? 3
Name? ada
Amount? 500
Insufficient funds: balance is $150.00.
Choice? 9
Please choose 1-6.
Choice? 6
Goodbye!
```

Study the transcript: every mistake gets a sentence, then the menu returns. That loop is the whole challenge.

---

## Architecture: One Function Per Option

```python
def find_account(accounts, name):
    """Return the account dict, or None if missing."""
    for acc in accounts:
        if acc["name"] == name:
            return acc
    return None


def create_account(accounts):
    """Ask for name + balance, append a new dict."""
    ...


def deposit(accounts):
    """Ask for name + amount, add to balance."""
    ...


def withdraw(accounts):
    """Ask for name + amount, subtract if valid."""
    ...


def list_accounts(accounts):
    """Print all accounts or 'No accounts yet.'."""
    ...


def show_total(accounts):
    """Print the sum of all balances."""
    ...


def read_float(prompt):
    """Keep asking until the user types a valid number."""
    while True:
        text = input(prompt)
        try:
            return float(text)
        except ValueError:
            print(f"'{text}' is not a number — try again.")


def main():
    """Menu loop: show, read choice, dispatch, repeat."""
    accounts = []
    while True:
        print("=== Python Bank ===")
        print("1. Create account")
        print("2. Deposit")
        print("3. Withdraw")
        print("4. List accounts")
        print("5. Show total funds")
        print("6. Quit")
        choice = input("Choice? ").strip()
        match choice:
            case "1":
                create_account(accounts)
            case "2":
                deposit(accounts)
            case "3":
                withdraw(accounts)
            case "4":
                list_accounts(accounts)
            case "5":
                show_total(accounts)
            case "6":
                print("Goodbye!")
                break
            case _:
                print("Please choose 1-6.")


if __name__ == "__main__":
    main()
```

Fill in each `...` body. Suggested logic per function:

| Function | Steps |
|---|---|
| `create_account` | Read name (reject empty), reject duplicate via `find_account`, `read_float` balance (reject negative), append dict |
| `deposit` | Read name, `find_account` (message if missing), `read_float` (must be > 0), add + confirm |
| `withdraw` | Same as deposit, plus `amount > balance` → insufficient message |
| `list_accounts` | `if not accounts:` → message; else `f"{a['name']}: ${a['balance']:.2f}"` each |
| `show_total` | `sum(a["balance"] for a in accounts)` with `:.2f` |

---

## Tips

- **Validate at the boundary.** All `input()` parsing goes through `read_float` + explicit checks. Logic functions never call `float()` on raw text.
- **Compare menu choice as a string.** `input()` returns `"1"`, not `1`. Matching `case "1":` avoids an `int()` conversion that would crash on `abc`.
- **`.strip()` the choice and name.** Users add trailing spaces; `"ada "` should find `"ada"`.
- **Guard clauses per function.** Unknown account → message + `return`. Bad amount → message + `return`. Happy path stays flat.
- **Format money once, everywhere:** `f"${value:.2f}"`. No bare floats in output.
- **Test the nasty path first:** empty name, `-50`, `abc`, unknown user, withdraw-more-than-balance, choice `9`. If those six all print messages instead of tracebacks, you are nearly done.

---

## Grading Checklist

- [ ] Menu shows all 6 options and loops until `6`
- [ ] Accounts are a list of `{"name", "balance"}` dicts; one function per option
- [ ] Duplicate names rejected; empty names rejected
- [ ] Negative/zero deposit and withdraw rejected with a message
- [ ] Overdraft rejected; balance never goes negative
- [ ] List and total format with `$x.xx` (2 decimals); empty state prints `No accounts yet.`
- [ ] No crash on: `abc` for numbers, unknown names, empty input, choice `9`/`0`/`abc`
- [ ] `read_float` uses `try/except ValueError` (no bare `except`); `ruff check` is clean

---

## Extra Challenges

1. **Transfer.** Add option 7: move money between two accounts. Reuse `find_account` twice; validate both names and the amount.
2. **Persist to file.** Save accounts to `bank.json` on quit, load on start (`json` module: `json.dump` / `json.load`, `FileNotFoundError` → start empty).
3. **Transaction log.** Keep a list of strings (`"ada +$50.00 deposit"`) and add menu option 8 to print it. Cap it at the last 20 entries.
4. **Interest.** On total display, also project +2% annual interest per account. Pure computation function, easy to unit-test.

---
#### [Previous](09-debugging/exercises.md) | [Next](../2-data-structures/01-lists/01-part.md)
