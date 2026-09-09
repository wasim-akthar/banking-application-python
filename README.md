# Python Console Banking Application (CLI)

A robust, object-oriented command-line banking application built with standard Python. This project translates core computer science and Python fundamentals—including inheritance, encapsulation, polymorphism, custom exception hierarchies, and file persistence—into a functional financial system.

---

## Features

* **User Authentication & Profiles**
* Registration with unique usernames and validated passwords.
* Session-based state management (`login` / `logout`).
* Support for multiple bank accounts under a single user profile.


* **Account Lifecycle Management**
* Support for multiple account types:
* **Savings Account**: Enforces a minimum balance rule ($100) and supports interest calculation.
* **Current Account**: Allows pre-configured overdraft protection limits (e.g., up to -$500).


* Auto-generation of unique 10-digit account numbers.


* **Safe Account Deletion & Liquidation**
* Prevents immediate deletion if funds remain (`balance > 0`); prompts the user to liquidate (withdraw to zero) or transfer to another linked account.
* Hard lock on closing accounts with an unpaid overdraft balance (`balance < 0`).
* Immediate clean deletion when `balance == 0`.


* **Financial Transactions**
* Deposits and withdrawals with input validation.
* Atomic inter-account and third-party fund transfers (all-or-nothing execution).
* Comprehensive transaction logging with timestamps, transaction IDs, and running balances.


* **Persistence**
* Automated serialization and deserialization via JSON files (`users.json`, `accounts.json`).
* Graceful initialization on clean installs without existing files.



---

## Project Structure

```
banking_system/
│
├── data/
│   ├── users.json             # Serialized user credentials & profile metadata
│   └── accounts.json          # Serialized account states & balance records
│
├── core/
│   ├── __init__.py
│   ├── exceptions.py          # Custom domain exceptions (e.g., InsufficientFundsError)
│   ├── user.py                # User entity definition
│   ├── accounts.py            # Account (Base), SavingsAccount, CurrentAccount
│   └── bank.py                # Bank aggregate managing global lookups
│
├── services/
│   ├── __init__.py
│   ├── auth_service.py        # Authentication & session tracking logic
│   └── account_service.py     # Transaction execution, transfers, & closure checks
│
├── storage/
│   ├── __init__.py
│   └── json_storage.py        # Persistence handlers for file I/O
│
├── main.py                    # Application entry point & interactive CLI menus
└── README.md                  # Project documentation

```

---

## Python Concepts Applied

* **Object-Oriented Programming (OOP)**:
* **Inheritance & Polymorphism**: `SavingsAccount` and `CurrentAccount` inherit from an abstract `Account` base class and override `withdraw()` with specialized balance limit logic.
* **Encapsulation**: Balances and credentials are kept protected/private, exposed only via authorized instance methods.
* **Class & Static Methods**: Used for factory constructors (`Account.from_dict()`) and validation utilities.


* **Error & Exception Handling**: Custom exceptions (`InsufficientFundsError`, `AccountClosureError`, `AccountNotFoundError`) to guard transaction invariants.
* **File Handling**: Reading and writing state cleanly using context managers (`with open(...)`) and the `json` standard library.
* **Higher-Order Functions**: Using `filter()` and `map()` for mini-statement queries and transaction history parsing.

---

## Getting Started

### Prerequisites

* Python 3.8 or higher installed on your system.
* No external third-party dependencies required (uses standard library modules: `os`, `json`, `datetime`, `uuid`).

### Installation

1. Clone or download the repository:
```bash
git clone https://github.com/your-username/banking-system-cli.git
cd banking-system-cli

```


2. Ensure the data directory exists:
```bash
mkdir -p data

```


3. Run the application:
```bash
python main.py

```



---

## Usage Workflow

1. **Launch App**: The console displays the **Public Welcome Menu**.
2. **Register**: Select `[2]` to create a new user profile.
3. **Login**: Authenticate via `[1]` to access the **User Dashboard**.
4. **Open Account**: Choose between a *Savings Account* or *Current Account*.
5. **Manage Funds**: Deposit initial money, execute transfers, or withdraw.
6. **Closing Accounts**: Attempting to delete an account with an active balance prompts fund redirection or liquidation before confirmation.
7. **Logout/Exit**: Exiting automatically commits all unwritten state changes to disk.
