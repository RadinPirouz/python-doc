# FastAPI – Application Lifespan (Startup & Shutdown Events)

FastAPI allows you to run code when your application **starts up** or **shuts down**.
This is useful for initializing resources, database connections, caches, or background tasks.

---

## Deprecated Method: `@app.on_event`

Older FastAPI versions use the `@app.on_event` decorator for lifecycle events:

```python
from fastapi import FastAPI

app = FastAPI()


@app.on_event("startup")
def on_startup():
    print("App is loading")


@app.on_event("shutdown")
def on_shutdown():
    print("App is shutting down")
```

### Characteristics

* `startup` runs once when the app starts
* `shutdown` runs once when the app is stopped
* Works for synchronous and asynchronous functions
* Still supported but **deprecated** in favor of the `lifespan` parameter

---

## Recommended Modern Approach: `lifespan` with `asynccontextmanager`

FastAPI now recommends using the `lifespan` parameter in the `FastAPI` constructor.
This uses Python's `asynccontextmanager` to define a **single lifecycle context**.

```python
from fastapi import FastAPI
from contextlib import asynccontextmanager

@asynccontextmanager
async def lifespan(app: FastAPI):
    # Code to run before the app starts
    print("App is loading")
    yield  # Application runs after this point
    # Code to run after the app stops
    print("App is shutting down")

app = FastAPI(lifespan=lifespan)
```

### How It Works

1. Code **before `yield`** executes on startup
2. Code **after `yield`** executes on shutdown
3. Supports async operations, e.g., connecting to a database

---

## Example: Using Lifespan for Database Initialization

```python
from fastapi import FastAPI
from contextlib import asynccontextmanager

@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.db = await connect_to_db()
    print("Database connected")
    yield
    await app.state.db.close()
    print("Database disconnected")

app = FastAPI(lifespan=lifespan)
```

* `app.state` is used to store shared resources
* Clean startup and shutdown handling
* Ensures proper resource cleanup

---

## Benefits of the Lifespan Approach

* Centralized lifecycle management
* Cleaner async support
* Avoids multiple scattered `@app.on_event` decorators
* Better for testing and production-ready apps

---

## Running the Application

Start the service with `uvicorn`:

```bash
uvicorn main:app --reload
```

* On startup, `App is loading` prints to the console
* On shutdown (Ctrl+C), `App is shutting down` prints to the console

---

## Best Practices

* Always use `lifespan` for new applications
* Use `app.state` to store shared resources
* Close database connections, caches, or background services in shutdown
* Keep startup logic lightweight to avoid blocking the server
* Avoid printing in production; use logging instead

---

This approach provides a **modern, production-ready pattern** for managing application startup and shutdown events in FastAPI.

