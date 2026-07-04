# 13 – Async/Await and Context Managers

This document explains **asynchronous programming** and **context managers** in Python. FastAPI uses both: `async def` endpoints for file uploads, and `asynccontextmanager` for application startup/shutdown (lifespan).

Read [08-decorators.md](08-decorators.md) and [02-file-io.md](02-file-io.md) (the `with` statement) first.

---

## 1. Synchronous vs Asynchronous

### Synchronous (normal) code

Runs one step at a time. Each step blocks until it finishes:

```python
def read_file():
    data = open("file.txt").read()  # waits until read completes
    return data
```

### Asynchronous code

Can pause while waiting for slow operations (network, disk, database) and let other work run:

```python
async def read_upload(file):
    content = await file.read()  # pauses here; other tasks can run
    return content
```

| Term | Meaning |
|------|---------|
| `async def` | Defines a coroutine function (can be paused) |
| `await` | Pauses until the awaited operation completes |
| Coroutine | The object returned when you call an `async def` function |

---

## 2. When to Use `async`

Use `async def` when your function performs I/O that can wait:

* Reading uploaded files (`UploadFile.read()`)
* Database queries (with async drivers)
* HTTP calls to other services
* Application lifespan hooks

You do **not** need `async` for simple endpoints that only return a dictionary:

```python
@app.get("/")
def home():
    return {"message": "OK"}
```

Both sync and async endpoints work in FastAPI. Use `async` when you need to `await` something inside the function.

---

## 3. Basic Async Example

```python
import asyncio

async def fetch_data():
    print("Starting fetch")
    await asyncio.sleep(1)  # simulates waiting for network
    print("Fetch complete")
    return {"data": 42}

async def main():
    result = await fetch_data()
    print(result)

asyncio.run(main())
```

Output:

```text
Starting fetch
Fetch complete
{'data': 42}
```

---

## 4. Async in FastAPI

```python
from fastapi import FastAPI, UploadFile

app = FastAPI()

@app.post("/upload")
async def upload(file: UploadFile):
    content = await file.read()
    return {"size": len(content)}
```

`file.read()` is asynchronous. You must `await` it inside an `async def` function.

---

## 5. Context Managers (`with`)

A context manager runs setup code before a block and cleanup code after it. The `with` statement is the standard way to use one.

```python
with open("file.txt") as f:
    data = f.read()
# file is automatically closed here
```

| Phase | What happens |
|-------|--------------|
| Enter `with` | `open()` returns a file object; `__enter__` runs |
| Inside block | You use the resource |
| Exit `with` | `__exit__` runs (closes the file, even on error) |

Context managers prevent resource leaks (open files, database connections).

---

## 6. `yield` in Context Managers

Some context managers use `yield` to separate setup from teardown:

```python
from contextlib import contextmanager

@contextmanager
def connect():
    print("Connecting")
    connection = "db-connection"
    yield connection
    print("Disconnecting")

with connect() as conn:
    print(f"Using {conn}")
```

Output:

```text
Connecting
Using db-connection
Disconnecting
```

* Code **before** `yield` = setup (startup)
* Code **after** `yield` = teardown (shutdown)

---

## 7. Async Context Managers

For async resources, use `async with` and `asynccontextmanager`:

```python
from contextlib import asynccontextmanager

@asynccontextmanager
async def lifespan(app):
    print("App starting")
    yield
    print("App shutting down")
```

FastAPI passes this to `FastAPI(lifespan=lifespan)`:

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI

@asynccontextmanager
async def lifespan(app: FastAPI):
    # startup
    app.state.db = await connect_to_db()
    yield
    # shutdown
    await app.state.db.close()

app = FastAPI(lifespan=lifespan)
```

See [FastAPI lifespan](../libraries/fastapi/15-lifespan.md) for the full pattern.

---

## 8. Generators and `yield` in Dependencies

FastAPI dependencies can use `yield` to run cleanup after a request:

```python
def get_session():
    session = Session(engine)
    try:
        yield session
    finally:
        session.close()
```

This is the same setup/teardown idea as context managers, used per request instead of per application.

---

## 9. Common Mistakes

### Forgetting `await`

```python
# Wrong — returns a coroutine object, not data
content = file.read()

# Correct
content = await file.read()
```

### Using `await` in a normal `def`

```python
# Wrong
def upload(file: UploadFile):
    content = await file.read()  # SyntaxError

# Correct
async def upload(file: UploadFile):
    content = await file.read()
```

### Mixing sync blocking calls in async endpoints

Avoid long blocking operations (like `time.sleep(10)`) inside `async def` functions. They block the entire event loop. Use `await asyncio.sleep(10)` or run blocking code in a thread pool.

---

## Summary

* `async def` and `await` let Python pause during slow I/O operations.
* Use `async` in FastAPI when you need to `await` file reads, database calls, or similar.
* `with` and context managers handle setup and cleanup of resources.
* `asynccontextmanager` and `yield` split startup/shutdown logic — used in FastAPI lifespan handlers.
* `yield` in dependencies runs cleanup after each request.
