# FastAPI – SQLModel and Database CRUD

## Prerequisites

* [37 – Databases and SQL basics](../../fundamentals/37-databases.md) — tables, sessions, ORM concepts
* [36 – Pydantic basics](../../fundamentals/36-pydantic-basics.md) — `BaseModel`, validation, input/output models
* [32 – Type hints](../../fundamentals/32-type-hints.md) — `Annotated`, `Depends`
* [35 – Async/await](../../fundamentals/35-async-await.md) — `yield` in dependencies and lifespan
* [15-lifespan](15-lifespan.md) — startup hooks (create tables on boot)
* Chapters [03](03-get-method.md)–[09](09-responses.md) — HTTP methods, status codes, `response_model`

## Overview

[SQLModel](https://sqlmodel.tiangolo.com/) is a library created by the same author as FastAPI. It combines:

| Layer | Library | Role |
|-------|---------|------|
| Database ORM | SQLAlchemy | Talks to the database |
| Data validation | Pydantic | Validates API input and output |

One SQLModel class can be both a **database table model** and a **Pydantic data model**, so you write less duplicate code than using SQLAlchemy and Pydantic separately.

This document replaces the in-memory `users` list from earlier chapters with a **SQLite** database and full CRUD endpoints.

---

# 1. Install SQLModel

```bash
pip install sqlmodel
```

SQLModel installs SQLAlchemy automatically. You also need FastAPI and Uvicorn (from [01-setup](01-setup.md)):

```bash
pip install fastapi uvicorn sqlmodel
```

---

# 2. Project Structure

```text
project/
├── main.py
└── database.db    # created automatically on first run
```

---

# 3. Define Models

Use separate models for the database table, API input, and API output. See [37 – Databases](../../fundamentals/37-databases.md#7-table-models-vs-api-models).

```python
from sqlmodel import Field, SQLModel


class UserBase(SQLModel):
    name: str = Field(index=True)
    age: int | None = Field(default=None, index=True)


class User(UserBase, table=True):
    id: int | None = Field(default=None, primary_key=True)
    secret_name: str


class UserCreate(UserBase):
    secret_name: str


class UserPublic(UserBase):
    id: int


class UserUpdate(SQLModel):
    name: str | None = None
    age: int | None = None
    secret_name: str | None = None
```

| Model | `table=True` | Purpose |
|-------|--------------|---------|
| `UserBase` | No | Shared fields |
| `User` | Yes | Database table — includes `secret_name` |
| `UserCreate` | No | POST body — client sends this |
| `UserPublic` | No | API response — hides `secret_name` |
| `UserUpdate` | No | PATCH body — all fields optional |

`Field(index=True)` asks the database to index that column for faster lookups. `primary_key=True` on `id` makes it the unique row identifier.

---

# 4. Create the Database Engine

The **engine** opens a connection to the database. See [37 – Databases](../../fundamentals/37-databases.md#5-engine-session-and-transactions).

```python
from sqlmodel import create_engine

sqlite_file_name = "database.db"
sqlite_url = f"sqlite:///{sqlite_file_name}"

connect_args = {"check_same_thread": False}
engine = create_engine(sqlite_url, echo=True, connect_args=connect_args)
```

| Setting | Purpose |
|---------|---------|
| `sqlite:///database.db` | Store data in a local file |
| `echo=True` | Print SQL statements to the terminal (useful for learning; disable in production) |
| `check_same_thread=False` | Required for SQLite with FastAPI's multi-threaded server |

---

# 5. Create Tables on Startup

Tables must exist before you insert data. Create them once when the application starts.

### Recommended: lifespan

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI
from sqlmodel import SQLModel


def create_db_and_tables():
    SQLModel.metadata.create_all(engine)


@asynccontextmanager
async def lifespan(app: FastAPI):
    create_db_and_tables()
    yield


app = FastAPI(lifespan=lifespan)
```

See [15-lifespan](15-lifespan.md) for the lifespan pattern.

### Deprecated alternative

```python
@app.on_event("startup")
def on_startup():
    create_db_and_tables()
```

Do not use both `lifespan` and `@app.on_event` in the same app.

---

# 6. Database Session

A **session** is a workspace for database operations during one request. See [37 – Databases](../../fundamentals/37-databases.md#5-engine-session-and-transactions).

```python
from sqlmodel import Session


def get_session():
    with Session(engine) as session:
        yield session
```

| Step | What happens |
|------|--------------|
| `Session(engine)` | Open a connection to the database |
| `yield session` | Hand the session to the endpoint |
| After request ends | Context manager closes the session |

Using `yield` means FastAPI runs cleanup after the response is sent — the same pattern as [35 – Async/await](../../fundamentals/35-async-await.md#8-generators-and-yield-in-dependencies).

---

# 7. Inject Session with `Depends`

Instead of opening a session manually in every endpoint, use a FastAPI **dependency**:

```python
from typing import Annotated

from fastapi import Depends
from sqlmodel import Session

SessionDep = Annotated[Session, Depends(get_session)]
```

Then inject it into any endpoint:

```python
def create_user(session: SessionDep, user: UserCreate):
    ...
```

FastAPI calls `get_session()`, passes the yielded `session` to your function, and closes it when the request completes.

---

# 8. CRUD Endpoints

## Create — POST

```python
from fastapi import status

@app.post("/users/", response_model=UserPublic, status_code=status.HTTP_201_CREATED)
def create_user(session: SessionDep, user: UserCreate):
    db_user = User.model_validate(user)
    session.add(db_user)
    session.commit()
    session.refresh(db_user)
    return db_user
```

| Step | Purpose |
|------|---------|
| `User.model_validate(user)` | Convert API input to a database model instance |
| `session.add(db_user)` | Stage the new row |
| `session.commit()` | Save to the database |
| `session.refresh(db_user)` | Reload to get the auto-generated `id` |

`response_model=UserPublic` hides `secret_name` from the response — see [09-responses](09-responses.md).

---

## Read all — GET

```python
from fastapi import Query
from sqlmodel import select

@app.get("/users/", response_model=list[UserPublic])
def read_users(
    session: SessionDep,
    offset: int = 0,
    limit: int = Query(default=100, le=100),
):
    users = session.exec(select(User).offset(offset).limit(limit)).all()
    return users
```

`select(User)` builds a query. `.offset()` and `.limit()` paginate results — see [07-query-parameters](07-query-parameters.md).

---

## Read one — GET

```python
from fastapi import HTTPException

@app.get("/users/{user_id}", response_model=UserPublic)
def read_user(session: SessionDep, user_id: int):
    user = session.get(User, user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

`session.get(User, user_id)` fetches one row by primary key.

---

## Update — PATCH

```python
@app.patch("/users/{user_id}", response_model=UserPublic)
def update_user(session: SessionDep, user_id: int, user: UserUpdate):
    db_user = session.get(User, user_id)
    if not db_user:
        raise HTTPException(status_code=404, detail="User not found")

    update_data = user.model_dump(exclude_unset=True)
    db_user.sqlmodel_update(update_data)
    session.add(db_user)
    session.commit()
    session.refresh(db_user)
    return db_user
```

`model_dump(exclude_unset=True)` includes only fields the client actually sent. `sqlmodel_update()` applies them to the database row.

---

## Delete — DELETE

```python
@app.delete("/users/{user_id}")
def delete_user(session: SessionDep, user_id: int):
    user = session.get(User, user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    session.delete(user)
    session.commit()
    return {"ok": True}
```

---

# 9. Complete Application

```python
from contextlib import asynccontextmanager
from typing import Annotated

from fastapi import Depends, FastAPI, HTTPException, Query, status
from sqlmodel import Field, Session, SQLModel, create_engine, select

# --- Models ---

class UserBase(SQLModel):
    name: str = Field(index=True)
    age: int | None = Field(default=None, index=True)


class User(UserBase, table=True):
    id: int | None = Field(default=None, primary_key=True)
    secret_name: str


class UserCreate(UserBase):
    secret_name: str


class UserPublic(UserBase):
    id: int


class UserUpdate(SQLModel):
    name: str | None = None
    age: int | None = None
    secret_name: str | None = None


# --- Database ---

sqlite_file_name = "database.db"
sqlite_url = f"sqlite:///{sqlite_file_name}"
connect_args = {"check_same_thread": False}
engine = create_engine(sqlite_url, connect_args=connect_args)


def create_db_and_tables():
    SQLModel.metadata.create_all(engine)


def get_session():
    with Session(engine) as session:
        yield session


SessionDep = Annotated[Session, Depends(get_session)]


# --- App ---

@asynccontextmanager
async def lifespan(app: FastAPI):
    create_db_and_tables()
    yield


app = FastAPI(lifespan=lifespan)


@app.get("/")
def root():
    return {"message": "API is working"}


@app.post("/users/", response_model=UserPublic, status_code=status.HTTP_201_CREATED)
def create_user(session: SessionDep, user: UserCreate):
    db_user = User.model_validate(user)
    session.add(db_user)
    session.commit()
    session.refresh(db_user)
    return db_user


@app.get("/users/", response_model=list[UserPublic])
def read_users(
    session: SessionDep,
    offset: int = 0,
    limit: int = Query(default=100, le=100),
):
    users = session.exec(select(User).offset(offset).limit(limit)).all()
    return users


@app.get("/users/{user_id}", response_model=UserPublic)
def read_user(session: SessionDep, user_id: int):
    user = session.get(User, user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user


@app.patch("/users/{user_id}", response_model=UserPublic)
def update_user(session: SessionDep, user_id: int, user: UserUpdate):
    db_user = session.get(User, user_id)
    if not db_user:
        raise HTTPException(status_code=404, detail="User not found")
    update_data = user.model_dump(exclude_unset=True)
    db_user.sqlmodel_update(update_data)
    session.add(db_user)
    session.commit()
    session.refresh(db_user)
    return db_user


@app.delete("/users/{user_id}")
def delete_user(session: SessionDep, user_id: int):
    user = session.get(User, user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    session.delete(user)
    session.commit()
    return {"ok": True}
```

---

# 10. Running the Application

```bash
uvicorn main:app --reload
```

Open interactive docs:

```text
http://localhost:8000/docs
```

After creating users, a `database.db` file appears in your project folder. Data persists across server restarts.

---

# 11. Testing with curl

## Create a user

```bash
curl -X POST "http://localhost:8000/users/" \
  -H "Content-Type: application/json" \
  -d '{"name": "abbas", "age": 20, "secret_name": "secret-abbas"}'
```

Response (`secret_name` is stored but not returned):

```json
{
  "name": "abbas",
  "age": 20,
  "id": 1
}
```

## List users

```bash
curl "http://localhost:8000/users/"
```

## Get one user

```bash
curl "http://localhost:8000/users/1"
```

## Update a user

```bash
curl -X PATCH "http://localhost:8000/users/1" \
  -H "Content-Type: application/json" \
  -d '{"age": 21}'
```

## Delete a user

```bash
curl -X DELETE "http://localhost:8000/users/1"
```

---

# 12. Endpoint Summary

| Method | Path | Purpose |
|--------|------|---------|
| GET | `/` | Health check |
| POST | `/users/` | Create user |
| GET | `/users/` | List users (paginated) |
| GET | `/users/{user_id}` | Get one user |
| PATCH | `/users/{user_id}` | Partial update |
| DELETE | `/users/{user_id}` | Delete user |

---

# 13. SQLModel vs In-Memory List

| Topic | In-memory list (ch. 03–06) | SQLModel (this chapter) |
|-------|---------------------------|-------------------------|
| Data survives restart | No | Yes |
| Production ready | No | Yes (with PostgreSQL) |
| Validation | Manual / Pydantic | Pydantic + database constraints |
| Querying | Python loops | SQL via `select()` |
| Hidden fields | `response_model` | `UserPublic` model |

---

# 14. Best Practices

* Use **one session per request** via `Depends(get_session)` — never share sessions across requests.
* Separate **table**, **create**, **public**, and **update** models.
* Never return sensitive fields (`secret_name`, passwords) — use `UserPublic` or `response_model`.
* Create tables on startup with `lifespan`, not on every request.
* Use `echo=True` only during development to see generated SQL.
* Replace SQLite with **PostgreSQL** in production.
* Add database migrations (e.g. Alembic) when your schema changes — `create_all()` does not alter existing tables.
* Use `HTTPException` with proper status codes for missing records — see [08-status-codes](08-status-codes.md).
* Commit after `add()` or `delete()`; call `refresh()` when you need database-generated values like `id`.

---

# 15. Production Note

A typical production stack:

```text
FastAPI + SQLModel
PostgreSQL (not SQLite)
Alembic (schema migrations)
Gunicorn with Uvicorn workers
Nginx or Traefik
```

Change the database URL for PostgreSQL:

```python
# Example — use environment variables for real credentials
database_url = "postgresql://user:password@localhost:5432/mydb"
engine = create_engine(database_url)
```

Do not commit `database.db` or production credentials to version control.

---

## Further Reading

* [SQLModel documentation](https://sqlmodel.tiangolo.com/)
* [SQLModel + FastAPI tutorial](https://sqlmodel.tiangolo.com/tutorial/fastapi/)
* [37 – Databases and SQL basics](../../fundamentals/37-databases.md)
