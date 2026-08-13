# 27 – Exception Handling

Python uses `try / except` blocks to handle runtime errors gracefully without crashing the program.

## Basic try / except

```python
def abbas(a, b):
    try:
        res = a / b
        print(res)
    except ZeroDivisionError:
        print("Zero Number Detected")
    except Exception as e:
        print(f"Error Detected {e}")

abbas(1, 0)
```

### Key Rule

* Always catch **specific exceptions first**.
* Use `Exception` only as a fallback.

## Full try / except Structure

```python
try:
    # code that may raise an error
except ZeroDivisionError:
    # handles division by zero
except Exception as e:
    # handles any other error
else:
    # runs if no error occurs
finally:
    # always runs (cleanup)
```

| Block | When it runs |
|-------|-------------|
| `try` | Code that may fail |
| `except` | When an exception is raised |
| `else` | When no exception occurred |
| `finally` | Always — used for cleanup |

## Raising Exceptions

```python
def set_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative")
    return age
```

## Best Practice

Functions should **return values**, not print them, so they can be tested and reused.

```python
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return "Zero Number Detected"
```
