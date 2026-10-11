# Challenge: Mini Bank with OOP Pillars

Build a menu-driven bank CLI that uses every OOP pillar: encapsulated accounts, an inherited savings type, polymorphic statement exporters, and JSON persistence.

> You are ready when you can model, persist, and defend your data with classes and clean interfaces.

## Goal

Write `bank.py` that manages accounts, earns interest on savings, exports statements two ways, and survives restarts via a JSON file.

| Requirement | Must use |
|---|---|
| Account model | `class` with `__init__`, methods, `__str__`, `@property` |
| Encapsulation | Validated `balance >= 0`, `ValueError` on bad set |
| Inheritance | `SavingsAccount(Account)` with `rate` + `add_interest()` |
| Polymorphism | `StatementExporter` ABC: `TextStatement` + `JsonStatement` |
| Persistence | `pathlib`, `json.dump` / `json.load` |
| Robustness | `try/except` for `FileNotFoundError`, `ValueError` |

## Specification

Account fields: `owner: str`, `_balance: float` with validated `@property`, plus `deposit` and `withdraw`.

```python
from abc import ABC, abstractmethod

class BankAccount:
    def __init__(self, owner: str, balance: float = 0.0):
        self.owner = owner
        self._balance = 0.0
        self.balance = balance

    @property
    def balance(self) -> float:
        return self._balance

    @balance.setter
    def balance(self, value: float):
        if value < 0:
            raise ValueError("balance cannot be negative")
        self._balance = value

    def deposit(self, amount: float) -> None:
        if amount <= 0:
            raise ValueError("deposit must be positive")
        self._balance += amount

    def withdraw(self, amount: float) -> None:
        if amount <= 0:
            raise ValueError("withdraw must be positive")
        if amount > self._balance:
            raise ValueError("insufficient funds")
        self._balance -= amount

    def __str__(self) -> str:
        return f"{self.owner}: {self._balance:.2f} EUR"
```

```text
ana: 70.00 EUR
```

Inheritance plus polymorphism:

```python
class SavingsAccount(BankAccount):
    def __init__(self, owner: str, balance: float = 0.0, rate: float = 0.02):
        super().__init__(owner, balance)
        self.rate = rate

    def add_interest(self) -> None:
        self._balance += self._balance * self.rate

class StatementExporter(ABC):
    @abstractmethod
    def export(self, owner: str, balance: float) -> str:
        ...

class TextStatement(StatementExporter):
    def export(self, owner: str, balance: float) -> str:
        return f"Statement for {owner}\nBalance: {balance:.2f} EUR"

class JsonStatement(StatementExporter):
    def export(self, owner: str, balance: float) -> str:
        import json
        return json.dumps({"owner": owner, "balance": balance})
```

Persistence file `bank.json` shape:

```python
data = {
    "accounts": {"ana": {"balance": 70.0, "rate": 0.02, "kind": "savings"}},
    "next_id": 1,
}
```

Menu options:

```text
1) create account
2) deposit
3) withdraw
4) show account
5) add interest (savings only)
6) export statement (text/json)
0) save + quit
```

## Starter Skeleton

```python
from pathlib import Path
import json

DB = Path("bank.json")

def load() -> dict:
    try:
        return json.loads(DB.read_text(encoding="utf-8"))
    except FileNotFoundError:
        return {"accounts": {}, "next_id": 1}
    except json.JSONDecodeError:
        print("corrupt bank.json, starting fresh")
        return {"accounts": {}, "next_id": 1}

def save(data: dict) -> None:
    DB.write_text(json.dumps(data, indent=2), encoding="utf-8")

if __name__ == "__main__":
    print("bank loaded. implement the menu loop here.")
```

Run it:

```bash
python bank.py
```

```text
bank loaded. implement the menu loop here.
```

Your job: rebuild `BankAccount`/`SavingsAccount` objects from `load()`, wire each menu choice to methods, route statements through the `StatementExporter` ABC, and call `save()` on quit.

## Example Transcript

```text
=== Mini Bank ===
1) create account
2) deposit
3) withdraw
4) show account
5) add interest (savings only)
6) export statement (text/json)
0) save + quit
> 1
owner: ana
savings? [y/n]: y
created ana: 0.00 EUR
> 2
owner: ana
amount: 100
deposited. ana: 100.00 EUR
> 3
owner: ana
amount: 30
withdrew. ana: 70.00 EUR
> 5
interest added. ana: 71.40 EUR
> 6
format (text/json): text
Statement for ana
Balance: 71.40 EUR
> 0
saved to bank.json. bye!
```

Second run must show the same balance — proof persistence works:

```bash
python bank.py
# choose 4, owner ana -> ana: 71.40 EUR
```

```text
ana: 71.40 EUR
```

## Tips

- Keep all file I/O in `load()` and `save()`. Logic stays testable.
- Validate at the boundary: parse `float(input())` inside `try/except ValueError`.
- Store the account kind (`"plain"` vs `"savings"`) so reload rebuilds the right class.
- Never store `_balance` logic in the menu. Call methods.
- Test unhappy paths first: overdraft, negative deposit, unknown owner.

| Tip | Why |
|---|---|
| One `Path` constant `DB` | Single place to change file |
| `encoding="utf-8"` everywhere | Portable saves |
| `indent=2` JSON | Human-debuggable file |
| Exporters take `(owner, balance)` | Uniform polymorphic interface |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Forgetting `save()` on quit | Data lost on restart | Save on option 0 and after each mutation |
| Bypassing property on load | Negative balance accepted | Rebuild via constructor, not `_balance` |
| `float(input())` unguarded | Crash on `abc` | `try/except ValueError` with message |
| Interest on plain account | `AttributeError: add_interest` | Check `isinstance(acc, SavingsAccount)` |
| Exporter `isinstance` chain | New format edits menu | Call `exporter.export(...)` polymorphically |

## Checklist

- [ ] `BankAccount` validates deposits, withdrawals, balance
- [ ] `SavingsAccount` inherits and adds interest via `super().__init__`
- [ ] `TextStatement` and `JsonStatement` share the ABC interface
- [ ] `bank.json` round-trips accounts and kind flags
- [ ] Missing/corrupt file starts fresh without traceback
- [ ] Menu loop handles all 7 options + invalid input
- [ ] Code runs with `python bank.py` on Python 3.12+

## Grading Checklist

| Points | Criterion |
|---|---|
| 20 | Classes correct: `__init__`, `self`, `__str__`, `@property` validation |
| 20 | Inheritance + ABC polymorphism used correctly |
| 20 | JSON persistence with `pathlib`, survives restart |
| 15 | Exceptions handled: `FileNotFoundError`, `JSONDecodeError`, `ValueError` |
| 15 | CLI menu complete with transcript-matching behavior |
| 10 | Code style: `__main__` guard, UTF-8, no dead code |

Score 80+ to advance. 90+ with extras.

## Extras

- Add a `Library` catalog variant: `Item` ABC with `Book` and `DVD` subclasses.
- Add CSV export via a third `CsvStatement` exporter with no menu changes.
- Add an `iter_accounts()` generator yielding one account at a time.
- Write a `@dataclass(frozen=True)` transaction log alongside balances.

#### [Previous](./06-abstraction/exercises.md) | [Next](../4-advanced/01-iterators-generators/01-part.md)
