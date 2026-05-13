# FastAPI – Simple Route Example

## Overview

This document explains how to create a basic FastAPI application with a single HTTP route and how to run it using `uvicorn`.

FastAPI is an ASGI web framework, and `uvicorn` is commonly used as the ASGI server to run FastAPI applications.

---

# 1. Create a Simple FastAPI Application

Create a Python file named:

```text
main.py
```

Add the following code:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def home_dir():
    return {"message": "Home Page"}
```

---

# 2. Code Explanation

## Import FastAPI

```python
from fastapi import FastAPI
```

This imports the `FastAPI` class from the FastAPI package.

---

## Create the Application Instance

```python
app = FastAPI()
```

This creates the main FastAPI application instance.

The variable name `app` is important because it is later used by `uvicorn` when starting the server.

---

## Define a GET Route

```python
@app.get("/")
def home_dir():
    return {"message": "Home Page"}
```

This creates an HTTP `GET` endpoint at the root path:

```http
GET /
```

When a user sends a request to `/`, the `home_dir` function is executed.

The function returns a Python dictionary:

```python
{"message": "Home Page"}
```

FastAPI automatically converts this dictionary into a JSON response.

---

# 3. Running the Application

FastAPI applications are usually run using `uvicorn`.

---

## Option 1: Run Using FastAPI CLI

You can run the application in development mode using:

```bash
fastapi dev main.py
```

This command starts the FastAPI application in development mode.

It is useful during local development because it supports automatic reload when the source code changes.

---

## Option 2: Run Directly with Uvicorn

You can also run the application directly using `uvicorn`:

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 1234
```

This is the more explicit and commonly used method.

---

# 4. Uvicorn Command Breakdown

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 1234
```

## `uvicorn`

Starts the ASGI server.

## `main`

Refers to the Python file name:

```text
main.py
```

You write `main`, not `main.py`.

## `app`

Refers to the FastAPI application instance:

```python
app = FastAPI()
```

## `--reload`

Automatically restarts the server when code changes.

This should only be used in development.

## `--host 0.0.0.0`

Makes the application listen on all available network interfaces.

This is useful when running inside Docker, virtual machines, or remote servers.

## `--port 1234`

Runs the application on port `1234`.

---

# 5. Accessing the Application

After starting the server, the application will be available at:

```text
http://localhost:1234/
```

The response will be:

```json
{
  "message": "Home Page"
}
```

---

# 6. Interactive API Documentation

FastAPI automatically generates API documentation.

## Swagger UI

```text
http://localhost:1234/docs
```

Swagger UI allows you to test API endpoints directly from the browser.

## ReDoc

```text
http://localhost:1234/redoc
```

ReDoc provides a clean documentation view for the API.

---

# 7. Complete Example

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def home_dir():
    return {"message": "Home Page"}
```

Run it with:

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 1234
```

---

# 8. Best Practices

Use `--reload` only during development.

Use a consistent application entry point such as:

```text
main:app
```

Explicitly define the host and port in Docker, cloud, or server environments.

Use `0.0.0.0` when the application needs to be reachable from outside the local machine.

Use `127.0.0.1` or `localhost` when the application should only be accessible locally.

Do not expose development servers directly to the internet.

In production, run FastAPI behind a proper process manager, reverse proxy, or container orchestration platform.

---

# 9. DevOps Production Note

The following command is suitable for local development:

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 1234
```

For production, avoid using `--reload`.

A typical production setup may include:

```text
FastAPI
Uvicorn or Gunicorn with Uvicorn workers
Nginx or Traefik
Docker or Kubernetes
Logging and monitoring
Health checks
Environment-based configuration
```

Example production-style command:

```bash
uvicorn main:app --host 0.0.0.0 --port 1234
```
