# FastAPI – Simple Route Example

This document demonstrates how to create a basic FastAPI application with a single HTTP route and how to run it using `uvicorn`, which is the default and recommended ASGI server for FastAPI.

---

## Create a Simple FastAPI Application

Create a Python file named `main.py` and add the following content:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def home_dir():
    return {"message": "Home Page"}
```

### Explanation

* `FastAPI()` initializes the application instance.
* `@app.get("/")` registers an HTTP GET endpoint at the root path (`/`).
* The `home_dir` function is the request handler and returns a JSON response.
* FastAPI automatically handles JSON serialization and response headers.

---

## Running the Application

FastAPI applications are typically run using **uvicorn**, an ASGI server designed for high performance.

### Option 1: Run Using FastAPI CLI (Development Mode)

FastAPI provides a development-friendly CLI wrapper that uses `uvicorn` internally:

```bash
fastapi dev main.py
```

This command:

* Starts the application in development mode
* Enables auto-reload on code changes
* Automatically binds to a local development interface

---

### Option 2: Run Directly with Uvicorn (Recommended)

For explicit control over runtime configuration, run the application directly with `uvicorn`:

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 1234
```

#### Command Breakdown

* `main` → Python file name (without `.py`)
* `app` → FastAPI application instance
* `--reload` → Automatically reloads the server on code changes (development only)
* `--host 0.0.0.0` → Exposes the service on all network interfaces
* `--port 1234` → Custom application port

---

## Accessing the Application

Once running, the application will be available at:

* API Endpoint:
  `http://localhost:1234/`

* Interactive API Docs (Swagger UI):
  `http://localhost:1234/docs`

* Alternative API Docs (ReDoc):
  `http://localhost:1234/redoc`

---

## Best Practices

* Use `--reload` only in development environments
* In production, run `uvicorn` behind a process manager (e.g., systemd, Docker, Kubernetes)
* Explicitly define host and port for containerized and cloud deployments
* Keep the application entry point (`main:app`) consistent across environments

