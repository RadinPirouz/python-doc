# FastAPI – Advanced Path and Query Parameters

## Prerequisites

* [10 – Type hints](../../fundamentals/10-type-hints.md) — `Annotated`, the ellipsis (`...`)
* [12 – HTTP basics](../../fundamentals/12-http-basics.md) — path vs query parameters
* [07-query-parameters](07-query-parameters.md) — basic query parameter patterns

This document demonstrates advanced usage of **path parameters** and **query parameters** in FastAPI, including:

* Validation rules
* Aliases
* Descriptions for OpenAPI
* Deprecation flags
* Clear separation of responsibilities between path and query parameters

---

## Example Application

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI, Query, Path, status
from fastapi.responses import JSONResponse

app = FastAPI()

users_db = [
    {"id": "1", "name": "radin"}
]


@app.get("/user/{target_id}")
def get_user_by_path(
    target_id: int = Path(
        ...,
        alias="User ID",
        description="Enter target unique ID",
        deprecated=True,
    )
):
    for item in users_db:
        if int(item["id"]) == target_id:
            return JSONResponse(
                content={"msg": f"Your target user name is {item['name']}"},
                status_code=status.HTTP_200_OK,
            )

    return JSONResponse(
        content={"msg": "user not found"},
        status_code=status.HTTP_404_NOT_FOUND,
    )


@app.get("/user")
def get_user_by_query(
    target: int | None = Query(
        default=None,
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

## Path Parameter Example

### Endpoint

```http
GET /user/{target_id}
```

**Example Request:**

```
/user/1
```

### Characteristics

* Always required
* Defined as part of the route
* Used to identify a specific resource
* Missing value results in **404 Not Found**
* Supports validation, aliases, and documentation metadata

---

## Query Parameter Example

### Endpoint

```http
GET /user?User%20ID=1
```

### Characteristics

* Optional by default
* Used for filtering and searching
* Not part of the route path
* Supports validation and documentation metadata
* Uses default values when not provided

---

## Advanced Parameter Configuration

### `Path()` Example

```python
Path(
    ...,
    alias="User ID",
    description="Enter target unique ID",
    deprecated=True
)
```

* `...` → parameter is required
* `alias` → custom name in documentation and URL
* `description` → shown in Swagger UI
* `deprecated` → marked as deprecated in OpenAPI

---

### `Query()` Example

```python
Query(
    default=None,
    gt=0,
    alias="User ID",
    description="Enter target unique ID"
)
```

* `gt=0` → value must be greater than zero
* Optional parameter with validation rules
* Appears in API documentation

---

## Path vs Query Parameters

See [12 – HTTP basics](../../fundamentals/12-http-basics.md#4-path-parameters-vs-query-parameters) for the general rules. FastAPI-specific details below.

### 1. Path Parameters

**What they are**
Values embedded directly in the URL path that identify a specific resource.

**When to use**
When the parameter is mandatory and uniquely identifies a resource.

**Example URL**

```
/users/42
```

**FastAPI Example**

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}
```

**Key Points**

* Always required
* Part of the route definition
* Used for IDs and unique identifiers
* Missing value → 404 error

---

### 2. Query Parameters

**What they are**
Key-value pairs appended to the URL after `?`.

**When to use**
For optional filtering, searching, sorting, and pagination.

**Example URL**

```
/users?limit=10&active=true
```

**FastAPI Example**

```python
@app.get("/users")
def list_users(limit: int = 10, active: bool = True):
    return {"limit": limit, "active": active}
```

**Key Points**

* Optional by default
* Not part of the route path
* Used to modify or filter results
* Defaults are used when missing

---

## Side-by-Side Comparison

| Feature       | Path Parameter      | Query Parameter                   |
| ------------- | ------------------- | --------------------------------- |
| Location      | Inside URL path     | After `?`                         |
| Required      | Yes                 | No (by default)                   |
| Purpose       | Identify a resource | Filter or modify results          |
| Example       | `/items/5`          | `/items?limit=10`                 |
| Missing Value | 404 Not Found       | Default value or validation error |

---

## Best Practices

* Use path parameters for resource identification
* Use query parameters for filtering and modifiers
* Avoid spaces in parameter aliases for production APIs
* Deprecate parameters instead of removing them
* Use validation (`gt`, `lt`, `regex`) for safer APIs
* Keep URL design consistent across services
* Prefer response models over manual JSON responses when possible

