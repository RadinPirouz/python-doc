# FastAPI – HTML Templates with Jinja2

## Prerequisites

* [03-get-method](03-get-method.md) — path parameters (`/items/{id}`)
* [35 – Async/await](../../fundamentals/35-async-await.md) — `async def` endpoints
* [13 – Dictionaries](../../fundamentals/13-dict.md) — dictionaries for template `context`

## Overview

FastAPI can return HTML pages instead of JSON by using a template engine. The built-in integration uses **Jinja2**, which lets you render dynamic HTML from template files and pass Python variables into the page.

This is useful when you need:

```text
Server-rendered HTML pages
Admin dashboards
Simple web UIs alongside your API
Reusable page layouts with shared headers and footers
```

---

# 1. Required Package

Install Jinja2:

```bash
pip install jinja2
```

If you use the standard FastAPI install, it may already be included:

```bash
pip install "fastapi[standard]"
```

---

# 2. Project Structure

A typical layout separates templates from application code:

```text
project/
├── main.py
└── templates/
    └── item.html
```

The `directory` argument in `Jinja2Templates` must point to the folder that contains your `.html` files.

---

# 3. Basic Example

Create `main.py`:

```python
from fastapi import FastAPI, Request
from fastapi.responses import HTMLResponse
from fastapi.templating import Jinja2Templates

app = FastAPI()

templates = Jinja2Templates(directory="templates")


@app.get("/items/{id}", response_class=HTMLResponse)
async def read_item(request: Request, id: str):
    return templates.TemplateResponse(
        request=request,
        name="item.html",
        context={"id": id},
    )
```

Create `templates/item.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Item {{ id }}</title>
</head>
<body>
    <h1>Item ID: {{ id }}</h1>
</body>
</html>
```

Run the application:

```bash
uvicorn main:app --reload
```

Open `http://127.0.0.1:8000/items/42` in your browser to see the rendered page.

---

# 4. Code Explanation

## Import Required Modules

```python
from fastapi import FastAPI, Request
from fastapi.responses import HTMLResponse
from fastapi.templating import Jinja2Templates
```

- `Request` — required by `TemplateResponse` so templates can access request data.
- `HTMLResponse` — tells FastAPI the endpoint returns HTML.
- `Jinja2Templates` — loads and renders Jinja2 template files.

---

## Configure the Template Engine

```python
templates = Jinja2Templates(directory="templates")
```

This creates a template loader that looks for `.html` files inside the `templates/` directory.

---

## Return a Rendered Template

```python
@app.get("/items/{id}", response_class=HTMLResponse)
async def read_item(request: Request, id: str):
    return templates.TemplateResponse(
        request=request,
        name="item.html",
        context={"id": id},
    )
```

| Argument | Purpose |
|----------|---------|
| `request` | The incoming HTTP request (required) |
| `name` | Template filename relative to the templates directory |
| `context` | Dictionary of variables available inside the template |

Inside `item.html`, variables from `context` are accessed with Jinja2 syntax: `{{ id }}`.

---

# 5. Passing Multiple Variables

You can pass any data the template needs through `context`:

```python
@app.get("/users/{user_id}", response_class=HTMLResponse)
async def user_profile(request: Request, user_id: int):
    return templates.TemplateResponse(
        request=request,
        name="user.html",
        context={
            "user_id": user_id,
            "username": "abbas",
            "is_active": True,
        },
    )
```

Example template:

```html
<h1>Profile: {{ username }}</h1>
<p>User ID: {{ user_id }}</p>

{% if is_active %}
    <span>Active</span>
{% else %}
    <span>Inactive</span>
{% endif %}
```

---

# 6. Template Inheritance

Jinja2 supports base layouts so you do not repeat HTML boilerplate in every file.

`templates/base.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}App{% endblock %}</title>
</head>
<body>
    {% block content %}{% endblock %}
</body>
</html>
```

`templates/item.html`:

```html
{% extends "base.html" %}

{% block title %}Item {{ id }}{% endblock %}

{% block content %}
    <h1>Item ID: {{ id }}</h1>
{% endblock %}
```

---

# 7. Static Files

Templates often reference CSS, JavaScript, or images. Mount a static files directory alongside your templates:

```python
from fastapi.staticfiles import StaticFiles

app.mount("/static", StaticFiles(directory="static"), name="static")
```

Reference static assets in a template:

```html
<link rel="stylesheet" href="{{ url_for('static', path='style.css') }}">
```

---

# 8. Summary

| Step | Action |
|------|--------|
| 1 | Install `jinja2` |
| 2 | Create a `templates/` directory with `.html` files |
| 3 | Initialize `Jinja2Templates(directory="templates")` |
| 4 | Return `templates.TemplateResponse(...)` from your route |
| 5 | Pass dynamic data through the `context` dictionary |

For most API-only projects, JSON responses are enough. Use templates when you need server-rendered HTML in the same FastAPI application.
