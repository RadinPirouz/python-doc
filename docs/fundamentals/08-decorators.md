# 08 – Decorators in Python

This document explains **decorators**, how they work, and how they are used to extend function behavior without modifying the original function code.

---

## 1. What Is a Decorator?

A **decorator** is a function that:

* Takes another function as input
* Adds extra behavior
* Returns a new function

Decorators are commonly used for:

* Input validation
* Logging
* Authentication
* Performance measurement
* Access control

---

## 2. Basic Decorator Structure

A decorator has three layers:

1. The decorator function
2. The wrapper function
3. The original function

### General Pattern

```python
def decorator(func):
    def wrapper(*args, **kwargs):
        # extra behavior
        return func(*args, **kwargs)
    return wrapper
```

---

## 3. Example: Input Validation Decorator

### Code

```python
def check_number(func):
    def wrapper(a, b):
        if not (isinstance(a, (int, float)) and isinstance(b, (int, float))):
            print("Input must be numbers")
            return
        return func(a, b)
    return wrapper
```

### Explanation

* `check_number` is the decorator.
* `func` is the function being decorated.
* `wrapper` replaces the original function.
* `a` and `b` are the arguments passed to the original function.
* `isinstance(a, (int, float))` ensures inputs are numeric.
* If validation fails, execution stops.
* If validation passes, the original function is called.

---

## 4. Using the Decorator with `@` Syntax

### Code

```python
@check_number
def bemola(a, b):
    try:
        res = a / b
        print(res)
    except ZeroDivisionError:
        print("Zero Number Detected")
    except Exception as e:
        print(f"Error Detected {e}")
```

### What Happens Internally

This line:

```python
@check_number
```

Is equivalent to:

```python
bemola = check_number(bemola)
```

The function `bemola` is replaced by `wrapper`.

---

## 5. Execution Flow

When calling:

```python
bemola(10, 2)
```

The flow is:

1. `wrapper(10, 2)` is called
2. Inputs are validated
3. `func(10, 2)` is executed
4. Result is printed

If calling:

```python
bemola(10, "a")
```

The output will be:

```text
Input must be numbers
```

---

## 6. Why Use Decorators?

Without decorators, input validation would need to be repeated in every function.

Decorators allow:

* Reusable logic
* Cleaner code
* Separation of concerns

---

## 7. Limitations in This Example

* The decorator only works with exactly two arguments.
* It does not preserve the original function’s metadata (`__name__`, `__doc__`).

---

## 8. Improved Version (Best Practice)

```python
from functools import wraps

def check_number(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        if not all(isinstance(x, (int, float)) for x in args):
            print("Input must be numbers")
            return
        return func(*args, **kwargs)
    return wrapper
```

### Improvements

* Supports any number of arguments
* Preserves function name and documentation
* More reusable and professional

---

## Summary

* Decorators modify function behavior without changing its code
* They wrap functions inside another function
* `@decorator` is syntactic sugar
* Commonly used for validation, logging, and access control
* Best practice is to use `*args`, `**kwargs`, and `functools.wraps`

---

## 9. Decorators in FastAPI

FastAPI uses decorators to register HTTP endpoints:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users")
def list_users():
    return []
```

`@app.get("/users")` tells FastAPI: "when a GET request arrives at `/users`, call `list_users`." The same pattern applies to `@app.post`, `@app.put`, and `@app.delete`.

See [FastAPI – Simple route](../libraries/fastapi/02-simple-route.md) for a full example.
