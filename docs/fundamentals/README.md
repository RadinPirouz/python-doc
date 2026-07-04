# Python Fundamentals

Ordered reference for core Python language topics. Read these **before** the [FastAPI](../libraries/fastapi/) guides if you are new to Python or web APIs.

## Core Python

| # | File | Topic |
|---|------|-------|
| 01 | [01-data-types.md](01-data-types.md) | Data types and basic operations |
| 02 | [02-file-io.md](02-file-io.md) | File input/output |
| 03 | [03-loops.md](03-loops.md) | Loops and iteration |
| 04 | [04-operators.md](04-operators.md) | Operators |
| 05 | [05-oop.md](05-oop.md) | Object-oriented programming |
| 06 | [06-packages-modules.md](06-packages-modules.md) | Packages, modules, venv, and pip |
| 07 | [07-error-handling.md](07-error-handling.md) | Error handling |
| 08 | [08-decorators.md](08-decorators.md) | Decorators |

## Topics Required for FastAPI

| # | File | Topic | Used in FastAPI for |
|---|------|-------|---------------------|
| 09 | [09-standard-library.md](09-standard-library.md) | Standard library overview | General utilities |
| 10 | [10-type-hints.md](10-type-hints.md) | Type hints and `typing` | Request validation, path/query params |
| 11 | [11-json.md](11-json.md) | JSON format and `json` module | Request/response bodies |
| 12 | [12-http-basics.md](12-http-basics.md) | HTTP methods, URLs, status codes | All endpoints |
| 13 | [13-async-await.md](13-async-await.md) | Async/await and context managers | File uploads, lifespan events |
| 14 | [14-pydantic-basics.md](14-pydantic-basics.md) | Pydantic models and validation | Request/response schemas |
| 15 | [15-databases.md](15-databases.md) | Databases, SQL, ORM, sessions | SQLModel, persistent storage |

## Suggested Reading Order for FastAPI

```
01-data-types → 03-loops → 05-oop → 06-packages-modules
       ↓
08-decorators → 10-type-hints → 11-json → 12-http-basics
       ↓
14-pydantic-basics → 15-databases
       ↓
13-async-await (when needed)
       ↓
FastAPI 01-setup onward
```
