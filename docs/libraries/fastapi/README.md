# FastAPI

[FastAPI](https://fastapi.tiangolo.com/) is a modern Python web framework for building HTTP APIs. It uses type hints and Pydantic for automatic validation and generates interactive documentation at `/docs`.

**This guide assumes you are learning.** Python and HTTP basics are covered in [Fundamentals](../fundamentals/) — read those first if any topic is unfamiliar.

---

## Before You Start

| Topic | Fundamentals chapter |
|-------|---------------------|
| Dictionaries, lists, loops | [01-data-types](../fundamentals/01-data-types.md), [03-loops](../fundamentals/03-loops.md) |
| Classes | [05-oop](../fundamentals/05-oop.md) |
| Imports, venv, pip | [06-packages-modules](../fundamentals/06-packages-modules.md) |
| `@decorator` syntax | [08-decorators](../fundamentals/08-decorators.md) |
| Type hints (`str`, `int`, `Optional`) | [10-type-hints](../fundamentals/10-type-hints.md) |
| JSON request/response bodies | [11-json](../fundamentals/11-json.md) |
| GET, POST, status codes, URLs | [12-http-basics](../fundamentals/12-http-basics.md) |
| `async`/`await`, lifespan | [13-async-await](../fundamentals/13-async-await.md) |
| Pydantic `BaseModel` | [14-pydantic-basics](../fundamentals/14-pydantic-basics.md) |
| Databases, SQL, sessions | [15-databases](../fundamentals/15-databases.md) |

---

## Chapters

| # | File | Topic |
|---|------|-------|
| 01 | [01-setup.md](01-setup.md) | Virtual environment and install |
| 02 | [02-simple-route.md](02-simple-route.md) | First app, Uvicorn, `/docs` |
| 03 | [03-get-method.md](03-get-method.md) | GET endpoints, path parameters |
| 04 | [04-post-method.md](04-post-method.md) | POST with JSON body |
| 05 | [05-put-method.md](05-put-method.md) | PUT updates |
| 06 | [06-delete-method.md](06-delete-method.md) | DELETE resources |
| 07 | [07-query-parameters.md](07-query-parameters.md) | Query strings and filtering |
| 08 | [08-status-codes.md](08-status-codes.md) | Status codes and `HTTPException` |
| 09 | [09-responses.md](09-responses.md) | `response_model`, `JSONResponse` |
| 10 | [10-query-path-advanced-params.md](10-query-path-advanced-params.md) | `Path()`, `Query()` validation |
| 11 | [11-post-types.md](11-post-types.md) | Content-Type overview |
| 12 | [12-form-post.md](12-form-post.md) | Form data with `Form()` |
| 13 | [13-body-post.md](13-body-post.md) | JSON body with `Body()` |
| 14 | [14-file-upload.md](14-file-upload.md) | File uploads |
| 15 | [15-lifespan.md](15-lifespan.md) | Startup/shutdown with lifespan |
| 16 | [16-template.md](16-template.md) | HTML templates (Jinja2) |
| 17 | [17-sqlmodel.md](17-sqlmodel.md) | SQLModel database CRUD |

---

## Learning Path

```
Setup (01) → Simple route (02) → GET (03) → POST (04)
    → PUT (05) → DELETE (06) → Query params (07)
    → Status codes (08) → Responses (09)
    → Advanced params (10) → POST types (11–14)
    → Lifespan (15) → Templates (16) → SQLModel (17)
```

Chapters 03–06 build a complete CRUD example using an in-memory list. Chapter 17 replaces that list with a real database. Chapters 07–10 deepen parameter and error handling. Chapters 11–14 cover different request body formats.

---

## How These Docs Are Organized

* **Fundamentals** explain Python and HTTP concepts once, in depth.
* **FastAPI chapters** show how to apply those concepts in real endpoints.
* Each chapter lists **Prerequisites** at the top — follow the links if you get stuck.

Do not skip fundamentals chapters for topics you have not seen before (decorators, type hints, JSON, HTTP methods). FastAPI builds directly on all of them.
