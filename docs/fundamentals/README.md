# Python Fundamentals

Ordered reference aligned with the Python course videos. Read these **before** the [FastAPI](../libraries/fastapi/) guides if you are new to Python or web APIs.

## Core Python Course (00–30)

| # | File | Topic |
|---|------|-------|
| 00 | [00-intro.md](00-intro.md) | Introduction |
| 01 | [01-variables.md](01-variables.md) | Variables and data types |
| 02 | [02-string.md](02-string.md) | Strings |
| 03 | [03-list.md](03-list.md) | Lists |
| 04 | [04-while.md](04-while.md) | While loops |
| 05 | [05-if.md](05-if.md) | If statements |
| 06 | [06-for.md](06-for.md) | For loops |
| 07 | [07-range.md](07-range.md) | Range |
| 08 | [08-break-continue-pass.md](08-break-continue-pass.md) | Break, continue, pass |
| 09 | [09-function.md](09-function.md) | Functions |
| 10 | [10-more-on-list.md](10-more-on-list.md) | More on lists |
| 11 | [11-tuple.md](11-tuple.md) | Tuples |
| 12 | [12-set.md](12-set.md) | Sets |
| 13 | [13-dict.md](13-dict.md) | Dictionaries |
| 14 | [14-module.md](14-module.md) | Modules and packages |
| 15 | [15-fstring.md](15-fstring.md) | F-strings |
| 16 | [16-format.md](16-format.md) | String formatting |
| 17 | [17-files.md](17-files.md) | File I/O |
| 18 | [18-scope.md](18-scope.md) | Scope |
| 19 | [19-class.md](19-class.md) | Classes |
| 20 | [20-method.md](20-method.md) | Methods |
| 21 | [21-class-variable.md](21-class-variable.md) | Class variables |
| 22 | [22-class-static-method.md](22-class-static-method.md) | Static and class methods |
| 23 | [23-inheritance.md](23-inheritance.md) | Inheritance |
| 24 | [24-special-method.md](24-special-method.md) | Special (magic) methods |
| 25 | [25-access-point.md](25-access-point.md) | Access modifiers |
| 26 | [26-property.md](26-property.md) | Property |
| 27 | [27-exception.md](27-exception.md) | Exception handling |
| 28 | [28-standard-library.md](28-standard-library.md) | Standard library |
| 29 | [29-docstring.md](29-docstring.md) | Docstrings |
| 30 | [30-venv.md](30-venv.md) | Virtual environments and pip |

## Topics Required for FastAPI (31+)

| # | File | Topic | Used in FastAPI for |
|---|------|-------|---------------------|
| 31 | [31-decorators.md](31-decorators.md) | Decorators | `@app.get`, route registration |
| 32 | [32-type-hints.md](32-type-hints.md) | Type hints and `typing` | Request validation, path/query params |
| 33 | [33-json.md](33-json.md) | JSON format and `json` module | Request/response bodies |
| 34 | [34-http-basics.md](34-http-basics.md) | HTTP methods, URLs, status codes | All endpoints |
| 35 | [35-async-await.md](35-async-await.md) | Async/await and context managers | File uploads, lifespan events |
| 36 | [36-pydantic-basics.md](36-pydantic-basics.md) | Pydantic models and validation | Request/response schemas |
| 37 | [37-databases.md](37-databases.md) | Databases, SQL, ORM, sessions | SQLModel, persistent storage |
| 38 | [38-threading.md](38-threading.md) | Threading basics | Concurrent tasks |

## Suggested Reading Order for FastAPI

```
00-intro → 01-variables → 03-list → 13-dict → 06-for
       ↓
19-class → 14-module → 30-venv
       ↓
31-decorators → 32-type-hints → 33-json → 34-http-basics
       ↓
36-pydantic-basics → 37-databases
       ↓
35-async-await (when needed)
       ↓
FastAPI 01-setup onward
```
