# Invariants and Validated Methods

A property guards one field. An invariant guards the whole object: a promise that is true after every public operation.

> Validate at the boundary, enforce in the object. The object must never be wrong, even if the caller is.

## Invariants: Promises That Always Hold

For a bank account the invariant is simple: `balance >= 0` at all times.

```python
class BankAccount:
    def __init__(self, owner: str, balance: float = 0.0):
        self.owner = owner
        self._balance = 0.0
        self.balance = balance  # setter enforces invariant
        self._check_invariant()

    @property
    def balance(self) -> float:
        return self._balance

    @balance.setter
    def balance(self, value: float):
        if value < 0:
            raise ValueError("balance cannot be negative")
        self._balance = value

    def _check_invariant(self) -> None:
        assert self._balance >= 0, "invariant broken: negative balance"

acc = BankAccount("Ana", 100.0)
print(acc.balance)
acc._check_invariant()
print("invariant holds")
```

```text
100.0
invariant holds
```

| Technique | Where | Purpose |
|---|---|---|
| Setter validation | Every write path | Reject bad values with `ValueError` |
| `_check_invariant()` | End of mutators (debug) | Catch logic slips early |
| Private storage `_balance` | Internals | One controlled entry point |

## Methods That Defend State

Actions are methods. They validate and mutate state through the same guard.

```python
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

acc = BankAccount("Ana", 100.0)
acc.deposit(50.0)
acc.withdraw(30.0)
print(acc)
```

```text
Ana: 120.00 EUR
```

Test the failure path too:

```bash
python -c "print('try acc.withdraw(999) and read the ValueError')"
```

```text
ValueError: insufficient funds
```

Every mutation funnels through one rule set. Add logging or auditing later in exactly one place.

## `ValueError` on Bad Set

Raise `ValueError` when the value type is right but the content is wrong.

```python
acc = BankAccount("Bo", 50.0)
for bad in (-10.0, 0.0):
    try:
        if bad < 0:
            acc.balance = bad
        else:
            acc.withdraw(bad)
    except ValueError as e:
        print(f"rejected {bad}: {e}")
print(acc)
```

```text
rejected -10.0: balance cannot be negative
rejected 0.0: withdraw must be positive
Bo: 50.00 EUR
```

| Raise | When | Example |
|---|---|---|
| `ValueError` | Right type, bad content | Negative balance, zero deposit |
| `TypeError` | Wrong type entirely | `deposit("fifty")` |
| `assert` | Internal logic bug (dev only) | Invariant check in tests |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Validation only in one method | `balance = -5` slips past `deposit` guard | Centralize in property setter |
| Writing `self._balance` directly in methods | Invariant bypassed | Route writes through setter or helper |
| Raising generic `Exception` | Callers cannot catch precisely | Raise `ValueError` with clear message |
| Catching and silencing `ValueError` | Bad state looks fine | Let it propagate or log then re-raise |
| Forgetting unhappy-path tests | Bugs reach production | Test overdraft, negative, zero amounts |

## Checklist

- [ ] I can state my class invariant in one sentence
- [ ] Every write path enforces `balance >= 0`
- [ ] `deposit` and `withdraw` reject zero and negatives
- [ ] Overdraft raises `ValueError: insufficient funds`
- [ ] `__str__` shows a clean one-line summary

#### [Previous](./01-part.md) | [Next](./exercises.md)
