# FastAPI – JSONResponse (Explicit JSON Responses)

This document demonstrates how to return **explicit JSON responses** in FastAPI using `JSONResponse`.
While FastAPI automatically serializes dictionaries to JSON, `JSONResponse` is useful when you need **full control** over the response.

---

## Example Application

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI, status
from fastapi.responses import JSONResponse

app = FastAPI()


@app.get("/")
def home():
    return JSONResponse(
        content={"msg": "API is working"},
        status_code=status.HTTP_200_OK
    )
```

---

## Why Use `JSONResponse`

FastAPI automatically converts Python dictionaries into JSON responses.
However, `JSONResponse` is useful when you need to:

* Explicitly control the response type
* Set custom status codes dynamically
* Customize headers
* Return non-standard JSON structures
* Override default response behavior

---

## Response Behavior

### Endpoint

```http
GET /
```

### Response Body

```json
{
  "msg": "API is working"
}
```

### HTTP Status Code

```
200 OK
```

---

## Comparison: Default Response vs JSONResponse

### Default FastAPI Response

```python
@app.get("/")
def home():
    return {"msg": "API is working"}
```

* Automatically serialized to JSON
* Simpler and recommended for most cases

---

### Explicit JSONResponse

```python
return JSONResponse(
    content={"msg": "API is working"},
    status_code=status.HTTP_200_OK
)
```

* Explicit control over response
* Useful for advanced use cases

---

## When to Use `JSONResponse`

* Returning conditional status codes
* Adding custom headers
* Returning responses outside standard request flow
* Building middleware or exception handlers
* Integrating with legacy systems

---

## Running the Application

Start the application using `uvicorn`:

```bash
uvicorn main:app --reload
```

---

## Best Practices

* Prefer returning dictionaries for simple APIs
* Use `JSONResponse` only when additional control is required
* Keep response formats consistent across endpoints
* Avoid mixing response styles unnecessarily
* Use response models for structured APIs

