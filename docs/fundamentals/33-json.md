# 33 – JSON in Python

This document explains **JSON** (JavaScript Object Notation) — the standard text format for exchanging data between clients and servers. FastAPI APIs almost always send and receive JSON.

Read [03-list.md](03-list.md) and [13-dict.md](13-dict.md) (dictionaries and lists) before this chapter.

---

## 1. What Is JSON?

JSON is a plain-text format for structured data. It is language-independent, but its structure maps closely to Python dictionaries and lists.

Example JSON:

```json
{
  "name": "abbas",
  "age": 20,
  "active": true,
  "tags": ["admin", "user"]
}
```

| JSON type | Python equivalent | Example |
|-----------|-------------------|---------|
| object | `dict` | `{"name": "abbas"}` |
| array | `list` | `[1, 2, 3]` |
| string | `str` | `"hello"` |
| number | `int` or `float` | `42`, `3.14` |
| boolean | `bool` | `true` / `false` |
| null | `None` | `null` |

---

## 2. Python Dict ↔ JSON

### Python dictionary

```python
user = {"name": "abbas", "age": 20}
```

### Same data as JSON (text)

```json
{"name": "abbas", "age": 20}
```

### List of objects

Python:

```python
users = [
    {"name": "abbas", "age": 20},
    {"name": "mmd", "age": 37},
]
```

JSON:

```json
[
  {"name": "abbas", "age": 20},
  {"name": "mmd", "age": 37}
]
```

FastAPI automatically converts Python `dict` and `list` return values into JSON responses. You do not need to call `json.dumps()` in most endpoints.

---

## 3. The `json` Module

Python's standard library includes `json` for manual conversion.

### Python → JSON string

```python
import json

user = {"name": "abbas", "age": 20}
text = json.dumps(user)
print(text)  # {"name": "abbas", "age": 20}
```

### JSON string → Python

```python
import json

text = '{"name": "abbas", "age": 20}'
user = json.loads(text)
print(user["name"])  # abbas
```

| Function | Direction |
|----------|-----------|
| `json.dumps(obj)` | Python object → JSON string |
| `json.loads(text)` | JSON string → Python object |

---

## 4. Content-Type Header

When a client sends JSON in an HTTP request, it sets:

```http
Content-Type: application/json
```

When a server responds with JSON, it typically returns:

```http
Content-Type: application/json
```

FastAPI sets this header automatically when you return a dictionary or use a Pydantic model.

---

## 5. JSON Request Body

A **request body** is data sent by the client in the HTTP message (not in the URL). For REST APIs, the body is usually JSON.

Example request:

```http
POST /new_user HTTP/1.1
Content-Type: application/json

{"name": "ali", "age": 25}
```

In FastAPI, you define the expected shape with a Pydantic model (see [36-pydantic-basics.md](36-pydantic-basics.md)) or type hints. FastAPI parses the JSON body and validates it before your function runs.

---

## 6. JSON Response Body

A **response body** is data the server sends back.

Example response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{"message": "User created successfully"}
```

In FastAPI:

```python
@app.get("/")
def home():
    return {"message": "Home Page"}
```

The returned dictionary becomes JSON automatically.

---

## 7. JSON vs Other Formats

| Format | Content-Type | Typical use |
|--------|--------------|-------------|
| JSON | `application/json` | REST APIs (default for FastAPI) |
| Form data | `application/x-www-form-urlencoded` | HTML forms |
| Multipart | `multipart/form-data` | File uploads |
| XML | `application/xml` | Legacy enterprise systems |
| Plain text | `text/plain` | Simple messages |

See [34-http-basics.md](34-http-basics.md) for how these fit into HTTP requests.

---

## 8. Common Mistakes

### Single quotes in JSON

JSON requires double quotes for strings. This is **invalid** JSON:

```json
{'name': 'abbas'}
```

This is **valid**:

```json
{"name": "abbas"}
```

Python dictionaries can use single quotes; JSON cannot.

### Trailing commas

Invalid in JSON:

```json
{"name": "abbas",}
```

Valid in Python 3, but not in JSON.

---

## Summary

* JSON is the standard data format for web APIs.
* JSON objects map to Python dictionaries; JSON arrays map to Python lists.
* Use `json.dumps()` and `json.loads()` for manual conversion.
* FastAPI serializes Python dicts/lists to JSON responses automatically.
* API request bodies are usually JSON with `Content-Type: application/json`.
