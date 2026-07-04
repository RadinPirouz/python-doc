# FastAPI – Application Lifespan, Startup, and Shutdown Events

## Overview

FastAPI allows you to execute code when the application starts and when it shuts down. This is useful for initializing and cleaning up shared resources such as database connections, cache clients, machine learning models, message queues, or background services.

The modern recommended approach is to use the `lifespan` parameter with an async context manager. The older `@app.on_event()` method is still available, but FastAPI marks it as deprecated in favor of lifespan handlers. ([fastapi.tiangolo.com][1])

---

# 1. Deprecated Method: `@app.on_event`

Older FastAPI applications often use `@app.on_event("startup")` and `@app.on_event("shutdown")`.

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

## Explanation

```python
@app.on_event("startup")
def on_startup():
    print("App is loading")
```

This function runs once when the application starts.

```python
@app.on_event("shutdown")
def on_shutdown():
    print("App is shutting down")
```

This function runs once when the application is shutting down.

## Important Note

FastAPI documentation recommends using the `lifespan` parameter instead of `@app.on_event()`. Also, when a `lifespan` handler is provided, FastAPI does not call the old `startup` and `shutdown` event handlers. You should use one approach consistently, not both. ([fastapi.tiangolo.com][1])

---

# 2. Recommended Method: `lifespan`

The modern approach is to define one lifecycle function using `asynccontextmanager`.

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI


@asynccontextmanager
async def lifespan(app: FastAPI):
    print("App is loading")

    yield

    print("App is shutting down")


app = FastAPI(lifespan=lifespan)
```

---

# 3. How Lifespan Works

The `lifespan` function has two main sections:

```python
@asynccontextmanager
async def lifespan(app: FastAPI):
    # Startup logic
    print("App is loading")

    yield

    # Shutdown logic
    print("App is shutting down")
```

## Code Before `yield`

This runs when the application starts.

Use this section for:

```text
Database connection setup
Cache connection setup
Loading configuration
Initializing shared services
Starting background clients
```

## Code After `yield`

This runs when the application shuts down.

Use this section for:

```text
Closing database connections
Closing cache clients
Flushing logs
Releasing resources
Stopping background services
```

FastAPI passes the lifespan context manager into the application and executes the code before `yield` on startup and after `yield` on shutdown. ([fastapi.tiangolo.com][1])

---

# 4. Example Application

Create or update `main.py` with the following content:

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI


@asynccontextmanager
async def lifespan(app: FastAPI):
    print("App is loading")

    yield

    print("App is shutting down")


app = FastAPI(lifespan=lifespan)


@app.get("/")
def root():
    return {"message": "API is working"}
```

---

# 5. Running the Application

Start the service using `uvicorn`:

```bash
uvicorn main:app --reload
```

When the application starts, the terminal prints:

```text
App is loading
```

When you stop the application, for example with `Ctrl + C`, the terminal prints:

```text
App is shutting down
```

---

# 6. Example: Database Initialization Pattern

In real applications, lifespan is commonly used to initialize and close database connections.

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI


async def connect_to_db():
    # Replace this with a real database connection
    return "database-connection"


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.db = await connect_to_db()
    print("Database connected")

    yield

    # Replace this with real cleanup logic
    print("Database disconnected")


app = FastAPI(lifespan=lifespan)


@app.get("/")
def root():
    return {"message": "API is working"}
```

---

# 7. Using `app.state`

`app.state` is useful for storing shared application-level resources.

Example:

```python
app.state.db = await connect_to_db()
```

Later, this resource can be accessed from the application.

Common resources stored in `app.state` include:

```text
Database clients
Redis clients
HTTP clients
Configuration objects
Service clients
Loaded models
```

---

# 8. Better Database Cleanup Example

If the database client has a `close()` method, close it after `yield`.

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI


@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.db = await connect_to_db()
    print("Database connected")

    yield

    await app.state.db.close()
    print("Database disconnected")


app = FastAPI(lifespan=lifespan)
```

This ensures that the application does not leave open connections after shutdown.

---

# 9. Why Lifespan Is Better

The lifespan approach is better for modern FastAPI applications because it keeps startup and shutdown logic in one place.

It helps with:

```text
Centralized lifecycle management
Cleaner async resource handling
Better application structure
Easier testing
More predictable production behavior
Cleaner resource cleanup
```

---

# 10. Complete Recommended Version

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI


@asynccontextmanager
async def lifespan(app: FastAPI):
    print("Application startup started")

    # Initialize shared resources here
    app.state.service_status = "ready"

    print("Application startup completed")

    yield

    print("Application shutdown started")

    # Clean up shared resources here
    app.state.service_status = "stopped"

    print("Application shutdown completed")


app = FastAPI(lifespan=lifespan)


@app.get("/")
def root():
    return {"message": "API is working"}


@app.get("/health")
def health_check():
    return {
        "status": "healthy",
        "service": app.state.service_status
    }
```

---

# 11. Testing the Application

Run the app:

```bash
uvicorn main:app --reload
```

Open:

```text
http://localhost:8000/
```

Expected response:

```json
{
  "message": "API is working"
}
```

Open:

```text
http://localhost:8000/health
```

Expected response:

```json
{
  "status": "healthy",
  "service": "ready"
}
```

---

# 12. Best Practices

Use `lifespan` for new FastAPI applications.

Avoid using `@app.on_event()` in new code because it is deprecated.

Do not mix `lifespan` with `startup` and `shutdown` event decorators.

Use `app.state` for shared application resources.

Close database connections, cache clients, HTTP clients, and background services during shutdown.

Keep startup logic lightweight.

Avoid using `print()` in production.

Use structured logging instead of printing to the console.

Do not use `--reload` in production.

