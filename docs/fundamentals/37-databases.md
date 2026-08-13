# 37 – Databases and SQL Basics

This document explains **relational databases** and **SQL** at a conceptual level. You need these ideas before using [SQLModel](../libraries/fastapi/17-sqlmodel.md) with FastAPI.

**Prerequisites:** [03-list.md](03-list.md) and [13-dict.md](13-dict.md) (dictionaries and lists), [36-pydantic-basics.md](36-pydantic-basics.md) (structured data models).

---

## 1. Why Use a Database?

In earlier FastAPI examples, data lived in a Python list:

```python
users = [
    {"name": "abbas", "age": 20},
    {"name": "mmd", "age": 37},
]
```

Problems with in-memory lists:

| Problem | What happens |
|---------|--------------|
| Data is lost on restart | The list resets when the server stops |
| No concurrent safety | Multiple requests can corrupt shared state |
| No querying at scale | Searching large lists in Python is slow |
| No persistence | Data cannot survive deployments or crashes |

A **database** stores data on disk (or a remote server) so it survives restarts and supports efficient queries.

---

## 2. Relational Databases

A **relational database** stores data in **tables** — like spreadsheets with rows and columns.

### Example: `users` table

| id | name   | age |
|----|--------|-----|
| 1  | abbas  | 20  |
| 2  | mmd    | 37  |
| 3  | asghar | 19  |

| Term | Meaning |
|------|---------|
| **Table** | A collection of related records (e.g. all users) |
| **Row** | One record (one user) |
| **Column** | One field (name, age) |
| **Primary key** | Unique identifier for each row (usually `id`) |

Common relational databases: **SQLite** (file-based, good for learning), **PostgreSQL** and **MySQL** (production).

---

## 3. SQL — The Database Language

**SQL** (Structured Query Language) is how you read and write database data.

### CRUD operations

| Operation | SQL command | Purpose |
|-----------|-------------|---------|
| **C**reate | `INSERT` | Add a new row |
| **R**ead | `SELECT` | Fetch rows |
| **U**pdate | `UPDATE` | Change existing rows |
| **D**elete | `DELETE` | Remove rows |

### Examples

```sql
-- Read all users
SELECT * FROM users;

-- Read one user by id
SELECT * FROM users WHERE id = 1;

-- Create a user
INSERT INTO users (name, age) VALUES ('ali', 25);

-- Update a user
UPDATE users SET age = 26 WHERE id = 1;

-- Delete a user
DELETE FROM users WHERE id = 1;
```

You do not write raw SQL in most FastAPI + SQLModel apps — SQLModel generates it for you. But understanding SQL helps you debug and design tables.

---

## 4. ORM — Object-Relational Mapping

An **ORM** lets you work with database rows as Python objects instead of writing SQL by hand.

```
Python class User  ↔  users table
User(name="ali")   ↔  row in database
```

| Without ORM | With ORM (SQLModel) |
|-------------|---------------------|
| Write SQL strings | Use Python classes and methods |
| Manual row parsing | Objects with attributes |
| Error-prone string formatting | Type-safe queries |

SQLModel is an ORM built on **SQLAlchemy** (database layer) and **Pydantic** (validation layer).

---

## 5. Engine, Session, and Transactions

These are the core SQLModel/SQLAlchemy concepts:

### Engine

The **engine** is the connection to the database file or server.

```python
from sqlmodel import create_engine

engine = create_engine("sqlite:///database.db")
```

One engine per application is typical.

### Session

A **session** is a workspace for a group of database operations. Think of it as a temporary buffer between your Python code and the database.

```python
from sqlmodel import Session

with Session(engine) as session:
    session.add(user)
    session.commit()
```

| Method | Purpose |
|--------|---------|
| `session.add(obj)` | Stage a new or changed row for saving |
| `session.commit()` | Write staged changes to the database |
| `session.refresh(obj)` | Reload the object from the database (e.g. to get auto-generated `id`) |
| `session.delete(obj)` | Mark a row for deletion |
| `session.get(Model, id)` | Fetch one row by primary key |
| `session.exec(select(...))` | Run a query |

### Transaction

A **transaction** is a group of operations that succeed or fail together. `commit()` saves all changes; if an error occurs before commit, changes are rolled back.

**Rule of thumb in FastAPI:** use **one session per request**. Open it at the start, commit if needed, close it when the request ends.

---

## 6. SQLite for Learning

SQLite stores the entire database in a single file (e.g. `database.db`). No separate database server is required.

```python
sqlite_file_name = "database.db"
sqlite_url = f"sqlite:///{sqlite_file_name}"
engine = create_engine(sqlite_url, connect_args={"check_same_thread": False})
```

`check_same_thread=False` is required for SQLite with FastAPI because multiple threads may handle requests. As long as each request has its own session, this is safe.

For production, use PostgreSQL or another server database — not SQLite on a shared production server.

---

## 7. Table Models vs API Models

In APIs you often need different shapes for input and output:

| Model | Purpose | Example |
|-------|---------|---------|
| **Table model** | Maps to the database table | `User` with `id`, `name`, `password_hash` |
| **Create model** | What the client sends to create a record | `UserCreate` — no `id` |
| **Public model** | What the client receives | `UserPublic` — no `password_hash` |
| **Update model** | Partial updates (PATCH) | `UserUpdate` — all fields optional |

See [36 – Pydantic basics](36-pydantic-basics.md) for the same input/output separation pattern. SQLModel combines Pydantic models with database table definitions.

---

## 8. In-Memory List vs Database

| Feature | In-memory list | Database |
|---------|----------------|----------|
| Survives restart | No | Yes |
| Suitable for production | No | Yes |
| Query by field | Manual loop | Indexed queries |
| Concurrent requests | Risky | Designed for it |
| Learning / prototyping | Fast to start | Slightly more setup |

FastAPI chapters 03–06 use lists for learning. [17 – SQLModel](../libraries/fastapi/17-sqlmodel.md) replaces the list with a real database.

---

## Summary

* Databases persist data across restarts; tables store rows with columns.
* SQL provides CRUD operations: `SELECT`, `INSERT`, `UPDATE`, `DELETE`.
* An ORM maps Python classes to tables — SQLModel does this for FastAPI.
* The **engine** connects to the database; a **session** handles operations per request.
* Use separate create/public/update models to control what clients send and receive.
* SQLite is fine for learning; use PostgreSQL (or similar) in production.
