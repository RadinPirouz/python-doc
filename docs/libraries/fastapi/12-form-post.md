# FastAPI – POST Requests with Form Data

## Prerequisites

* [11-post-types](11-post-types.md) — form `Content-Type` overview
* [12 – HTTP basics](../../fundamentals/12-http-basics.md) — POST request body
* [10 – Type hints](../../fundamentals/10-type-hints.md) — parameter defaults

This document demonstrates how to handle **form-based POST requests** in FastAPI using the `Form()` dependency.
Form data is commonly used in **HTML forms**, authentication flows, and legacy systems.

---

## Example Application

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI, Form, status
from fastapi.responses import JSONResponse

app = FastAPI()

users_db = [
    {"id": "1", "name": "radin", "password": "123"}
]


@app.post("/user/")
def get_user_from_form(
    target: int = Form(
        ...,
        gt=0,
        alias="User ID",
        description="Enter target unique ID",
    )
):
    for item in users_db:
        if item["id"] == str(target):
            return JSONResponse(
                content={"msg": f"Your target user name is {item['name']}"},
                status_code=status.HTTP_200_OK,
            )

    return JSONResponse(
        content={"msg": "user not found"},
        status_code=status.HTTP_404_NOT_FOUND,
    )
```

---

## Form Data Overview

### What is Form Data

Form data is sent in the request body using:

```
Content-Type: application/x-www-form-urlencoded
```

or

```
multipart/form-data
```

FastAPI requires the `Form()` dependency to explicitly declare form inputs.

---

## Endpoint Behavior

### Endpoint

```http
POST /user/
```

### Request Body (Form Data)

```
User ID=1
```

### Example Using `curl`

```bash
curl -X POST "http://localhost:8000/user/" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "User ID=1"
```

---

## Response Examples

### Success Response

```json
{
  "msg": "Your target user name is radin"
}
```

Status Code: **200 OK**

---

### Failure Response

```json
{
  "msg": "user not found"
}
```

Status Code: **404 Not Found**

---

## `Form()` Parameter Configuration

```python
Form(
    ...,
    gt=0,
    alias="User ID",
    description="Enter target unique ID"
)
```

| Parameter     | Description                     |
| ------------- | ------------------------------- |
| `...`         | Field is required               |
| `gt=0`        | Value must be greater than zero |
| `alias`       | Custom form field name          |
| `description` | Displayed in API docs           |

---

## Swagger / OpenAPI Behavior

* Form fields appear as input fields
* Aliases are reflected in the UI
* Validation rules are enforced automatically
* Errors are returned with clear messages

---

## Running the Application

Start the service using `uvicorn`:

```bash
uvicorn main:app --reload
```

---

## Best Practices

* Use form data only when required (e.g. HTML forms)
* Prefer JSON for APIs and services
* Avoid exposing sensitive fields in plain form data
* Use HTTPS for all form submissions
* Validate and sanitize all inputs
* Use authentication and hashing for passwords
* Do not store credentials in plain text

